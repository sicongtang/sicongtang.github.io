---
layout: post
title: "Java Performance - OS Monitoring"
date: 2014-05-21 00:16:37 +0800
comments: true
categories: books
---
##Bottom Up Approach
Bottom up begins at the lowest level of the software stack, at the CPU level looking at statistics such as CPU cache misses, inefficient use of CPU instructions, and then working up the software stack at what constructs or idioms are used by the application.
##Choosing the Right CPU Architecture 
One of the major design points behind the SPARC T-series processors is to address CPU cache misses by introducing multiple hardware threads per core.
##CPU Utilization
A system with a single CPU socket with a quad core processor with hyperthreading disabled will show four CPUs in the GNOME System Monitor and report four virtual processors using the Java API Runtime.availableProcessors(). 

	xosview
	vmstat
	mpstat
	top 

Which java thread is consuming CPU?

	jstack
##Monitoring Linux CPU Scheduler Run Queue
	vmstat

##Memory Utilization
	top
	/proc/meminf
However, the following vmstat output from a Linux system illustrates a system that is experiencing swapping.  P36

##Monitoring Lock Contention on Linux
	pidstat -w -I -p 9391 5

Hence, 3500 divided by 2, the num- ber of virtual processors = 1750. 1750 * 80,000 = 140,000,000. The number of clock cycles in 1 second on a 3.0GHz processor is 3,000,000,000. Thus, the percentage of clock cycles wasted on context switches is 140,000,000/3,000,000,000 = 4.7%. 

The cost of a voluntary context switch at a processor clock cycle level is an expensive operation, generally upwards of about 80,000 clock cycles. 

Again applying the general guideline of 3% to 5% of clock cycles spent in voluntary context switches implies a Java application that may be suffering from lock contention. 

##Quick Lock Contention Monitoring
##Isolating Hot Locks 
A common practice to find contended locks in a Java application has been to periodically take thread dumps and look for threads that tend to be blocked on the same lock across several thread dumps.  

##Monitoring Involuntary Context Switches 
In contrast to voluntary context switching where an executing thread voluntarily takes itself off the CPU, involun- tary thread context switches occur when a thread is taken off the CPU as a result of an expiring time quantum or has been preempted by a higher priority thread. 

Involuntary context switches can also be monitored on Linux using `pidstat -w`. High involuntary context switches are an indication there are more threads ready to run than there are virtual processors available to run them. As a result it is common to observe a high run queue depth in vmstat, high CPU utilization, and a high number of migrations (migrations are the next topic in this section) in conjunction with a large number of involuntary context switches.

On Linux, creation of processor sets and assigning applications to those processor sets can be accomplished using the Linux `taskset` command.

##Monitoring Thread Migrations 
As a general guideline, Java applications scaling across multiple cores or virtual processors and observing migrations greater than 500 per second could benefit from binding Java applications to processor sets.  

##Network I/O Utilization 
	netstat -i 
	nicstat

##Disk I/O Utilization
	iostat -xm

One of the challenges with monitoring disk I/O utilization is identifying which files are being read or written to and which application is the source of the disk activity.

At the application level any strategy to minimize disk activity will help such as reducing the number of read and write operations using buffered input and output streams or integrating a caching data structure into the application to reduce or eliminate disk interaction.
##Additional Command Line Tools 
	sar
##Monitoring CPU Utilization on SPARC T-Series Systems