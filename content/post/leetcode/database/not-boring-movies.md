---
date: "2017-07-13T20:18:25+08:00"
comment: true
categories: ["database", "leetcode"]
tags: ["database", "mysql", "leetcode"]
title: "Leetcode 数据库面试题 - Not Boring Movies"
---

某城市新开了一家影院，不少人很喜欢去这家影院。该影院也张贴了关于的电影的评级和描述的海报。

请写出一条 SQL，返回 id 为奇数，且 description 不是 "boring" 的电影，并按 rating 降序。
<!--more-->

# 原题

X city opened a new cinema, many people would like to go to this cinema. The cinema also gives out a poster indicating the movies’ ratings and descriptions.

Please write a SQL query to output movies with an odd numbered ID and a description that is not 'boring'. Order the result by rating.

For example, table cinema:

|   id    | movie     |  description |  rating   |
|:-------:|:---------:|:------------:|:---------:|
|   1     | War       |   great 3D   |   8.9     |
|   2     | Science   |   fiction    |   8.5     |
|   3     | irish     |   boring     |   6.2     |
|   4     | Ice song  |   Fantacy    |   8.6     |
|   5     | House card|   Interesting|   9.1     |

For the example above, the output should be:

|   id    | movie     |  description |  rating   |
|:-------:|:---------:|:------------:|:---------:|
|   5     | House card|   Interesting|   9.1     |
|   1     | War       |   great 3D   |   8.9     |


# SQL

```
SELECT * 
FROM cinema
WHERE description != 'boring' AND id % 2 = 1
ORDER BY rating DESC
```