---
layout: post
title: "Operating system concepts - Process/Thread"
date: 2014-03-25 05:57:31 +0800
comments: true
categories: books
---
##Process Control Block
__Process Control Block__ (PCB, also called Task Controlling Block, Task Struct, or Switchframe) is a data structure in the operating system kernel containing the information needed to manage a particular process.

Process state. The state may be new, ready, running, waiting, halted, and so on. 

Program counter. The counter indicates the address of the next instruction to be executed for this process. 

CPU registers. They include accumulators, index registers, stack pointers, and general-purpose registers, plus any condition-code information. Along with the program counter, this state information must be saved when an interrupt occurs, to allow the process to be continued correctly afterward.

<!--more-->

CPU-scheduling information. This information includes a process priority, pointers to scheduling queues, and any other scheduling parameters.

Memory-management information. This information may include such information as the value of the base and limit registers, the page tables, or the segment tables, depending on the memory system used by the operating system. 

Accounting information. This information includes the amount of CPU and real time used, time limits, account numbers, job or process numbers, and so on. 

I/O status information. This information includes the list of I/O devices allocated to the process, a list of open files, and so on.


##Context Switch
A __context switch__ (also sometimes referred to as a process switch or a task switch) is the switching of the CPU (central processing unit) from one process or thread to another.

A __context__ is the contents of a CPU's registers and program counter at any point in time.

A __program counter__ is a specialized register that indicates the position of the CPU in its instruction sequence and which holds either the address of the instruction being executed or the address of the next instruction to be executed, depending on the specific system.

Context switching can be described in slightly more detail as the kernel (i.e., the core of the operating system) performing the following activities with regard to processes (including threads) on the CPU:

1. suspending the progression of one process and storing the CPU's state (i.e., the context) for that process somewhere in memory,

2. retrieving the context of the next process from memory and restoring it in the CPU's registers and

3. returning to the location indicated by the program counter (i.e., returning to the line of code at which the process was interrupted) in order to resume the process.

###When to switch?
Context switches can occur only in kernel mode.

__Multitasking__ 1.)Preemptive schedulers often configure a timer interrupt to fire when a process exceeds its time slice. 2.)This context switch can be triggered by the process making itself unrunnable, such as by waiting for an I/O or synchronization operation to complete.

__Interrupt handling__ 1.) Hardware interrupt

__User and kernel mode switching__ may cause context switch.

##Multithread programing
Benifits 1.) Responsiveness 2.)Resource sharing 3.) Economy 4.)Scalability
##Multicore Programming
Concerns 1.) Dividing activities 2.)Balance Data splitting 3.)Data dependency 4.)Testing and debugging
##Multithread Model
* Many-to-one model
* One-to-one model

The nptl(native posix thread library) implementation only uses a 1:1 thread model. The scheduler handles every thread as if it were a process. Therefore only the supported scope is PTHREAD_SCOPE_SYSTEM(Plesae see on Pthread Scheduling label). The default scheduling policy is SCHED_OTHER, which is the default Linux scheduler. The nptl implementation can utilize multiple CPUs.

* Many-to-many model
* Green thread

On multi-CPU machines, native threads can run more than one thread simultaneously by assigning different threads to different CPUs. Green threads run on only one CPU.

Green threads, the threads provided by the JVM, run at the user level, meaning that the JVM creates and schedules the threads itself. Therefore, the operating system kernel doesn't create or schedule them. Instead, the underlying OS sees the JVM only as one thread.

* Light weight process(LWP)
In the traditional meaning of the term, as used in Unix System V and Solaris, a LWP runs in user space on top of a single kernel thread and shares its address space and system resources with other LWPs within the same process.

##Pthread Scheduling
There are two possible contention scopes. PTHREAD_SCOPE_SYSTEM and PTHREAD_SCOPE_PROCESS. They can be set with pthread_attr_setscope(). The scope of a thread can only be specified before the thread is created.

* PTHREAD_SCOPE_SYSTEM

A thread that has a scope of PTHREAD_SCOPE_SYSTEM will content with other processes and other PTHREAD_SCOPE_SYSTEM threads for the CPU.

* PTHREAD_SCOPE_PROCESS

All threads of a process that have a scope of PTHREAD_SCOPE_PROCESS will be grouped together and this group of threads contents for the CPU. If there is a process with 4 PTHREAD_SCOPE_PROCESS threads and 4 PTHREAD_SCOPE_SYSTEM threds, then each of the PTHREAD_SCOPE_SYSTEM threads will get a fifth of the CPU and the other 4 PTHREAD_SCOPE_PROCESS threads will share the remaing fifth of the CPU. How the PTHREAD_SCOPE_PROCESS threads share their fifth of the CPU among themselves is determined by the scheduling policy and the thread's priority.

##Process Creation
When a process creates a new process, two possibilities exist in terms of execution:

1. The parent continues to execute concurrently with its children.
2. The parent waits until some or all of its children have terminated.

There are also two possibilities in terms of the address space of the new process:

1. The child process is a duplicate of the parent process (it has the same program and data as the parent).
2. The child process has a new program loaded into it.

For example, if `clone()` is passed the flags `CLONE FS`, `CLONE VM`, `CLONE SIGHAND`, and `CLONE FILES`, the parent and child tasks will share the same file-system information (such as the current working directory), the same memory space, the same signal handlers, and the same set of open files.

However, if none of these flags is set when `clone()` is invoked, no sharing takes place, resulting in functionality similar to that provided by the `fork()` system call.







