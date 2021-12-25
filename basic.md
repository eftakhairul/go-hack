### new
In Golang the keyword `new` is used to create a new instance of a type.
```go
type User struct {
    Name string
}
u := new(User)
```
Here, `new` returns pointer reference to the type: `User`.
This is same as:
```go
u := &User{}
```

### make
There is another keyword: `make` that also used to create a new instance of a type. However, it only works with slices, maps, and channel (chan). The reason is that slice, map and chan are data structures. They need to be initialized, otherwise they won't be usable.
```go
u := make([]User, 10)
```

### diff between new and make

`new(User)`: it returns a pointer to type `User` a value of type `*User`, it allocates and zeroes the memory.` new(User)` is equivalent to &User{}.

`make(User)`: it returns an initialized value of type `User`.It allocates and initializes the memory.

```go
p *[]int = new([]int) // *p = nil, which makes p useless
v []int = make([]int, 100) // creates v structure that has pointer to an array, length field, and capacity field. So, v is immediately usable.
```