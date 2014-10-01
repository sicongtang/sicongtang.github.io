---
layout: post
title: "Operating system concepts - MainMemory"
date: 2014-04-09 07:39:27 +0800
comments: true
categories: books
---
##Contiguous Memory Allocation
Problem:

Both the first-fit and best-fit strategies for memory allocation suffer from __external fragmentation__.

The general approach to avoiding this problem is to break the physical memory into fixed-sized blocks and allocate memory in units based on block size. With this approach, the memory allocated to a process may be slightly larger than the requested memory. The difference between these two numbers is __internal fragmentation__ — unused memory that is internal to a partition.

Solution:

One solution to the problem of external fragmentation is __compaction__. The goal is to shuffle the memory contents so as to place all free memory together in one large block. Compaction is possible only if relocation is dynamic and is done at execution time.

Another possible solution to the external-fragmentation problem is to permit the logical address space of the processes to be noncontiguous, thus allowing a process to be allocated physical memory wherever such memory is available.

##Page and Segmentation
__Paging__ is a memory-management scheme that permits the physical address space of a process to be noncontiguous. Paging avoids external fragmentation and the need for compaction.

{% img https://farm8.staticflickr.com/7454/13951470216_7af3187c5c.jpg paging_with_TLB %}

The __translation look-aside buffer(TLB)__ is associative, high-speed memory. Each entry in the TLB consists of two parts: a key (or tag) and a value. When the associative memory is presented with an item, the item is compared with all keys simultaneously. If the item is found, the corresponding value field is returned.

If the page number is not in the TLB (known as a __TLB miss__), a memory reference to the page table must be made. When the frame number is obtained, we can use it to access memory (Figure 8.11). In addition, we add the page number and frame number to the TLB, so that they will be found quickly on the next reference.

__Segmentation__ is a memory-management scheme that supports this user view of memory.
##Page Table
* hierarchical paging

Problem&Solution:

For example, consider a system with a 32-bit logical address space. If the page size in such a system is 4 KB (212), then a page table may consist of up to 1 million entries (232/212). Assuming that each entry consists of 4 bytes, each process may need up to 4 MB of physical address space for the page table alone. Clearly, we would not want to allocate the page table contiguously in main memory. One simple solution to this problem is to divide the page table into smaller pieces. We can accomplish this division in several ways.

	$ getconf PAGESIZE
	4096

* hashed page tables

Problem&Solution:

A common approach for handling address spaces larger than 32 bits is to use a __hashed page table__, with the hash value being the virtual page number.

* inverted page tables

Problem&Solution:

Usually, each process has an associated page table. One of the drawbacks of this method is that each page table may consist of millions of entries. These tables may consume large amounts of physical memory just to keep track of how other physical memory is being used.

Drawbacks:

Although this scheme decreases the amount of memory needed to store each page table, it increases the amount of time needed to search the table when a page reference occurs. 

Systems that use inverted page tables have difficulty implementing shared memory. Shared memory is usually implemented as multiple virtual addresses (one for each process sharing the memory) that are mapped to one physical address. This standard method cannot be used with inverted page tables; because there is only one virtual page entry for every physical page, one physical page cannot have two (or more) shared virtual addresses.




