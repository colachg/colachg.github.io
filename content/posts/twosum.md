+++ 
draft = false
date = 2019-11-07T11:29:45+08:00
title = "TwoSum"
slug = "/twosum" 
tags = []
categories = ["LeetCode"]
+++

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the same element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## Write the test first

```go
func TestTwoSum(t *testing.T) {
	got := TwoSum([]int{2, 7, 11, 15}, 9)
	expected := [2]int{0, 1}

	if got != expected {
		t.Errorf("expected '%v' but got '%v'", got, expected)
	}
}
```

## Try to run the test

Run the test `go test`

Inspect the compilation error

```shellscript
./sum_test.go:6:9: undefined: TwoSum
```

## Write the minimal amount of code for the test to run and check the failing test output

Write enough code to satisfy the compiler and that's all - remember we want to check that our tests fail for the correct reason.

```go
// TwoSum ...
func TwoSum(nums []int, target int) [2]int {
	return [2]int{1, 2}
}
```

Now run the tests and we should be happy that the test is correctly reporting what is wrong.

`sum_test.go:10: expected '[1 2]' but got '[0 1]'`

## Write enough code to make it pass

In the strictest sense of TDD we should now write the minimal amount of code to make the test pass. A pedantic programmer may do this

```go
// TwoSum ...
func TwoSum(nums []int, target int) [2]int {
	return [2]int{0, 1}
}
```

Ah hah! Foiled again, TDD is a sham right?  
We could write another test, with some different numbers to force that test to fail but that feels like a game of cat and mouse.

For now, let's fix it properly

```go
// TwoSum ...
func TwoSum(numbers []int, target int) [2]int {
	for i := 0; i < len(numbers); i++ {
		for j := i + 1; j < len(numbers); j++ {
			if numbers[i]+numbers[j] == target {
				return [2]int{i, j}
			}
		}
	}
	return nil;
}
```

Do you want more sufficient confidence? Let's use [table driven tests](https://github.com/golang/go/wiki/TableDrivenTests) to build a list of test cases that can be tested in the same manner.

```go
// sum_test.go
package twosum

import (
	"testing"
)

func TestTwoSum(t *testing.T) {
	cases := []struct {
		numbers []int
		target  int
		result  [2]int
	}{
		{[]int{1, 2}, 3, [2]int{0, 1}},
		{[]int{2, 7, 11, 15}, 13, [2]int{0, 2}},
		{[]int{3, 7, 1, 15}, 4, [2]int{0, 2}},
		{[]int{2, 7, 11, 15}, 17, [2]int{0, 3}},
		{[]int{2, 7, 11, 15}, 26, [2]int{2, 3}},
		{[]int{2, 7, 11, 15}, 9, [2]int{0, 1}},
	}

	for _, test := range cases {
		got := TwoSum(test.numbers, test.target)
		expected := test.result
		if got != expected {
			t.Errorf("expected '%v' but got '%v'", expected, got)
		}
	}
}
```

## Refactor

For each element, we try to find its complement by looping through the rest of array which takes O(n) time.  
Therefore, the time complexity is O(n^2)

Space complexity : O(1).

To improve our run time complexity, we need a more efficient way to check if the complement exists in the array. If the complement exists, we need to look up its index.  
We reduce the look up time from O(n) to O(1) by trading space for speed.

```go
func TwoSum(nums []int, target int) [2]int {
	numMap := make(map[int]int)
	for index, num := range nums {
		if i, ok := numMap[target-num]; ok {
			return [2]int{i, index}
		}
		numMap[num] = index
	}
	return [2]int{-1, -1}
}
```

## Wrapping up

- Sometimes we should find the balance between time complexity and space complexity.
