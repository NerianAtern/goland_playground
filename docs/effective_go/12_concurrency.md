### Concurrency
Go has a different approach of concurrency: Shared values are passed around on channels and are never actively shared by separate threads of execution. Only one goroutine has access to the value at any given time. No data races can occur by design.
*Do not communicate by sharing memory; instead, share memory by communicating.*

Goroutines:
Goroutines are functions executing concurrently with other goroutines in the same address space. This costs only the allocation of a stack space.
- goroutines are multiplexed onto multiple OS threads
- functions can be prefixed by `go` keyword to start goroutine
- when call finishes, goroutine exists silently
```
func Announce(message string, delay time.Duration) {
    go func() {
        time.Sleep(delay)
        fmt.Println(message)
    }() // Note the parentheses - must call the function.
}
```

Channels:
Like aps, channels are a reference type. If an optional integer parameter is provided, it sets the buffer size for the channel.
```
ci := make(chan int) // unbuffered channel of integers
cj := make(chan int, 0) // unbuffered channel of integers
cs := make(chan *os.File, 100) // buffered channel of pointers to Files
```
- channels combine communication (value exchange) with synchronization
```
c := make(chan int) // Allocate a channel.
// Start the sort in a goroutine; when it completes, signal on the channel.
go func() {
    list.Sort()
    c <- 1 // Send a signal; value does not matter.
}()
doSomethingForAWhile()
<-c // Wait for sort to finish; discard sent value.
```
- receivers always block until data is received
- unbuffered channels block until receiver has value
- buffered channels block until value is set in buffer. If the buffer is full, this means waiting until receiver retrievs value

Channels of channels:
Channels can be passed around like any value, e.g. for safe, parallel demultiplexing. (channels are *first-class citizens*!)

Parallelization:
Each goroutine can have a different channel. We block the further execution of the code until all channels are drained. All goroutines live in the same address space, so this can be realized by using slices. 

A leaky buffer:
Constantly allocating memory for data streams and letting the garbage collector clean it up creates massive performance bottlenecks. 
- set an upper limit for the memory pool (number of buffers)
- client picks up a buffer from a list if not empty, otherwise it creates a new one
- the server puts the buffer back into the buffer list, or drops it if the list is full (it *leaks*). The garbage collector takes care of it
- the pool with never grow larger than the predefined cap, but if required, enough buffers will be created to process something
```
var freeList = make(chan *Buffer, maxFreeBuffers) // The Tray Stack (Buffered Channel)
var serverChan = make(chan *Buffer)              // Incoming Server Queue

func client() {
    for {
        var b *Buffer
        
        // Grab an existing buffer, or make a new one if empty
        select {
        case b = <-freeList:
            // Got one from the stack! Zero allocation cost.
        default:
            // The stack was empty, allocate a brand new one.
            b = new(Buffer)
        }
        
        loadData(b)
        serverChan <- b
    }
}

func server() {
    for {
        b := <-serverChan
        process(b)
        
        // Recycle the buffer, or let it leak if the stack is full
        select {
        case freeList <- b:
            // Put it back on the stack for the next client to reuse.
        default:
            // The stack is full! Just carry on. 
            // The buffer "leaks" out and the GC will eventually clean it up.
        }
    }
```