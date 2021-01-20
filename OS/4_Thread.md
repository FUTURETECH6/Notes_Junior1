[TOC]

# Basis

## Benefit

* multiple tasks of an application can be implemented by threads
    * e.g., update display, fetch data, spell checking, answer a network request
* process creation is heavy-weight while thread creation is lightweight, why?
    * 线程天生共享资源，不需要重拷贝
* threads can simplify code, increase efficiency
* Kernels are generally multithreaded

## Def

<u>A thread is an **independent** stream of instructions that can be scheduled to run as such by the kernel</u>



```mermaid
graph TB
subgraph singleThread
    0[funA]
    1[funB]
    6[PCB]
end

subgraph ThreadB
    4[funB]
    5[TCB]
end

subgraph ThreadA
    2[funA]
    3[TCB]
end

0-->2
1-->4
```



Process contains many states and resources
* code, heap, data, file handlers (including socket), IPC
* process ID, process group ID, user ID
* stack, registers, and program counter (PC)

Threads exist within the process, and shares its resources

* each thread has its own essential resources (per-thread resources): <u>**stack**(如果没有独立的栈，则T1和T2同时从funA调用funB就会混乱)</u>, **registers**, **program counter**, **thread-specific data**(由于资源是共享的，所以如果线程A要独占某个全局变量，就需要加上修饰符TLS(Thread Local Storage)，==但是这也不能阻止线程B通过dereference的方法来取得它)…==
* access to shared resources need to be synchronized

Threads are individually scheduled by the kernel

* each thread has its own independent flow of control
* each thread can be in any of the scheduling states



![](assets/image-20201012143010681.png)



## Benefits

* Responsiveness
    * multithreading an interactive application allows a program to continue running even part of it is blocked or performing a lengthy operation
* Resource sharing
    * sharing resources may result in efficient communication and high degree of cooperation. Threads share the resources and memory of the process by default.
* Economy
    * thread is more lightweight than processes: create and context switch
* Scalability
    * better utilization of multiprocessor architectures: running in parallel



## ~~Multi-thread Server Arch~~

<img src="assets/image-20201012143149570.png" style="zoom: 50%;" />

**Nginx**

预先创先好n个进程，每个进程创建m个线程的线程池。

遇到请求先发给一个线程分发的机制，然后会将请求通过队列丢到线程池，

![](assets/image-20201012143316787.png)

## Concurrency vs Parallelism

* Concurrency 并发
    * 目的是为了节省时间，有时候可能T1执行时要读内存，这时候就可以先进行T2
    * 并发考虑的是切分任务，将其改为可以并发的执行顺序
    * ![](assets/image-20201012143451871.png)
* Parallelism 并行
    * 强调同时执行，关注的是程序的结构是否能并行
    * 并行要考虑能否同时进行多个
    * ![](assets/image-20201012143510559.png)

**Ex**

* C![](assets/image-20201012144221706.png)
* P![image-20201012144232242](assets/image-20201012144232242.png)
* C+P![](assets/image-20201012144252880.png)
* Buffer(分发器): ![](assets/image-20201012144420683.png)

# Implementation

* *user threads* are supported above the kernel and managed without kernel support
    * ==three thread libraries: POSIX Pthreads, Win32 threads, and Java threads==
    * 从内核角度考虑，由于内核调度的基本单元是进程，因此用户态的线程不能作为被单独调度的实体
* *kernel threads* are supported and managed directly by the kernel
  * all contemporary OS supports kernel threads
  * 线程可以作为内核调度的最基本单元



## ~~Kernel Thread~~

* To make concurrency cheaper, the execution aspect of process is separated out into threads. As such, the OS now manages threads and processes. All thread operations are implemented in the kernel and the OS schedules all threads in the system. OS managed threads are called kernel-level threads
* In this method, the kernel knows about and manages the threads. No runtime system is needed in this case. <u>**Instead** of thread table in each process</u>, the kernel has a **thread table** that keeps track of all threads in the system. In addition, the kernel also maintains the **traditional process table** to keep track of processes. Operating Systems kernel provides system call to create and manage threads.



* Advantages
    * Because kernel has full knowledge of all threads, scheduler may decide to give more time to a process having large number of threads than process having small number of threads.
    * Kernel-level threads are especially good for applications that frequently block. (block的时候可以换到其他线程
* Disadvantages
    * The kernel-level threads are slow and inefficient. For instance, threads operations are hundreds of times slower than that of user-level threads. (从create和maintain的角度
    * Since kernel must manage and schedule threads as well as processes. It require a full thread control block (TCB) for each thread to maintain information about threads. As a result there is significant overhead and increased in kernel complexity.

## ~~User Thread~~

用进程的概念进行线程调度

* Kernel-Level threads make concurrency much cheaper than process because, much less state to allocate and initialize. However, for fine-grained concurrency, kernel-level threads still suffer from too much overhead. Thread operations still require system calls. Ideally, <u>we require thread operations to be as fast as a procedure call.</u> Kernel-Level threads have to be general to support the needs of all programmers, languages, runtimes, etc. For such fine grained concurrency we need still "cheaper" threads.
* To make threads <u>cheap and fast</u>, they need to be implemented at user level. User-Level threads are managed entirely by the run-time system (user-level library). <u>The kernel knows nothing about user-level threads and manages them as if they were single-threaded processes.</u> User-Level threads are small and fast, each thread is represented by a PC, register, stack, and small threadcontrol block. Creating a new thread, switching between threads, and synchronizing threads are done via procedure call. i.e no kernel involvement. User-Level threads are hundred times faster than Kernel-Level threads.



* Advantages:
    * The most obvious advantage of this technique is that a user-level threads package can be implemented on an Operating System that does not support threads.
    * User-level threads does not require modification to operating systems.
    * Simple Representation: each thread is represented simply by a PC, registers, stack and a small control block, all stored in the user process address space.
    * Simple Management: This simply means that creating a thread, switching between threads and synchronization between threads can all be done without intervention of the kernel.
    * Fast and Efficient: Thread switching is not much more expensive than a procedure call. (不需要sys call)
* Disadvantages
  * User-Level threads are not a perfect solution as with everything else, they are a trade off. Since, User-Level threads are invisible to the OS they are not well integrated with the OS. As a result, Os can make poor decisions like scheduling a process with idle threads, blocking a process whose thread initiated an I/O even though the process has other threads that can run and unscheduling a process with a thread holding a lock. Solving this requires communication between between kernel and user-level thread manager.
  * There is a lack of coordination between threads and operating system kernel. Therefore, process as whole gets one time slice irrespect of whether process has one thread or 1000 threads within. It is up to each thread to relinquish control to other threads. (内核和用户线程之间无法沟通)
  * User-level threads requires non-blocking systems call i.e., a multithreaded kernel.   Otherwise, entire process will blocked in the kernel, even if there are runnable threads left in the processes. For example, if one thread causes a page fault, the process blocks.

## Model between K th and U th（反正知道有不同的模型就好了）

A relationship must exist between user threads and kernel threads

Kernel threads are the real threads in the system, so for a user thread to make progress the user program has to have its scheduler take a user thread and then run it on a kernel thread.

### Many-to-one

Many user-level threads mapped to a single kernel thread
* thread management is done by the thread library in user space
(efficient)
* entire process will block if a thread makes a blocking system call
    * convert blocking system call to non-blocking (e.g., select in
    Unix)?
* multiple threads are unable to run in parallel on multi-processors

Examples:

* Solaris green threads

<img src="assets/image-20201012150633292.png" style="zoom:25%;" />

### One-to-one

Each user-level thread maps to one kernel thread
* it allows other threads to run when a thread blocks
* multiple thread can run in parallel on multiprocessors
* creating a user thread requires creating a corresponding kernel thread
* it leads to overhead
    * 一个UserT必须有KernelT
* most operating systems implementing this model limit the number of
threads

Examples

* Windows NT/XP/2000
* Linux

<img src="assets/image-20201012150747244.png" style="zoom:33%;" />

### Mant-to-many

* Many user level threads are mapped to many kernel threads
    * it solves the shortcomings of 1:1 and m:1 model
    * developers can create as many user threads as necessary
    * corresponding kernel threads can run in parallel on a multiprocessor
* Examples
    * Solaris prior to version 9
    * Windows NT/2000 with the ThreadFiber package

#### Two-level

Similar to many-to-many model, except that it allows a user thread to be bound to kernel thread.

<img src="assets/image-20201013000401606.png" style="zoom: 25%;" />

# Issues

* Semantics of `fork` and `exec` system calls
    * 是否要拷贝所有线程，是否要执行所有code
* Signal handling
    * 例如有线程注册了一个信号处理函数，那某个线程发出的异常的信号要发送到哪里？是自己的信号处理函数还是整个进程的信号处理函数
* Thread cancellation of target thread
* Thread-specific data
* Scheduler activations

## fork & exec

* Fork duplicates the whole single-threaded process
* Does fork duplicate only the calling thread or all threads for multithreaded process?
    * some UNIX systems have two versions of fork, one for each semantic
* <u>Exec typically replaces the entire process, multithreaded or not</u>
    * use “fork the calling thread” if calling exec soon after fork
* Which version of fork to use depends on the application
    * Exec is called immediately after forking: duplicating all threads is not necessary
    * Exec is not called: duplicating all threads

## Signal handling

* Signals are used in UNIX systems to notify a process that a particular event has occurred. It follows the same pattern
    * a signal is generated by the occurrence of a particular event
    * a signal is delivered to a process
    * once delivered, the signal must be handled
* Signal is handled by one of two signal handlers
    * default
    * user-defined
* Every signal has default handler that kernel runs when handling signal
    * User-defined signal handler can override default
    * For single-threaded, signal delivered to process
* Actually we use signal to kill a program
    * Kill: terminate or signal a process
        * Kill -9 pidxyz



* A signal can be synchronous (exceptions) or asynchronous (e.g., I/O)
    * synchronous signals are delivered to the same thread causing the signal
* Asynchronous signals(例如`kill -9`就是) can be delivered to:
    * the thread to which the signal applies
    * every thread in the process
    * certain threads in the process (signal masks)
    * a specific thread to receive all signals for the process



## ~~Thread Cancellation~~

* Thread cancellation: terminating a (target) thread **before** it has finished
    * does it cancel the target thread immediately or later?
* Two general approaches:
    * asynchronous cancellation: terminates the target thread immediately
        * what if the target thread is in critical section requesting resources?
        * 可能会导致其他线程资源异常
    * deferred cancellation: allows the target thread to **periodically** check if it should be cancelled (周期性看是否有人请求，有的话就先释放资源然后再自杀
        * Pthreads: cancellation point

。。。

## ~~Thread Specific Data~~

Different from local variables

* Local variables visible only during single function invocation
* TLS visible across function invocations

Similar to static data

* TLS is unique to each thread



<!--不用TLS如何实现：-->



## ~~LWP~~

LWP课本上的与linux的概念冲突，在Linux里指的是内核线程

M2M（？O2O）模型中在用户态和内核中间转换的，

# ~~OS Example~~

## WinXP

## Linux

* Linux has both fork and clone system call
* Clone accepts a set of flags which determine sharing between the parent and children
    * FS/VM/SIGHAND/FILES —> equivalent to thread creation
        * CLONE_FS, CLONE_VM, CLONE_SIGHAND, CLONE_FILES
    * no flag set no sharing (copy) —> equivalent to fork
        * fork可以带一些特殊的flag
    * Linux doesn’t distinguish between process and thread, uses term task rather than thread



Pthreads标准, linux里的实现是pthreads库



同一个func被多个线程调用存在的问题

## Java



# ~~LWP: Ex of linux T~~

• Threads have their own identity (thread ID), and can function independently.
• They share the address space within the process, and reap the benefits of avoiding any IPC (Inter-Process Communication) channel (shared memory, pipes and so on) to communicate.
• Threads of a process can directly communicate with each other
• for example, independent threads can access/update a global variable.
• This model eliminates the potential IPC overhead that the kernel would have had to incur. As threads are in the same address space, a thread context switch is inexpensive and fast. 避免使用IPC，天生共享资源

User-level threads in Linux follow the open POSIX (Portable Operating System Interface for uniX) standard, designated as IEEE 1003. The user-level library (on Ubuntu, glibc.so) has an implementation of the POSIX API for threads. • Threads exist in two separate execution spaces in Linux — in user space and the kernel. • User-space threads are created with the pthread library API (POSIX compliant). • In Linux, kernel threads are regarded as “light-weight processes”. An LWP is the unit of a basic execution context. Unlike other UNIX variants, including HP-UX and SunOS, there is no special treatment for threads. A process or a thread in Linux is treated as a “task”, and shares the same structure representation (list of struct task_structs). • These user-space threads are mapped to kernel threads.

```c
main{
    pthread_t tid = pthread_self();	 // 库实现的
    int sid = syscall(SYS_gettid);  // 全局唯一的，内核维护
}
```

* An LWP is a process created to facilitate a user-space thread. <u>Each user-thread has a 1×1 mapping to an LWP.</u>
* The creation of LWPs is different from an ordinary process; for a user process “P”, its set of LWPs share the same group ID. Grouping them allows the kernel to enable resource sharing among them (resources include the address space, physical memory pages (VM), signal handlers and files). This further enables the kernel to avoid context switches among these processes. Extensive resource sharing is the reason these processes are called light-weight processes.

## Use clone to create

* Linux handles LWPs via the non-standard clone() system call. It is similar to fork(), but more generic. Actually, fork() itself is a manifestation of clone(), which allows programmers to choose the resources to share between processes.
* The clone() call creates a process, but the child process shares its execution context with the parent, including the memory, file descriptors and signal handlers. The pthread library too uses clone() to implement threads. Refer to ./nptl/sysdeps/pthread/createthread.c in the glibc version 2.11.2 sources.

```c
NAME
       clone, __clone2 - create a child process

SYNOPSIS
       /* Prototype for the glibc wrapper function */

       #define _GNU_SOURCE
       #include <sched.h>

       int clone(int (*fn)(void *), void *child_stack,
                 int flags, void *arg, ...
                 /* pid_t *ptid, void *newtls, pid_t *ctid */ );
```

file descrptor 的表是per process的不是全局的，通过clone创建新thread，如果选择了共享fd，则子线程都不需要open了

## Flag

* `SIGCHLD`: The thread sends a `SIGCHLD` signal to the parent process after completion. It allows the parent to `wait()` forall its threads to complete.
* `CLONE_FS`: Shares the parent's filesystem information with its thread. This includes the root of the filesystem, the current working directory, and the umask.
* `CLONE_FILES`: The calling and caller process share the same file descriptor table. Any change in the table is reflected in the parent process and all its threads.
* `CLONE_SIGHAND` : Parent and threads share the same signal handler table. Again, if the parent or any thread modifies a signal action, it is reflected to both the parties.
* `CLONE_VM`: The parent and threads run in the same memory space. Any memory writes/mapping performed by any of them is visible to other process.

### CLONE_FILES

```c
#define _GNU_SOURCE  // clone
#include <fcntl.h>
#include <sched.h>  // clone
#include <stdio.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <unistd.h>  // close

// 64kB stack
#define STACK 1024 * 64

// The child thread will execute this function
int threadFunction(void *argument) {
    printf("child thread entering\n");
    close(*(int *)argument);
    printf("child thread exiting\n");
    return 0;
}

int main() {
    int fd = open("/dev/null", O_RDWR);  // 黑洞
    if (fd < 0) {
        perror("/dev/null");
        exit(1);
    }
    // Allocate the stack
    void *stack = malloc(STACK);
    if (stack == 0) {
        perror("malloc: could not allocate stack");
        exit(1);
    }
    printf("Creating child thread\n");
    // Call the clone system call to create the child thread
    pid_t pid = clone(&threadFunction, (char *)stack + STACK,  // 栈是向下为正的
        SIGCHLD | CLONE_FS | CLONE_FILES | CLONE_SIGHAND | CLONE_VM, (void *)&fd);

    if (pid == -1) {
        perror("clone");
        exit(2);
    }

    // Wait for the child thread to exit
    pid = waitpid(pid, 0, 0);
    if (pid == -1) {
        perror("waitpid");
        exit(3);
    }

    // Attempt to write to file should fail, since our thread has closed the file.
    if (write(fd, "c", 1) < 0)  // 子进程关闭了fd，共享也没用
        printf("Parent:\t child closed our file descriptor\n");
    else  // 如果把上面的CLONE_FILES关掉就可以写了，因为fd不再是共享的了
        printf("Parent:\t write to file successfully\n");

    // Free the stack
    free(stack);
    return 0;
}
```

注意第52行注释，关闭file descriptor共享后就可以了，因为clone的时候将fd给了子进程一份，fd关的是这份

### CLONE_VM

CLONE_VM 保证内存的共享

```c
#define _GNU_SOURCE  // clone
#include <sched.h>   // clone
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/wait.h>

static int child_func(void *arg) {
    char *buf = (char *)arg;
    printf("Child sees buf = \"%s\"\n", buf);
    strcpy(buf, "hello from child");  // 说明替换成功
    return 0;
}

int main(int argc, char **argv) {
    // Allocate stack for child task.
    const int STACK_SIZE = 65536;
    char *stack          = malloc(STACK_SIZE);
    if (!stack) {
        perror("malloc");
        exit(1);
    }
    unsigned long flags = 0;
    // When called with the command-line argument "vm", set the CLONE_VM flag on.
    if (argc > 1 && !strcmp(argv[1], "vm"))
        flags |= CLONE_VM;

    char buf[100];
    strcpy(buf, "hello from parent");
    if (clone(child_func, stack + STACK_SIZE, flags | SIGCHLD, buf) == -1) {
        perror("clone");
        exit(1);
    }
    int status;
    if (wait(&status) == -1) {
        perror("wait");
        exit(1);
    }
    printf("Child exited with status %d. buf = \"%s\"\n", status, buf);
    return 0;
}
```

输出

```sh
==== executing `gcc -o ./bin/VM.out ./VM.c -g` ====
==== executing `./bin/VM.out` ====
Child sees buf = "hello from parent"
Child exited with status 0. buf = "hello from parent"
==== executing `./bin/VM.out vm` ====
Child sees buf = "hello from parent"
Child exited with status 0. buf = "hello from child"
```



## COW

页表是per process的，父进程create子进程的时候

* fork process：不共享地址空间，需要把PT和内存都dup一份
    * 理论上：VA不变，PA和PT都拷贝
    * 实际上：只拷贝PT，两个地址都不变，有修改时才dup物理内存（CopyOnWrite机制，在PT上给PA加一个ReadOnly标记，有修改时发出exception，内核才来拷贝）
* clone thread：共享地址空间，连页表都是共享的



## TLS





### MultiProcessor

一份内核代码的资源是全局的，所以即使在多个CPU上执行内核程序（例如syscall），也要考虑同步的问题

》？为什么是全局的，cache L1不是core独占的吗