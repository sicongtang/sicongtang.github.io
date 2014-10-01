---
layout: post
title: "Operating system concepts - IO System"
date: 2014-04-27 17:43:57 +0800
comments: true
categories: books
---
##I/O Hardware
The device communicates with the machine via a connection point, or __port__ — for example, a serial port.

If devices use a common set of wires, the connection is called a bus. A __bus__ is a set of wires and a rigidly defined protocol that specifies a set of messages that can be sent on the wires.

This figure shows a __PCI bus__ (the common PC system bus) that connects the processor–memory subsystem to the fast devices and an expansion bus that connects relatively slow devices, such as the keyboard and serial and USB ports.

{% img https://farm8.staticflickr.com/7406/14029529882_f49ab52235_z.jpg pcbus_structure.png %}


A __controller__ is a collection of electronics that can operate a port, a bus, or a device. A serial-port controller is a simple device controller. It is a single chip (or portion of a chip) in the computer that controls the signals on the wires of a serial port.

By contrast, a __SCSI bus controller__ is not simple. Because the SCSI protocol is complex, the SCSI bus controller is often implemented as a separate circuit board (or a host adapter) that plugs into the computer. It typically contains a processor, microcode, and some private memory to enable it to process the SCSI protocol messages.


##Polling & Interrupts
In many computer architectures, three CPU-instruction cycles are sufficient to poll a device: read a device register, logical–and to extract a status bit, and branch if not zero. But __polling__ becomes inefficient when it is attempted repeatedly yet rarely finds a device to be ready for service, while other useful CPU processing remains undone.

The hardware mechanism that enables a device to notify the CPU is called an __interrupt__.

The basic interrupt mechanism works as follows. The CPU hardware has a wire called the __interrupt-request line__ that the CPU senses after executing every instruction. When the CPU detects that a controller has asserted a signal on the interrupt-request line, the CPU performs a state save and jumps to the __interrupt-handler routine__ at a fixed address in memory.

Another example is found in the implementation of system calls. Usually, a program uses library calls to issue system calls. The library routines check the arguments given by the application, build a data structure to convey the arguments to the kernel, and then execute a special instruction called a __software interrupt__, or __trap__.

##Direct Memory Access
For a device that does large transfers, such as a disk drive, it seems wasteful to use an expensive general-purpose processor to watch status bits and to feed data into a controller register one byte at a time—a process termed __programmed I/O (PIO)__. Many computers avoid burdening the main CPU with PIO by offloading some of this work to a special-purpose processor called a __direct-memory-access (DMA) controller__.


{% img https://farm8.staticflickr.com/7340/14029558121_fb7debb6a5_z.jpg DMA_transfer.png %}

##Blocking and Nonblocking I/O
An alternative to a nonblocking system call is an asynchronous system call. An asynchronous call returns immediately, without waiting for the I/O to complete.

The difference between nonblocking and asynchronous system calls is that a nonblocking read() returns immediately with whatever data are available—the full number of bytes requested, fewer, or none at all. An asynchronous read() call requests a transfer that will be performed in its entirety but will complete at some future time. 

{% img https://farm3.staticflickr.com/2927/14032739105_b20504ca80_z.jpg syn_asyn_io.png %}

##Buffering
A __buffer__ is a memory area that stores data being transferred between two devices or between a device and an application.

Buffering is done for three reasons. 
1. One reason is to cope with a speed mismatch between the producer and consumer of a data stream.
2. A second use of buffering is to provide adaptations for devices that have different data-transfer sizes.
3. A third use of buffering is to support __copy semantics__ for application I/O.

With __copy semantics__, the version of the data written to disk is guaranteed to be the version at the time of the application system call, independent of any subsequent changes in the application’s buffer. A simple way in which the operating system can guarantee copy semantics is for the write() system call to copy the application data into a kernel buffer before returning control to the application. The disk write is performed from the kernel buffer, so that subsequent changes to the application buffer have no effect.

##Caching
A __cache__ is a region of fast memory that holds copies of data.

When the kernel receives a file I/O request, the kernel first accesses the buffer cache to see whether that region of the file is already available in main memory. If it is, a physical disk I/O can be avoided or deferred.

##Transforming I/O Requests to Hardware Operations
The figure suggests that an I/O operation requires a great many steps that together consume a tremendous number of CPU cycles.

1. A process issues a blocking read() system call to a file descriptor of a file that has been opened previously.
2. The system-call code in the kernel checks the parameters for correctness. In the case of input, if the data are already available in the buffer cache, the data are returned to the process, and the I/O request is completed.
3. Otherwise, a physical I/O must be performed. The process is removed from the run queue and is placed on the wait queue for the device, and the I/O request is scheduled. Eventually, the I/O subsystem sends the request to the device driver. Depending on the operating system, the request is sent via a subroutine call or an in-kernel message.
4. The device driver allocates kernel buffer space to receive the data and schedules the I/O. Eventually, the driver sends commands to the device controller by writing into the device-control registers.
5. The device controller operates the device hardware to perform the data transfer.
6. The driver may poll for status and data, or it may have set up a DMA transfer into kernel memory. We assume that the transfer is managed by a DMA controller, which generates an interrupt when the transfer completes.
7. The correct interrupt handler receives the interrupt via the interrupt- vector table, stores any necessary data, signals the device driver, and returns from the interrupt.
8. The device driver receives the signal, determines which I/O request has completed, determines the request’s status, and signals the kernel I/O subsystem that the request has been completed.
9. The kernel transfers data or return codes to the address space of the requesting process and moves the process from the wait queue back to the ready queue.
10. Moving the process to the ready queue unblocks the process. When the scheduler assigns the process to the CPU, the process resumes execution at the completion of the system call.










