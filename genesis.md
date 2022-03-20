### Genesis

Example of Genensis
```GO
package main

import "fmt"

type Number interface {
	int | int32 | int64 | uint | uint32 | uint64 | float32 | float64
}

func BubbleSort[num Number](input []num) []num {

	isSwaped := true
	for isSwaped {
		isSwaped = false
		for i := 0; i < len(input)-1; i++ {
			if input[i] > input[i+1] {
				input[i], input[i+1] = input[i+1], input[i]
				isSwaped = true
			}
		}
	}
	return input
}

func main() {
	input := []int{4,5, 3, 2, 1}
	fmt.Println(BubbleSort(input))
}
```

What's the difference between interface and any? And why is it useful?

`func GenericFoo[T any](x, y T)` and `func InterfaceyFoo(x, y interface{})`

The difference with the generic version is you're still describing a specific type and what that means is we've still constrained this function to only work with one type. Here, in first function x & y are both the SAME TYPE. In the second function, x & y are both of type interface{} which means we can pass in ANY TYPE.