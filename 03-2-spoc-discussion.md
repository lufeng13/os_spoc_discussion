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

# 小组思考题

#### 2.有一台假想的计算机，页大小（page size）为32 Bytes，支持32KB的虚拟地址空间（virtual address space）,有4KB的物理内存空间（physical memory），采用二级页表，一个页目录项（page directory entry ，PDE）大小为1 Byte,一个页表项（page-table entries PTEs）大小为1 Byte，1个页目录表大小为32 Bytes，1个页表大小为32 Bytes。页目录基址寄存器（page directory base register，PDBR）保存了页目录表的物理地址（按页对齐）。

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