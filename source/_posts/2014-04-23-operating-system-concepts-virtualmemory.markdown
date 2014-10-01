---
layout: post
title: "Operating system concepts - VirtualMemory"
date: 2014-04-23 05:43:26 +0800
comments: true
categories: books
---
##Demand Paging
Loading the entire program into memory results in loading the executable code for all options, regardless of whether an option is ultimately selected by the user or not. An alternative strategy is to load pages only as they are needed. This technique is known as __demand paging__ and is commonly used in virtual memory systems.
##Page Fault
A page fault causes the following sequence to occur:

![Alt text](https://farm6.staticflickr.com/5325/13952117776_754a2b4bda.jpg "Steps_in_handling_a_page_fault.png")

1.	Trap to the operating system. 
2.	Save the user registers and process state. 
3.	Determine that the interrupt was a page fault. 
4.	Check that the page reference was legal and determine the location of the page on the disk. 
5.	Issue a read from the disk to a free frame:
	a. Wait in a queue for this device until the read request is serviced.
	b. Wait for the device seek and/or latency time.
	c. Begin the transfer of the page to a free frame.
6.	While waiting, allocate the CPU to some other user (CPU scheduling, optional). 
7.	Receive an interrupt from the disk I/O subsystem (I/O completed). 
8.	Save the registers and process state for the other user (if step 6 is executed). 
9.	Determine that the interrupt was from the disk. 
10.	Correct the page table and other tables to show that the desired page is now in memory. 
11.	Wait for the CPU to be allocated to this process again. 
12.	Restore the user registers, process state, and new page table, and then resume the interrupted instruction. 

##Copy-on-Write
Considering that many child processes invoke the exec() system call immediately after creation, the copying of the parent’s address space may be unnecessary.

Instead, we can use a technique known as __copy-on-write__, which works by allowing the parent and child processes initially to share the same pages. These shared pages are marked as copy-on-write pages, meaning that if either process writes to a shared page, a copy of the shared page is created.

##Page Replacement
1.	Find the location of the desired page on the disk. 
2.	Find a free frame: 
	a. If there is a free frame, use it. 
	b. If there is no free frame, use a page-replacement algorithm to select a victim frame. 
	c. Write the victim frame to the disk; change the page and frame tables accordingly. 
3.	Read the desired page into the newly freed frame; change the page and frame tables. 
4.	Restart the user process. 

__Page Replacement Algorithm__

* FIFO Page Replacement
* Optimal Page Replacement
* LRU Page Replacement
* LRU-Approximation Page Replacement
* Additional-Reference-Bits Algorithm
* Second-Chance Algorithm
	a. Enhanced Second-Chance Algorithm
* Counting-Based Page Replacement
	a. least-frequently-used (LFU) page-replacement algorithm
	b. most-frequently-used (MFU) page-replacement algorithm

##Raw I/O
Some operating systems give special programs the ability to use a disk partition as a large sequential array of logical blocks, without any file-system data structures. This array is sometimes called the raw disk, and I/O to this array is termed __raw I/O__.

__Raw I/O__ bypasses all the file- system services, such as file I/O demand paging, file locking, prefetching, space allocation, file names, and directories.

##Thrashing
In fact, look at any process that does not have “enough” frames. If the process does not have the number of frames it needs to support pages in active use, it will quickly page-fault. At this point, it must replace some page. However, since all its pages are in active use, it must replace a page that will be needed again right away. Consequently, it quickly faults again, and again, and again, replacing pages that it must bring back in immediately.

This high paging activity is called __thrashing__. A process is thrashing if it is spending more time paging than executing.

