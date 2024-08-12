# Why is it so hard to get Concurrency right

## Race Condition
It is a condition when two or more process must executed in correct order but the code is not guaranteed to do so.

Example : when one concurrent process try to write a data in a variable while another process try to read the same variable

```go
var data int

go func(){
    data++
}()

if data == 0 {
    fmt.Printf("the value is 0.\n")
} else {
    fmt.Printf("the value is %v.\n", data)
}
```

## Memory Access Synchoronization

There's name for a section of your program that needs exclusive access to shared resource. This is called Critical section

1. Our goroutine which is incrementing the data variables
1. Our if statement which checkc whether the value of data is 0
1. Our fmt.Printf statement which retrieeve the value of data for ouput.

the code below is not idiomatic Go (i don't suggest you to solve your data race like this).

```go

var memoryAccess sync.Mutex
var value int
go func(){
    memoryAccess.Lock()
    value++
    memoryAcess.Unlock()
}()

memoryAccess.Lock()
if data == 0 {
    fmt.Printf("the value is 0.\n")
} else {
    fmt.Printf("the value is %v.\n", data)
}
memoryAcesss.Unlock()
```

## Deadlock, Livelock and Starvation

These issue all concern ensuring your program has something useful to do at all times. If not handled properly, your program could enter a state in which it will stop functioning all together.


### Deadlock
A deadlock is a program which all concurrent procceses are waiting on onether. In this state, the program will never recover without outside intervention.

