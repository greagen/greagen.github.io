---
layout:     post
title:     ELF Format
subtitle:   ELF 文件格式
date:       2020-05-21
author:     Greagen
header-img: img/paper_read.jpg
catalog: true
tags:

    - ELF文件格式
---



# ELF Format

##　一、概述

　　ELF 文件是Linux以及Unix系统下的标准二进制文件格式。ELF 文件包含可执行文件、动态链接文件、目标文件、内核引导镜像文件等[<sup>1</sup>](#refer-anchor)。ELF文件一般采用的后缀有:none, .axf, .bin, .elf, .o, .prx, .puff, .ko, .mod and .so。magic number 为：0x7F 'E' 'L' 'F'。了解二进制文件结构是进行逆向工程、二进制文件分析等操作的基础和前提。



## 二、基本概念

### 1. ELF 文件类型

ELF文件可以被标记为以下几种类型：

- ET_NONE: 位置类型。该标记的文件类型不确定或者还未定义。
- ET_REL:　重定位文件（目标文件, xxx.o ）。可重定位文件是还未链接到可执行文件的独立代码，即编译后的 .o 文件。
- ET_EXEC: 可执行文件。这类文件也成为程序，是一个进程开始的入口。
- ET_DYN:　共享目标文件，即动态链接文件，.so 文件。在程序运行时被装载并链接到程序的进程镜像中。
- ET_CORE: 核心文件。在程序崩溃或者进程传递了一个SIGSEGV信号（分段违规）时，会在核心文件中记录整个进程的镜像信息。可以使用GDB读取这类文件来辅助调试并查找程序崩溃的原因。



### 2. File header

文件头是ELF文件的最开头部分，从文件的 0 偏移量开始。文件头信息是除了文件头之外的剩余部分文件的一个映射。该部分主要记录了ELF文件类型，程序头个数，节个数等等。通过`readelf -h`命令查看文件头信息，样例如下所示:

```shell
$ readelf -h libpng16.so.16
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Shared object file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x6230
  Start of program headers:          64 (bytes into file)
  Start of section headers:          253104 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         10
  Size of section headers:           64 (bytes)
  Number of section headers:         28
  Section header string table index: 27
```

ELF头的源码定义以及各个字段的意义[<sup>2</sup>](#refer-anchor)。

简要说明如下：

```c
typedef struct {
        unsigned char   e_ident[EI_NIDENT]; // 开头取16字节数据，显示文件标志等信息。
        Elf64_Half      e_type;  // 显示第一小节中所示的文件类型
        Elf64_Half      e_machine; //CPU 架构，有一百种之多，见源码定义文档中。
        Elf64_Word      e_version;
        Elf64_Addr      e_entry; 
        Elf64_Off       e_phoff; // 程序头表偏移
        Elf64_Off       e_shoff; //节头表偏移
        Elf64_Word      e_flags;
        Elf64_Half      e_ehsize; // elf header size
        Elf64_Half      e_phentsize; //程序头实体size，程序头表中的所有实体都具有相同的size
        Elf64_Half      e_phnum; // 程序头个数
        Elf64_Half      e_shentsize; //节头实体size，节头表中所有实体均有相同size
        Elf64_Half      e_shnum; //节头个数
        Elf64_Half      e_shstrndx; //
} Elf64_Ehdr;
```



每个ELF文件包含一个文件头，后面紧接文件数据。后面的数据包括：程序头和节头等。



### 3. segment & section

段（segment）和节（section）是二进制文件内容的两个重要概念。许多人经常把二者搞混。我们首先看一下wiki上对二者的解释。

> The segments contain information that is needed for run time execution of the file, while sections contain important data for linking and relocation. Any byte in the entire file can be owned by one section at most, and orphan bytes can occur which are unowned by any section.

一个段包含零个至多个节，一个elf文件包含零个或多个段。

二者的区别也体现在接下来的程序头和节头的概念中。

>An ELF file has two views: the program header shows the *segments* used at run time, whereas the section header lists the set of *sections* of the binary.

程序头描述的是段的信息。程序头表是程序头列表，跟在 ELF 头的后面。

段提供执行层面的操作，与OS交互，其中可加载的段（loadable segments）加载到进程镜像。段告诉操作系统该段是否应该被加载到内存中，该段的可读可写属性等。节提供链接层面的信息，包含指令、符号表，重定向信息等，与链接器打交道。节告诉链接器的是跳转符号、指令原始内容等。



![Segment VS Section](/home/lab/Desktop/greagen.github.io/img/segment-vs-section.jpg)



### 4. Program header

程序头是对段（segment）的描述，程序头表是文件中所有段组成的程序头列表，跟在最开始的 ELF 头后面。ELF 头中的`e_phoff`定义了文件中的程序头表的偏移量。程序头的源码定义见官方文档说明[<sup>3</sup>](#refer_acthor)。

```c
typedef struct {
	Elf64_Word	p_type; // 程序头类型
	Elf64_Word	p_flags; //段的权限
	Elf64_Off	p_offset; //该段的偏置
	Elf64_Addr	p_vaddr; // 虚拟地址
	Elf64_Addr	p_paddr; // 物理地址
	Elf64_Xword	p_filesz; // 段大小
	Elf64_Xword	p_memsz; 
	Elf64_Xword	p_align;
} Elf64_Phdr;
```



常见的程序头类型有五种：PT_LOAD, PT_DYNAMIC, PT_NOTE, PT_INTERP, PT_PHDR。

样例如下：

```shell
$ readelf -l libpng16.so.16

Elf file type is DYN (Shared object file)
Entry point 0x6230
There are 10 program headers, starting at offset 64

Program Headers:
  Type           Offset             VirtAddr           PhysAddr
                 FileSiz            MemSiz              Flags  Align
  LOAD           0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000004cf8 0x0000000000004cf8  R      1000
  LOAD           0x0000000000005000 0x0000000000005000 0x0000000000005000
                 0x0000000000023795 0x0000000000023795  R E    1000
  LOAD           0x0000000000029000 0x0000000000029000 0x0000000000029000
                 0x000000000000a560 0x000000000000a560  R      1000
  LOAD           0x00000000000338f0 0x00000000000348f0 0x00000000000348f0
                 0x0000000000000710 0x0000000000000718  RW     1000
  DYNAMIC        0x0000000000033908 0x0000000000034908 0x0000000000034908
                 0x0000000000000230 0x0000000000000230  RW     8
  NOTE           0x0000000000000270 0x0000000000000270 0x0000000000000270
                 0x0000000000000024 0x0000000000000024  R      4
  NOTE           0x0000000000033540 0x0000000000033540 0x0000000000033540
                 0x0000000000000020 0x0000000000000020  R      8
  GNU_EH_FRAME   0x000000000002d4b8 0x000000000002d4b8 0x000000000002d4b8
                 0x0000000000000e94 0x0000000000000e94  R      4
  GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000000000 0x0000000000000000  RW     10
  GNU_RELRO      0x00000000000338f0 0x00000000000348f0 0x00000000000348f0
                 0x0000000000000710 0x0000000000000710  R      1

 Section to Segment mapping:
  Segment Sections...
   00     .note.gnu.build-id .gnu.hash .dynsym .dynstr .gnu.version .gnu.version_d .gnu.version_r .rela.dyn .rela.plt 
   01     .init .plt .plt.sec .text .fini 
   02     .rodata .eh_frame_hdr .eh_frame .note.gnu.property 
   03     .init_array .fini_array .data.rel.ro .dynamic .got .bss 
   04     .dynamic 
   05     .note.gnu.build-id 
   06     .note.gnu.property 
   07     .eh_frame_hdr 
   08     
   09     .init_array .fini_array .data.rel.ro .dynamic .got 
```



#### 4.1 PT_LOAD

一个可执行文件至少包含一个 PT_LOAD 类型的段。这类段是可装载的段，将会被装载或映射到内存中。一个需要动态链接的



三种字符串表（待查是否需要研究以使用）

```c
1. .dynstr
2. .shstrtab
3. .strtab
```







## Reference

[1] [ELF Format wiki](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)

[2] [ELF File header defination in source code.](https://refspecs.linuxfoundation.org/elf/gabi4+/ch4.eheader.html)

[3] [Program Header](https://refspecs.linuxfoundation.org/elf/gabi4+/ch5.pheader.html)

