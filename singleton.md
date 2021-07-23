# Thread safe singlon

If it can be singleton, then make it singleton, but do it right.
First attemp:
```go
var (
   mut sync.Mutex
   conn *DbConnection
)

func GetConnection() *DbConnection {
  if conn != nil {
    return conn
  }

  conn = &DbConnection{}
  return conn
}
```

Problem is it's not thread safe. If two function call at the same time, it willc create two connections.

Okay. Second attemp with Mutex Locks:
```go
type DbConnection struct {}

var (
   mut sync.Mutex
   conn *DbConnection
)

func GetConnection() *DbConnection {
    if conn == nil {
        mut.Lock()
        defer mut.Unlock()
        if conn == nil {
            conn = &DbConnection{}
        }
    }
    return conn
}
```
Still, it's not go way. Let's try with Sync package
```go
type DbConnection struct {}

var (
   dbOnce sync.Once
   conn *DbConnection
)

func GetConnection() *DbConnection {
    dbOnce.Do(func() {
        conn = &DbConnection{}
    }
    
    return conn
}
```

Sweet :)
