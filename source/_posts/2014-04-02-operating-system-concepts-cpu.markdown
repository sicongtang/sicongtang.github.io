---
layout: post
title: "Operating system concepts - CPU"
date: 2014-04-02 06:30:57 +0800
comments: true
categories: books
---
##CPU Scheduling
__CPU scheduling__ is the task of selecting a waiting process from the ready queue and allocating the CPU to it. The CPU is allocated to the selected process by the dispatcher.

##Scheduling Criteria

* CPU utilization 
* Throughput
* Turnaround time

The interval from the time of submission of a process to the time of completion is the turnaround time. __Turnaround time__ is the sum of the periods spent waiting to get into memory, waiting in the ready queue, executing on the CPU, and doing I/O.

* Waiting time
* Response time

<!--more-->

##Scheduling Alogrithm

* First-Come, First-Served Scheduling
* Shortest-Job-First Scheduling
* Priority Scheduling

A major problem with priority scheduling algorithms is __indefinite blocking__, or __starvation__.

A solution to the problem of indefinite blockage of low-priority processes is __aging__.
	
* Round-Robin Scheduling
* Multilevel Queue Scheduling
* Multilevel Feedback Queue Scheduling

##Multiple-Processor/Multicore Scheduling
* Processor Affinity

Because of the high cost of invalidating and repopulating caches, most SMP systems try to avoid migration of processes from one processor to another and instead attempt to keep a process running on the same processor. This is known as __processor affinity__.

Some systems — such as Linux — also provide system calls that support hard affinity, thereby allowing a process to specify that it is not to migrate to other processors.

The main-memory architecture of a system can affect processor affinity issues. Recall the knowledge non-uniform memory access (NUMA) mentioned in previous post, in which a CPU has faster access to some parts of main memory than to other parts. 

* Load Balancing

Load balancing attempts to keep the workload evenly distributed across all processors in an SMP system. It is important to note that load balancing is typically only necessary on systems where each processor has its own private queue of eligible processes to execute.

There are two general approaches to load balancing: __push migration__ and __pull migration__. Linux runs its load-balancing algorithm every 200 milliseconds (push migration) or whenever the run queue for a processor is empty (pull migration).

* Memory Stall

Researchers have discovered that when a processor accesses memory, it spends a significant amount of time waiting for the data to become available. This situation, known as a __memory stall__, may occur for various reasons, such as a __cache miss__ (accessing data that are not in cache memory).

* Virtualization

The virtualization software presents one or more virtual CPUs to each of the virtual machines running on the system and then schedules the use of the physical CPUs among the virtual machines.



