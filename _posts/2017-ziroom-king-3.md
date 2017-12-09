---
title: 王者编程大赛之三 — 01背包
date: 2017-12-06 13:22:00
tags:
- 算法
- 数据结构
categories:
- 算法
---

服务目前每月会对搬家师傅进行评级，根据师傅的评级排名结果，我们将优先保证最优师傅的全天订单。

假设师傅每天工作 8 个小时，给定一天 n 个订单，每个订单其占用时间长为 $T_i$，挣取价值为 $V_i$，现请您为师傅安排订单，并保证师傅挣取价值最大。
![]()<!--more-->

输入格式
输入 n 组数据，每组以逗号分隔并且每一个订单的编号，时长，挣取价值，数字以空格分隔
输出格式
输出争取价值，订单编号，订单编号按照价值由大到小排序，争取价值相同，则按照每小时平均争取价值由大到小排序

示例：
输入：[MV10001 2 100,MV10002 3 120,MV10003 1 200,MV10004 3 200,MV10005 4 70,MV10006 3 120,MV10007 2 10,MV10008 2 30,MV10009 6 500,MV10010 3 400]
输出：800 MV10010 MV10003 MV10004
输入：[M10001 2 100,M10002 3 210,M10003 3 300,M10004 2 150,M10005 1 70,M10006 2 220,M10007 1 10,M10008 3 30,M10009 3 200,M10010 2 400]
输出：990 M10010 M10003 M10006 M10005

## 解题思路

本题是 01 背包问题，典型的 [动态规划](https://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741374.html) 实现。动态规划

https://www.cnblogs.com/Christal-R/p/Dynamic_programming.html

## 编码实现

## 总结

动态规划的其他 [典型应用](https://www.cnblogs.com/wuyuegb2312/p/3281264.html)。