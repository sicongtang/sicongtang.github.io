title: JVM Internals
date: 2015-02-20 11:38:20
categories: JVM
---
##1. JVM Structure

{% img http://i.imgur.com/Ij8dKbY.png?1 jvm_architecture %}

##2. Class Loader SubSystem
* Loading: finding and importing the binary data for a type
* Linking: performing verification, preparation, and (optionally) resolution
  1. Verification: ensuring the correctness of the imported type
  2. Preparation: allocating memory for class variables and initializing the memory to default values
  3. Resolution: transforming symbolic references from the type into direct references.
* Initialization: invoking Java code that initializes class variables to their proper starting values.

Parent-Delegation Model
* Bootstrap Class Loader: $JAVA_HOME/jre/lib/rt.jar, controlled by -Xbootclasspath option
* Extension Class Loader: %JAVA_HOME/jre/lib/ext/*.jar, controlled by -Djava.ext.dirs system property
* Application/System Class Loader: $CLASSPATH, controlled by -classpath/-cp/$CLASSPATH
* User defined classloader: custom classpath [path on disk, a network address etc]

##3. Runtime data area
Runtime Data Areas are the memory areas assigned when the JVM program runs on the OS. The runtime data areas can be divided into 6 areas. Of the six, one PC Register, JVM Stack, and Native Method Stack are __created for one thread__. Heap, Method Area, and Runtime Constant Pool are __shared by all threads__.

##3.1.1 method area
Inside a Java Virtual Machine instance, information about loaded types is stored in a logical area of memory called the __method area__. When the Java Virtual Machine loads a type, it uses a class loader to locate the appropriate class file. The class loader reads in the class file--a linear stream of __binary data__-- and passes it to the virtual machine. The virtual machine extracts information about the type from the binary data and stores the information in the method area. Memory for class (static) variables declared in the class is also taken from the method area.

* Type Information
* The Constant Pool
* Field Information
* Method Information
* Class Variables
  - Class variables are shared among all instances of a class and can be accessed even in the absence of any instance. Constants (__class variables declared final__) are not treated in the same way as non-final class variables. Every type that uses a final class variable gets a copy of the constant value in its own constant pool.
* A Reference to Class ClassLoader
  - For those types loaded via a class loader object, the virtual machine must store a reference to the class loader object that loaded the type. The virtual machine uses this information during __dynamic linking__. When one type refers to another type, the virtual machine requests the referenced type from the same class loader that loaded the referencing type.
* A Reference to Class Class
  - class `Class` gives the running application access to the information stored in the method area
* Method Tables
  - For each non-abstract class a Java Virtual Machine loads, it could generate a method table and include it as part of the class information it stores in the method area. A method table is an array of direct references to all the instance methods that may be invoked on a class instance, including instance methods inherited from superclasses.

##3.1.2 runtime constant pool

##3.2 heap
The Heap is used to allocate __class instances and arrays__ at runtime. __Arrays and objects__ can never be stored on the stack because a frame is not designed to change in size after it has been created. The frame only stores references that point to objects or arrays on the heap. Unlike primitive variables and references in the local variable array (in each frame) objects are always stored on the heap so they are not removed when a method ends. Instead objects are only removed by the garbage collector.

##3.3 java stack
When the Java Virtual Machine invokes a Java method, it checks the class data to determine the number of words required by the method in the local variables and operand stack. It creates a stack frame of the proper size for the method and pushes it onto the Java stack.

The stack frame has three parts: local variables, operand stack, and frame data.

##3.4 ps register
Each thread of a running program has its own pc register, or program counter, which is created when the thread is started. The pc register is one word in size, so it can hold both a native pointer and a returnValue. As a thread executes a Java method, the pc register contains __the address of the current instruction__ being executed by the thread. An "address" can be a native pointer or an offset from the beginning of a methodís bytecodes. If a thread is executing a native method, the value of the pc register is undefined.

##3.5 native method stack

##4. Execution Engine
* Interpreter: Reads, interprets and executes the bytecode instructions one by one.
* JIT (Just-In-Time) compiler: The JIT compiler has been introduced to compensate for the disadvantages of the interpreter. The execution engine runs as an interpreter first, and at the appropriate time, the JIT compiler compiles the entire bytecode to change it to native code.

Oracle Hotspot VM uses a JIT compiler called Hotspot Compiler. It is called Hotspot because Hotspot Compiler searches the 'Hotspot' that requires compiling with the highest priority through profiling, and then it compiles the hotspot to native code. If the method that has the bytecode compiled is no longer frequently invoked, in other words, if the method is not the hotspot any more, the Hotspot VM removes the native code from the cache and runs in interpreter mode. The Hotspot VM is divided into the Server VM and the Client VM, and the two VMs use different JIT compilers.
