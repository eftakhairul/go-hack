# Timeout

It always better to handle timeout when you do any sort of network call.

Don't use HTTP client directly:
```go
var netClient = &http.Client{
  Timeout: time.Second * 10,
}

response, err := netClient.Get(url)

//handle err here
```
There is problem. As it's singelton. All request through this clinet will have the same timeout. Let's use context, this way you can set different timeout for different request.

```go
ctx, cancel := context.WithTimeout(context.Background(), time.Minute)
defer cancel()

req := req.WithContext(ctx)
res, err := c.Do(req)
```

What if we need to deal with a blocking function
```go
func SendValue(){
  sendChan := make(chan bool, 1)

  go func() {
    // send() is blocking function
    sendChan <- Send()
   }()

  select {
    case <-sendChan:
    case <-time.After(time.Minute):
      return
  }

  //continue logic in case send didn't timeout
}
func Send() bool {
	time.Sleep(5 * time.Second)
 	return true
}
```
Now, the above code is missing logic to cancel the Send() function and thus terminating the goroutine (otherwise this can cause a goroutine leak).
```go
func SendValue(){
  sendChan := make(chan bool, 1)
  done := make(chan struct{})

  go func() {
    select {
      case sendChan <- Send():
      case <-done:
          close(sendChan)
          return
    }
   }()

  select {
    case <-sendChan:
    case <-time.After(time.Minute):
      // terminate the go function
      done <- struct{}{}
      return
  }

  //continue logic in case send didn't timeout
}

func Send() bool {
  time.Sleep(5 * time.Second)
  return true
}
```
You can also implement this with context.WithCancel().