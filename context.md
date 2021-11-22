# context.Context

context.Context belongs to the Golang standard library, it is a package like all the others and it is not a core part of the language specification. The simplest use of `context.Context` is to carry and propagate termination signals.

Usually context.Context is used when there are slow operations or operations that may block, in the path of doing something useful.

Those kinds of operations are typically IO related operations, for instance reading from disk, reading from network, calling up a database, etc.

Basic requirements of `context.Context` are:

1. If a function takes a `context.Context` as an argument, it should be the function's first argument.
2. `context.Context` should be bound to the ctx identifier.
3. Do not store a `context.Context` in a structure, but pass it around to any function that may need it

Example of context.Context for cancellation:
```go
select {
    case <-ctx.Done():
        // the operation should be cancelled
        // the simplest way it is to just return
        return ctx.Err()
    case result <- resultCh:
        // we got some result to use
}
```

There are three ways to cancel a context:
1. WithCancel:
```go
ctx, cancel := context.WithCancel(context.Background())
defer cancel()

```
2. WithDeadline:
```go
ctx, cancel := context.WithDeadline(
    context.Background(),
    time.Now().Add(500*time.Millisecond))
defer cancel()
```
3. WithTimeout:
```go
ctx, cancel := context.WithDeadline(
    context.Background(),
    time.Now().Add(500*time.Millisecond))
defer cancel()
```

## A full example of handling context.Context with error handling

1. Set up a channel to get the result of the blocking operation
2. Do the blocking part in another goroutine and send the result to the channel set up before
3. In the main control flow, listen to both channels, the result channel and the ctx.Done() channel
4. Avoid leaking goroutines by finding a way to stop the blocking goroutine. Usually, this involves calling something like the .Close() method.

```go
package main

import (
	"context"
	"errors"
	"fmt"
	"time"
	"math/rand"
)

func Send() bool {
	time.Sleep(5 * time.Second)
	return true
}

func main() {

	// obtain a first background
	sendChan := make(chan bool, 1)

	ctx := context.Background()
	ctx, cancel := context.WithCancel(ctx)
	defer cancel()

	go func(ctx context.Context, sendChan chan<- bool) {
		select {
		case <-ctx.Done():
			close(sendChan)
		case sendChan <- Send():
		}

		sendChan <- Send()
	}(ctx, sendChan)

	go func() {
		max := 10
		min := 1
		ramdNum := rand.Intn(max - min) + min
		fmt.Println(ramdNum)
		time.Sleep(2 * time.Second)
		if ramdNum > 5 {
			cancel()
		}

	}()

	var err error
	var result bool

	select {
	case <-ctx.Done():
		err = ctx.Err()
	case result = <-sendChan:
	}

	if err != nil {
		switch {
		case errors.Is(err, context.Canceled):
			fmt.Println("The context was cancelled")
		case errors.Is(err, context.DeadlineExceeded):
			fmt.Println("The deadline of the context expired")
		}

		return
	}

	fmt.Println("no cancel is occured")
	fmt.Println(result)
}
```

## Store values in context.Context
You can set the value of a specific key using the `context.WithValue(ctx context.Context, key, value interface{})`

Example:
```go
package main

import (
    "context"
    "fmt"
)

type keyOne string
type keyTwo string

func main() {
    ctx := context.Background()
    ctx = context.WithValue(ctx, keyOne("one"), "valueOne")
    ctx = context.WithValue(ctx, keyTwo("one"), "valueTwo")

    fmt.Println(ctx.Value(keyOne("one")).(string))
    fmt.Println(ctx.Value(keyTwo("one")).(string))
}
```