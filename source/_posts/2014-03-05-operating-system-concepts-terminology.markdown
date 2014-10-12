---
layout: post
title: "Operating system concepts - Terminology"
date: 2014-03-05 05:04:59 +0800
comments: true
categories: books
---

The aim of this post is to review the crucial terms of operating-system and basic interaction of these underlying components.

Before starting with topic, I am going to reveal why I try to read books of OS related at this point. Let me conclude the reasons, first and foremost, various applications in my daily work encounter issues that are associated with LINUX system, such as buffer size, open file limits and unable to create native threads etc. All of these issues point out to the knowledge of OS. Next, Java performance tuning is always based on the operating system concept, like memory management, cpu utilization etc. Furthermore, including all the design concepts are derived from the solution of common issue inside operating system.

Due to these, it is so encouraged to read and go through operating system concept again that I could be inspired and get everything tied. If there are any findings or association with Java, I will make more explanations and comments linked with definitive resource.

<!--more-->

##1.Storage Device Hierarchy
registers -- > cache -- > main memory (RAM random access memory) -- > electronic disk -- > magnetic disk -- > optical disk -- > magnetic tapes

![Alt text](https://farm4.staticflickr.com/3775/13508587744_aa4be3c238_o.png "cpu_cache_hierarchy")

##2.Symmetric Multiprocessing (SMP)
SMP systems are tightly coupled multiprocessor systems with a pool of homogeneous processors running independently, each processor executing different programs and working on different data and with capability of sharing common resources (memory, I/O device, interrupt system and so on) and connected using a system bus or a crossbar.

	$uname -a
	Linux [nodename] 2.6.18-348.18.l.e15 #1 SMP [date] x86_64 x86_64 GUN/Linux

##3.Non-uniform Memory Access (NUMA)
NUMA is a computer memory design used in multiprocessing, where the memory access time depends on the memory location relative to the processor. Under NUMA, a processor can access its own local memory faster than non-local memory (memory local to another processor or memory shared between processors). 
###NUMA collector enhancements
* The Parallel Scavenger garbage collector has been extended to take advantage of machines with NUMA (Non Uniform Memory Access) architecture. 
The NUMA-aware allocator can be turned on with the `-XX:+UseNUMA` flag in conjunction with the selection of the Parallel Scavenger garbage collector. The Parallel Scavenger garbage collector is the default for a server-class machine. The Parallel Scavenger garbage collector can also be turned on explicitly by specifying the `-XX:+UseParallelGC` option.
If you want see the detail, please see [oracle official document](http://docs.oracle.com/javase/7/docs/technotes/guides/vm/performance-enhancements-7.html).

##4.Clustering
Clustering can be structured asymmetrically or symmetrically. In asymmetric clustering, one machine is in hot-standby mode while the other is running the applications. The hot-standby host machine does nothing but monitor the active server. If that server fails, the hot-standby host becomes the active server. In symmetric mode, two or more hosts are running applications and are monitoring each other. This mode is obviously more efficient, as it uses all of the available hardware. It does require that more than one application be available to run.

For example in real-world scenario: The secondary EMS server is hot-standby mode. Two Weblogic servers are symmetric mode and load balance.

##5.Network
###5.1.local area network (LAN)
A local area network (LAN) is a computer network that user interconnects computers in a limited area such as a home, school, computer laboratory, or office building using network media.
###5.2.storage-area network(SAN)
SANs are primarily used to enhance storage devices, such as disk arrays, tape libraries, and optical jukeboxes, accessible to servers so that the devices appear like locally attached devices to the operating system.
###5.3.wide area network (WAN)
A wide area network (WAN) is a network that covers a broad area (i.e., any telecommunications network that links across metropolitan, regional, or national boundaries) using private or public network transports. WANs are often built using leased lines. WANs can also be built using less costly circuit switching or packet switching methods.

##6.Computing Environments
###6.1.traditional computing
__Batch systems__ processed jobs in bulk, with predetermined input (from files or other sources of data).

__Time-sharing__ systems used a timer and scheduling algorithms to rapidly cycle processes through the CPU, giving each user a share of the resources. Time-sharing systems are an extension of multiprogramming wherein CPU scheduling algorithms rapidly switch between jobs, thus providing the illusion that all jobs are running concurrently.

###6.2client–Server computing (compute or file server system)
A __socket__ is identified by an IP address concatenated with a port number. Servers implementing specific services (such as telnet, FTP, and HTTP) listen to well-known ports (a telnet server listens to port 23; an FTP server listens to port 21; and a Web, or HTTP, server listens to port 80). 

The __remote procedure calls (RPCs)__ was designed as a way to abstract the procedure-call mechanism for use between systems with network connections.

__Remote method invocation (RMI)__ is a Java feature similar to RPCs.

###6.3.peer-to-peer computing
###6.4.web-based computing

##7.System Calls
In computing, a system call is how a program requests a service from an operating system's kernel.
System calls provide an essential interface between a process and the operating system.
System calls can be grouped roughly into six major categories: process control, file manipulation, device manipulation, information maintenance, communications, and protection.

To monitor and trace the system call within the process, we can use strace command in Linux.
Strace is used for debugging and troubleshooting the execution of an executable on Linux environment. It displays the system calls used by the process, and the signals received by the process.

	$strace

##8.Interprocess Communication
There are two common models of interprocess communication:
###8.1.message-passing model
__Message passing__ is useful for exchanging smaller amounts of data, because no conflicts need be avoided.

Message passing may be either blocking or nonblocking— also known as synchronous and asynchronous. 1.)__Blocking send__ 2.)__Nonblocking send__ 3.)__Blockingreceive__ 4.)__Nonblocking receive__

Whether communication is direct or indirect, messages exchanged by commu- nicating processes reside in a temporary queue. 1.)__Zero capacity__ 2.)__Boundedcapacity__ 3.)__Unboundedcapacity__
###8.2.shared-memory model
In the __shared-memory__ model, processes use shared memory create and shared memory attach system calls to create and gain access to regions of memory owned by other processes.


























