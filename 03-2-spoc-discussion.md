# 与视频相关思考题

## 6.1 非连续内存分配的需求背景

#### 1.为什么要设计非连续内存分配机制？

##### 连续内存分配机制的缺点

* 分配给程序的物理内存必须连续，分配过程会产生内存碎片；

* 内存分配的动态修改困难；
	
* 内存利用效率较低。

##### 非连续内存分配机制的优势

* 提高内存利用效率和管理灵活性

	* 允许一个程序使用非连续的物理地址空间
	
	* 允许共享代码和数据
	
	* 支持加载和动态链接

#### 2.非连续内存分配中内存分块大小有哪些可能的选择？大小与大小是否可变？

非连续内存分配中，根据内存分块的大小的不同，可以分为段式和页式。

段式存储管理中，内存分块较大，且大小可变；页式存储管理中，内存分块较小，且大小不可变。

#### 3.为什么在大块时要设计大小可变，而在小块时要设计成固定大小？小块时的固定大小可以提供多种选择吗？



小块时的固定大小可以提供多种选择，页大小可以为1k,4k等等。

## 6.2 段式存储管理

#### 1.什么是段、段基址和段内偏移？

段表示访问方式和存储数据等属性相同的一段地址空间；

段基址是段所保存的内容在内存中的起始地址；

段内偏移是指段所保存的内容在内存中的地址相对于起始地址的偏移量。

#### 2.段式存储管理机制的地址转换流程是什么？为什么在段式存储管理中，各段的存储位置可以不连续？这种做法有什么好处和麻烦？





## 6.3 页式存储管理

#### 1.什么是页（page）、帧（frame）、页表（page table）、存储管理单元（MMU）、快表（TLB, Translation Lookaside Buffer）和高速缓存（cache）？

页是指把逻辑地址空间划分为大小相同的基本分配单位；

帧是指把物理地址空间划分为大小相同的基本分配单位；

页表是从页面到页帧实现逻辑地址到物理地址转换；

存储管理单元

快表

高速缓存

#### 2.页式存储管理机制的地址转换流程是什么？为什么在页式存储管理中，各段的存储位置可以不连续？这种做法有什么好处和麻烦？



多次存储访问

## 6.4 页表概述

#### 1.每个页表项有些什么内容？有哪些标志位？它们起什么作用？

每个页表项包括标志位和

#### 2.页表大小受哪些因素影响？

页表大小受页大小、地址空间大小和进程数目的影响。

## 6.5 快表和多级页表

#### 1.快表（TLB）与高速缓存（cache）有什么不同？

#### 2.为什么快表中查找物理地址的速度非常快？它是如何实现的？为什么它的的容量很小？

#### 3.什么是多级页表？多级页表中的地址转换流程是什么？多组页面有什么好处和麻烦？

## 6.6 反置页表

#### 1.页寄存器机制的地址转换流程是什么？

#### 2.反置页表机制的地址转换流程是什么？

#### 3.反置页表项有些什么内容？

PID、逻辑页号、标志位

## 6.7 段页式存储管理

#### 1.段页式存储管理机制的地址转换流程是什么？这种做法有什么好处和麻烦？



#### 2.如何实现基于段式存储管理的内存共享？



#### 3.如何实现基于页式存储管理的内存共享？




# 个人思考题

#### 请简要分析64bit CPU体系结构下的分页机制是如何实现的

```
  + 采分点：说明64bit CPU架构的分页机制的大致特点和页表执行过程
  - 答案没有涉及如下3点；（0分）
  - 正确描述了64bit CPU支持的物理内存大小限制（1分）
  - 正确描述了64bit CPU下的多级页表的级数和多级页表的结构或反置页表的结构（2分）
  - 除上述两点外，进一步描述了在多级页表或反置页表下的虚拟地址-->物理地址的映射过程（3分）
```

# 小组思考题

#### 1.某系统使用请求分页存储管理，若页在内存中，满足一个内存请求需要150ns (10^-9s)。若缺页率是10%，为使有效访问时间达到0.5us(10^-6s),求不在内存的页面的平均访问时间。请给出计算步骤。

[x]
500=0.9*150+0.1*x

#### 2.有一台假想的计算机，页大小（page size）为32 Bytes，支持32KB的虚拟地址空间（virtual address space）,有4KB的物理内存空间（physical memory），采用二级页表，一个页目录项（page directory entry ，PDE）大小为1 Byte,一个页表项（page-table entries PTEs）大小为1 Byte，1个页目录表大小为32 Bytes，1个页表大小为32 Bytes。页目录基址寄存器（page directory base register，PDBR）保存了页目录表的物理地址（按页对齐）。

PTE格式（8 bit） :

```
  VALID | PFN6 ... PFN0
```

PDE格式（8 bit） :

```
  VALID | PT6 ... PT0
```

其

```
VALID==1表示，表示映射存在；VALID==0表示，表示映射不存在。
PFN6..0:页帧号
PT6..0:页表的物理基址>>5
```

在物理内存模拟数据文件中，给出了4KB物理内存空间的值，请回答下列虚地址是否有合法对应的物理内存，请给出对应的pde index, pde contents, pte index, pte contents。

```
1) Virtual Address 6c74
   Virtual Address 6b22
2) Virtual Address 03df
   Virtual Address 69dc
3) Virtual Address 317a
   Virtual Address 4546
4) Virtual Address 2c03
   Virtual Address 7fd7
5) Virtual Address 390e
   Virtual Address 748b
```

比如答案可以如下表示：

```
Virtual Address 7570:
  --> pde index:0x1d  pde contents:(valid 1, pfn 0x33)
    --> pte index:0xb  pte contents:(valid 0, pfn 0x7f)
      --> Fault (page table entry not valid)

Virtual Address 21e1:
  --> pde index:0x8  pde contents:(valid 0, pfn 0x7f)
      --> Fault (page directory entry not valid)

Virtual Address 7268:
  --> pde index:0x1c  pde contents:(valid 1, pfn 0x5e)
    --> pte index:0x13  pte contents:(valid 1, pfn 0x65)
      --> Translates to Physical Address 0xca8 --> Value: 16
```

> 练习

> Virtual Address 6c74

> Virtual Address 6b22

##### 以6b22为例，描述转换过程
 
###### 1.根据虚拟地址得到索引和偏移等信息

|  6   |   b  |   2  |   2  |
| ---- | ---- | ---- | ---- |
| 0110 | 1011 | 0010 | 0010 |

| 页目录表项 | 页表项    | 页内偏移  |
| -------- | -------- | -------- |
|  11010   | 11001    | 00010    |
|  26      | 25       | 2        |

###### 2.根据页目录表的基址和偏移，在页目录表中找到对应表项

页目录表项对应的物理地址为0x220 + 26，即page 11行第27项，即d2。 

###### 3.求出页表基址

| d    | 2    |
| ---- | ---- |
| 1101 | 0010 |

| Valid | PT0      |
| ----- | -------- |
| 1     | 101 0010 |

左移五位得页表基址

| 1010 | 0100 | 0000 |
| ---- | ---- | ---- |
| a    | 4    | 0    |

故页表基址为0xa40

###### 4.根据页表的基址和偏移，在页表中找到对应表项

则页表项对应的物理地址为0xa40 + 25，即page 0x52行第26项，即c7。 

###### 5.计算转换后物理地址

| c    | 7    |
| ---- | ---- |
| 1100 | 0111 |

| Valid | PFN     |
| ----- | ------- |
| 1     | 100 0111|

```
Virtual Address 6b22:
  --> pde index:0x1a  pde contents:(valid 1, pfn 0x52)
    --> pte index:0x25  pte contents:(valid 1, pfn 0x47)
      --> Translates to Physical Address 0x8e2 --> Value: 1a
```
```
Virtual Address 6c74:
  --> pde index:0x1b  pde contents:(valid 1, pfn 0x20)
    --> pte index:0x3  pte contents:(valid 1, pfn 0x61)
      --> Translates to Physical Address 0xc34 --> Value: 06
```

#### 3.请基于你对原理课二级页表的理解，并参考Lab2建页表的过程，设计一个应用程序（可基于python, ruby, C, C++，LISP等）可模拟实现(2)题中描述的抽象OS，可正确完成二级页表转换。

```
#include <iostream>
#include <cstdio>
#include <fstream>

using namespace std;

int memory[4096];

int main() {

	fstream fin;
	fin.open("memory.txt");

	for(int i = 0 ; i < 128 ; i++) {
		string str;
		fin >> str;
		fin >> str;
		for(int j = 0 ; j < 32 ; j++) {
			fin >> hex >> memory[i * 32 + j];
		}
	}

	fin.close();

	int virtual_address;
	printf("Virtual Address 0x");
	scanf("%x", &virtual_address);

	int PDE_index = (virtual_address & 0x7c00) >> 10;
	int PTE_index = (virtual_address & 0x03e0) >> 5;
	int page_offset = virtual_address & 0x001f;

	int PDE = memory[544 + PDE_index];
	int pt = PDE & 0x7f;
	int PDE_valid = (PDE & 0x80) >> 7;
	printf("  --> pde index:0x%x  pde contents:(valid %d, pt 0x%x)\n", PDE_index, PDE_valid, pt);

	if(PDE_valid != 1) {
		printf("      --> Fault (page directory entry not valid)\n");
	}
	else {
		int PTE = memory[pt * 32 + PTE_index];
		int pfn = PTE & 0x7f;
		int PTE_valid = (PTE & 0x80) >> 7;
		printf("    --> pte index:0x%x  pte contents:(valid %d, pfn 0x%x)\n", PTE_index, PTE_valid, pfn);
	
		int physical_address = pfn * 32 + page_offset;
		int value = memory[physical_address];

		if(PTE_valid != 1) {
			printf("      --> Fault (page table entry not valid)\n");
		}
		else {
			printf("      --> Translates to Physical Address 0x%x --> Value: %x\n", physical_address, value);
		}
	}

	return 0;
}
```

#### 4.假设你有一台支持反置页表的机器，请问你如何设计操作系统支持这种类型计算机？请给出设计方案。


#### 5.X86的页面结构


# 扩展思考题

#### 阅读64bit IBM Powerpc CPU架构是如何实现反置页表，给出分析报告。