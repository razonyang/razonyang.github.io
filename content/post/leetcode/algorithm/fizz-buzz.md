---
date: "2017-07-08T20:53:41+08:00"
comment: true
categories: ["algorithm", "leetcode"]
tags: ["golang", "leetcode", "algorithm"]
title: "Leetcode 算法题 - Fizz Buzz"
---

Leetcode 算法题 - Fizz Buzz

编写一个程序，用字符串表示数字 1 到 n，其中：

- 如果数字是**3**的倍数，则输出**Fizz**

- 如果数字是**5**的倍数，则输出**Buzz**

- 如果数字是**3**和**5**的倍数，则输出**FizzBuzz**
<!--more-->

# 原题

Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

Example:

```
n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

# 实现

完整代码和测试已托管在 [go-algorithm](https://github.com/razonyang/go-algorithm)。

## Go 语言版①

```
func fizzBuzz(n int) []string {
	ret := make([]string, n)

	for i := 1; i <= n; i++ {
		if i%15 == 0 {
			ret[i-1] = "FizzBuzz"
		} else if i%5 == 0 {
			ret[i-1] = "Buzz"
		} else if i%3 == 0 {
			ret[i-1] = "Fizz"
		} else {
			ret[i-1] = strconv.Itoa(i)
		}
	}

	return ret
}
```

## Go 语言版②

```
func fizzBuzz(n int) []string {
	ret := make([]string, n)
	three := 3
	five := 5
	fifteen := 15

	for i := 1; i <= n; i++ {
		if i == fifteen {
			ret[i-1] = "FizzBuzz"
			fifteen += 15
			five += 5
			three += 3
		} else if i == five {
			ret[i-1] = "Buzz"
			five += 5
		} else if i == three {
			ret[i-1] = "Fizz"
			three += 3
		} else {
			ret[i-1] = strconv.Itoa(i)
		}
	}

	return ret
}
```

> 经过测试，两个版本的性能差不多。