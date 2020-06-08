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





## 软件水印技术

C. Collberg and C. Thomborson. Software watermarking: Models and dynamic embeddings. In Principles of Programming Languages 1999, Jan. 1999.

Software watermark is a unique identifier embedded in the protected software, which is hard to remove but easy to verify.



“a sufficiently determined attackers will eventually be able to defeat any watermark.” [9].

[9] C. Collberg, E. Carter, S. Debray, A. Huntwork, C. Linn, and M. Stepp. Dynamic path-based software watermarking. In Proceedings of the Conference on Programming Language Design and Implementation, 2004.



G. Myles and C. Collberg. Detecting software theft via whole program path birthmarks. In ISC, pages 404–415, 2004. [7,9,11,14,17,20,22,25]

软件水印是通过想软件中嵌入独特的水印，来表示软件的授权，是一种厂商的主动防御。需要向软件中嵌入与水印相关的额外的代码。软件指纹是基于软件内容，无需嵌入额外代码的技术。该技术可以有软件厂商使用，也可以由第三方检测机构使用，软件指纹不能检测是否侵犯授权，只能检测比较的双方是否是复制关系，根据相似度进而给出法律上的风险推断。



## software birthmark

> A software birthmark is a unique characteristic, or set of characteristics, that a program possesses and which can be used to identify the program.   —— Detecting Software theft via...



**G. Myles and C. Collberg. Detecting software theft via whole program path birthmarks. In ISC, pages 404–415, 2004.**

该论文提出一种指纹特征：Whole Program Path Birthmarks (WPPB)，该方法是动态技术，依赖于程序的执行模式。该论文声称自己首次提出动态指纹的概念，也就是说之前的工作都是静态的。此外，该工作对比的也是静态的指纹技术，是由Tamada提出的四种静态指纹技术。

该工作的做法是从CFG开始，记录执行路径，将执行路径转换成DAG图，该图每个节点记录的是程序可能的执行路径，各个节点之间互相调用跳转。最后比较图同构，计算比例获取相似度。

测试环节太少，不太规范，早期研究看看就行。论文测试数据为一个java程序，wc.jar文件。



最早使用软件指纹的工作之一(Detecting software theft via 说的):

Derrick Grover. Program identification. In Derrick Grover, editor, The Protection of Computer Software – Its Technology and Applications, pages 122–154. Cambridge University Press, 1989



an IBM court case [下文] 使用了寄存器弹入弹出的顺序来证明PC-AT ROM 被非法复制。

Ross J. Anderson and Fabien A. P. Petitcolas. On the limits of steganography. IEEE Journal of Selected Areas in Communications, 16(4):474–481, May 1998. Special issue on copyright & privacy protection.



### n-gram

**Ginger Myles Christian Collberg. k-gram Based Software Birthmarks[J]. sac '05 proceedings of the acm symposium on applied computing, 2005:314-318.**

使用kgram的文本比较方法，静态生成程序指纹，没有其他的优化，使用java应用进行测试。对比工作为Tamada



**H. Tamada, K. Okamoto, M. Nakamura, and A. Monden. Dynamic software birthmarks to detect the theft of windows applications. In International Symposium on Future Software Technology 2004 (ISFST 2004), 2004.**

使用的是动态执行window应用程序，记录API函数调用的防范，





D. Schuler, V. Dallmeier, and C. Lindig. A dynamic birthmark for java. In ASE ’07: Proceedings of the twenty-second IEEE/ACM international conference on Automated software engineering, 2007.

H. Tamada, M. Nakamura, A. Monden, and K. ichi Matsumoto. Design and evaluation of birthmarks for detecting theft of java programs. In Proc. IASTED International Conference on Software Engineering, 2004.



Haruaki Tamada, Masahide Nakamura, Akito Monden, and Kenichi Matsumoto. Detecting the theft of programs using birthmarks. Information Science Technical Report NAIST-IS-TR2003014 ISSN 0919-9527, Graduate School of Information Science, Nara Institute of Science and Technology, Nov 2003.





### behavior based birthmarks

**Behavior Based Software Theft Detection，Xinran Wang ,Yoon-Chan Jhi ,Sencun Zhu ,Peng Liu CCS'09**

该文章只针对large-scale software，因此小型软件或者组件不在文章考虑范围内。并且该文章使用自动化的动态分析过程，记录程序执行时系统调用情况。以系统调用为节点，以数据依赖和控制依赖为边，生成系统调用依赖图（SCDG），过滤噪声后，对1-1比较的两个程序进行子图同构匹配。

该方法不适用大规模数据量的小组件数据库。但是文章中提出的五种指纹要求和四中指纹分类友参考意义。

5 requirements



4 classes of software birthmark

static source code based birthmark [27], static executable code based birthmark [22], dynamic WPP based birthmark [21], and dynamic API based birthmark [26, 28].

系统调用行为在恶意软件检测、分类等领域的应用:

M. Christodorescu, S. Jha, and C. Kruegel. Mining specifications of malicious behavior. In Proceedings of
ESEC/FSE, 2008.

E. Kirda, C. Kruegel, G. Banks, G. Vigna, and R. A.
Kemmerer. Behavior-based spyware detection. In Proceedings of the 15th conference on USENIX Security Symposium, 2006.





**BinType** 静态分析工具，可以在大规模场景下使用，用于回复二进制代码中变量类型和函数参数。（怎么静态分析实现的？？）

**Pandora**中科院工作，使用二进制中的字符串识别IoT固件的版本





博士论文：Detecting Fine-Grained Similarity in Binaries，

该作者发表的论文是 Detecting code clones in binary executables ISSTA09

Deckard: Scalable and accurate tree-based detection of code clones



**software piracy detection**领域查论文

