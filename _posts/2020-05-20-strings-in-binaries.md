---
layout:     post
title:      Strings in Binaries
subtitle:   
date:       2020-05-20
author:     Greagen
header-img: img/paper_read.jpg
catalog: true
tags:

    - 字符串
    - 同源文件检测
    - 组件版本识别
---

# Strings in Binaries

二进制文件中经常会包含大量的字符串信息，这些信息可能是错误报告，调试信息，输出结果，运算常量等。这些字符串能够反应出大量的代码信息和开发信息，在软件分析、代码分析等领域有着诸多的应用。



## Format

ASCII format strings

Unicode encoding strings



## 版本字符串恢复

PANDORA: A Scalable and Efficient Scheme to Extract Version of Binaries in IoT Firmwares

[Weidong Zhang ](https://ieeexplore.ieee.org/author/37086427212); [Yu Chen ](https://ieeexplore.ieee.org/author/37086424756); [Hong Li](https://ieeexplore.ieee.org/author/37085741673); [Zhi Li ](https://ieeexplore.ieee.org/author/37085683916); [Limin Sun](https://ieeexplore.ieee.org/author/37086428800)

ICC'2018

论文首先从二进制文件的数据段和代码段中提取ASCII 形式的字符串，在数据段中提取Unicode encoding strings，然后使用整理的模式匹配数据库，匹配字符串是否符合版本字符串特定的格式比如“version: %d%d”等等。找到匹配成功的字符串后，根据控制流，获取相应依赖的变量的值。最终推断出版本字符串的内容。



评价：方法简单，可扩展，实验给出检出率84%，但是没有说检出的结果是不是都为正确的版本，并且方法受限于版本字符串匹配模式是否全面。



## 同源二进制文件检测

### IHB

利用数据段和代码段的ASCII 形式的字符串和数据段Unicode 形式的字符串。

提取方法，数据段ASCII字符串使用strings,　代码段的通过反汇编，识别push等指令，然后记录连续的ASCII 字符拼接起来，超过一定长度则记录该特征。Unicode形式的字符串通过识别两字节序列，判断是否存在于语言字符集中。

该工作使用了Minhash进行加快查找速度。



