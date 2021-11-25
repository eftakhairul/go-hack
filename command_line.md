# All Helful Commands

## go run
```bash 
go run hello.go
```

`go run` compile the code, put the binary into temporary directoory, execute the binary from temporary directory and print the output to the console. After everything, it delete the binary.

## go build
```bash
go build hello.go
```
`go build` compile the code and put the binary into the current directory. Binary name is the same as the file name unless you specify the name with `-o` flag.

## go install
```bash
go install hello.go
```

`go install` does the same thing as `go build` but it put the binary into the package directory ($GOPATH/bin/). `go get` also does the same thing for third-party libraries.