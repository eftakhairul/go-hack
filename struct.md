# Structs Initalization
In GO, you can initialize structs in multiple way with the `struct` keyword.

```Go
type Person struct {
  age  int
  name string
}

// struct literal
person := &Person{
  age:  25,
  name: "Anton",
}

// new build-in
person := new(Person) // person is of type *Person
person.age = 25
person.name = "Anton"


// Anonymous struct
anonStruct := struct {
  age  int
  name string
}{
  age:  25,
  name: "Anton",
}

// function with anonymous struct return statement
func Person() *struct{
  Age  int
  Name string
} {
  return &struct{
    Age  int
    Name string
  }{25, "Anton"}
}

// Variadic function constructor

type PersonOptionFunc func(*Person)

func WithName(name string) PersonOptionFunc {
  return func(p *Person) {
    p.name = name
  }
}

func WithAge(age int) PersonOptionFunc {
  return func(p *Person) {
    p.age = age
  }
}

func NewPerson(opts ...PersonOptionFunc) *Person {
  p := &Person{}
  for _, opt := range opts {
    opt(p)
  }
  return p
}

// usage:
p := people.NewPerson(people.WithName("Anton"), people.WithAge(25))

fmt.Println(p)
```