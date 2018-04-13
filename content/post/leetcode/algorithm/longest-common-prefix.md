---
date: "2017-07-11T09:52:01+08:00"
comment: true
categories: ["algorithm", "leetcode"]
tags: ["leetcode", "algorithm", "golang"]
title: "Leetcode 算法题 - Longest Common Prefix"
---

编写一个方法返回字符串数组的最长公共前缀。
<!--more-->

## 原题

Write a function to find the longest common prefix string amongst an array of strings.


## 分析

依据题意，需要注意以下几点：

- 区分大小写

- 空数组的处理


## 实现

完整代码和测试已托管在 [go-algorithm](https://github.com/razonyang/go-algorithm)。

```
func longestCommonPrefix(strs []string) string {
	if len(strs) == 0 {
		return ""
	}

	max := len(strs[0])

	for i := 1; i < len(strs); i++ {
		max = min(max, len(strs[i]))
		for j := 0; j < max; j++ {
			if strs[i][j] != strs[i-1][j] {
				max = j
				continue
			}
		}
	}

	return strs[0][:max]
}

func min(a, b int) int {
	if a > b {
		return b
	}

	return a
}
```