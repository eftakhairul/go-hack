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