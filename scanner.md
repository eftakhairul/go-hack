# Read Single Line

### bufio.Reader
```bash
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    fmt.Println("input text:")
    reader := bufio.NewReader(os.Stdin)
    line, err := reader.ReadString('\n')
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("read line: %s-\n", line)
}
```

### bufio.Scanner
```bash
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    fmt.Println("input text:")
    scanner := bufio.NewScanner(os.Stdin)
    scanner.Scan()
    err := scanner.Err()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("read line: %s-\n", scanner.Text())
}
```

### fmt.Scanln()
```bash
package main

import (
    "fmt"
    "log"
)

func main() {
    fmt.Println("input text:")
    var w1, w2, w3 string
    n, err := fmt.Scanln(&w1, &w2, &w3)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("number of items read: %d\n", n)
    fmt.Printf("read line: %s %s %s-\n", w1, w2, w3)
}
```

# Read multiple lines
### bufio.Reader
```bash

import (
    "bufio"
    "fmt"
    "log"
    "os"
    "strings"
)

func main() {
    fmt.Println("input text:")
    reader := bufio.NewReader(os.Stdin)

    var lines []string
    for {
        line, err := reader.ReadString('\n')
        if err != nil {
            log.Fatal(err)
        }
        if len(strings.TrimSpace(line)) == 0 {
            break
        }
        lines = append(lines, line)
    }

    fmt.Println("output:")
    for _, l := range lines {
        fmt.Println(l)
    }
}
```

###  bufio.Scanner
```bash
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    fmt.Println("input text:")
    scanner := bufio.NewScanner(os.Stdin)

    var lines []string
    for {
        scanner.Scan()
        line := scanner.Text()
        if len(line) == 0 {
            break
        }
        lines = append(lines, line)
    }

    err := scanner.Err()
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println("output:")
    for _, l := range lines {
        fmt.Println(l)
    }
}
```

### fmt.Scan()
```bash
package main

import (
    "fmt"
    "log"
)

func main() {
    fmt.Println("input text:")
    var w1, w2, w3 string
    n, err := fmt.Scan(&w1, &w2, &w3)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("number of items read: %d\n", n)
    fmt.Printf("read text: %s %s %s-\n", w1, w2, w3)
}
```

# Read a single character
### bufio.Reader
```bash
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    fmt.Println("input text:")
    reader := bufio.NewReader(os.Stdin)
    char, _, err := reader.ReadRune()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("read character: %c-\n", char)
}
```

### fmt.Scanf()
```bash
package main

import (
    "fmt"
    "log"
)

func main() {
    fmt.Println("input text:")
    var char rune
    _, err := fmt.Scanf("%c", &char)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("read character: %c-\n", char)
}
```

# Read formatted user input
```bash
package main

import (
    "fmt"
    "log"
)

func main() {
    fmt.Println("input text:")
    var name string
    var country string
    n, err := fmt.Scanf("%s is born in %s", &name, &country)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("number of items read: %d\n", n)
    fmt.Println(name, country)
}
```

# Read numbers from user input
```bash
package main

import (
    "fmt"
    "log"
)

func main() {
    fmt.Println("input number:")
    var number int64
    _, err := fmt.Scan(&number)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("read number: %d\n", number)
}
```