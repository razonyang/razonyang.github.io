---
date: 2017-04-16T12:25:48+08:00
comment: true
categories: ["development", "linux"]
tags: ["linux", "pushd", "popd"]
title: "Linux 切换目录小技巧"
---

在 Linux 终端，经常需要进行目录间的切换，有时候，我们需要切换至某个目录操作，完事后切换回原本目录，
用 `cd` 难免有些繁琐，此时 `pushd` 和 `popd` 就很方便了。
<!--more-->

# 命令

| 命令   | 说明                                    |
|:-----:|:--------------------------------------:|
| pushd | 将当前目录压入一个“虚拟的堆栈”，并切换到目标目录 |
| popd  | 弹出堆栈顶部的目录，并切换至该目录            |
| dirs  | 显示堆栈中的目录列表                       |


# 示例

| Command               | Explanation                                                | Output                    |
|:----------------------|:-----------------------------------------------------------|:--------------------------|
| `dirs`                | 显示当前目录栈中的所有记录                                      | ~                         |
| `pushd ~/Downloads`   | 进入 `Downloads` 目录，从输出可以看到，`Downloads` 目录已经压入栈中 | ~/Downloads ~            |
| `pushd ~/Documents/`  | 进入 `Documents` 目录                                        | ~/Documents ~/Downloads ~ |
| `popd`                | 回退到上一个目录，也就是 `Downloads` 目录                        | ~/Downloads ~             |
| `popd`                |回退到上上个目录，也就是 `～` 目录                                 | ~                        |