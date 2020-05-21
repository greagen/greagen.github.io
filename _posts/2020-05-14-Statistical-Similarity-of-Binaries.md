---
layout:     post
title:      Statistical Similarity of Binaries
subtitle:   PLDI'16
date:       2020-05-14
author:     Greagen
header-img: img/paper_read.jpg
catalog: true
tags:
    - static binary analysis
    - verification-aided similarity
    - partial equivalence
    - statistical similarity
---

# Statistical Similarity of Binaries

Yaniv David, Nimrod Partush, Eran Yahav

PLDI ’16

Keywords: static binary analysis; verification-aided similarity; partial equivalence; statistical similarity



## 解决的问题

二进制代码重用会带来风险。因此检测的是给定二进制程序 q，在一个大规模检测数据集 T 上进行针对每一个 t 的相似度计算。

三种场景挑战。代码由：

- 不同编译器编译而来。
- 不同编译器版本编译而来。
- 代码被修改，例如同一版本打了补丁。



## 实现思路

通过把代码分解成更小的代码片段，作者使用了片段层面的统计相似度来获取原始代码之间的相似度度量。该相似度是语义相似度，因为编译过程致使产生的二进制语法千差万别。

