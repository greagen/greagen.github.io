---
layout:     post
title:      Software Birthmark
subtitle:   
date:       2020-05-16
author:     Greagen
header-img: img/paper_read.jpg
catalog: true
tags:
    - software watermark
    - software birthmark
---

# Software Birthmark

A software birthmark is a unique characteristic that a program possesses and can be used to identify the program.

软件胎记（？？？好奇怪的翻译）是程序具有的独特性特征，因为其具有足够的独特性，可以用来识别所属的程序。软件胎记可以用来进行代码克隆检测、代码抄袭检测等。





## 软件水纹技术

C. Collberg and C. Thomborson. Software watermarking: Models and dynamic embeddings. In Principles of Programming Languages 1999, Jan. 1999.

Software watermark is a unique identifier embedded in the protected software, which is hard to remove but easy to verify.



“a sufficiently determined attackers will eventually be able to defeat any watermark.” [9].

[9] C. Collberg, E. Carter, S. Debray, A. Huntwork, C. Linn, and M. Stepp. Dynamic path-based software watermarking. In Proceedings of the Conference on Programming Language Design and Implementation, 2004.





## software birthmark

G. Myles and C. Collberg. Detecting software theft via whole program path birthmarks. In ISC, pages 404–415, 2004.

D. Schuler, V. Dallmeier, and C. Lindig. A dynamic birthmark for java. In ASE ’07: Proceedings of the twenty-second IEEE/ACM international conference on Automated software engineering, 2007.

H. Tamada, M. Nakamura, A. Monden, and K. ichi Matsumoto. Design and evaluation of birthmarks for detecting theft of java programs. In Proc. IASTED International Conference on Software Engineering, 2004.

H. Tamada, K. Okamoto, M. Nakamura, and A. Monden. Dynamic software birthmarks to detect the theft of windows applications. In International Symposium on Future Software Technology 2004 (ISFST 2004), 2004.



### behavior based birthmarks

Behavior Based Software Theft Detection，Xinran Wang ,Yoon-Chan Jhi ,Sencun Zhu ,Peng Liu CCS'09

该文章只针对large-scale software，因此小型软件或者组件不在文章考虑范围内。并且该文章使用自动化的动态分析过程，记录程序执行时系统调用情况。以系统调用为节点，以数据依赖和控制依赖为边，生成系统调用依赖图（SCDG），过滤噪声后，对1-1比较的两个程序进行子图同构匹配。

该方法不适用大规模数据量的小组件数据库。但是文章中提出的五种指纹要求和四中指纹分类友参考意义。

5 requirements



4 classes of software birthmark

static source code based birthmark [27], static executable code based birthmark [22], dynamic WPP based birthmark [21], and dynamic API based birthmark [26, 28].





**BinType** 静态分析工具，可以在大规模场景下使用，用于回复二进制代码中变量类型和函数参数。（怎么静态分析实现的？？）

**Pandora**中科院工作，使用二进制中的字符串识别IoT固件的版本





博士论文：Detecting Fine-Grained Similarity in Binaries，

该作者发表的论文是 Detecting code clones in binary executables ISSTA09

Deckard: Scalable and accurate tree-based detection of code clones





