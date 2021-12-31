[TOC]

# 3 Sync Problems

**Semaphore**

```c
void wait(semaphore *S) {
    S->value--;
    if (S->value < 0) {  // 没位置了就加到waiting queue
        add this process to S->list;  // waiting queue
        block();
    }
}
void signal(semaphore *S) {
    S->value++;
    if (S->value <= 0) {  // 没东西了就从waiting queue里wake up一个
        remove a process P from S->list;  // wake up
        wakeup(P);
    }
}
```

## ==Bounded-buffer problem==

过桥问题：一个餐厅二十个人吃饭，只有一个门，一次进一个人。只需要考虑producer不需要考虑consumer

```c
# empty init to be 20， 用以对有N值的信号量同步
# mutex init to be 1， 用以对小的二值信号量的同步
# full init to 0

// producer
do {
    // produce an item
    ...
    wait(empty);  // empty->val--;
    wait(mutex);
    // add the item to the buffer，进门
    ...
    signal(mutex);
    signal(full);  // full->val++;
} while (TRUE)
```



(procedurer-cosumer problem)

https://www.studytonight.com/operating-system/bounded-buffer#

* Two processes, the producer and the consumer share N buffers
    * the producer generates data, puts it into the buffer
    * the consumer consumes data by removing it from the buffer
* The problem is to make sure:
    * the producer won’t try to add data into the buffer if its full
    * the consumer won’t try to remove data from an empty buffer
* Solution:
    * n buffers, each can hold one item
    * semaphore `mutex` initialized to the value 1
    * semaphore `full` initialized to the value 0
        * 满的buffer的个数
    * semaphore `empty` initialized to the value N
        * 空的buffer的个数

```c
/*
mutex = 1; full = 0; empty = N;
*/
// producer
do {
    // produce an item
    ...
    wait(empty);  // empty->val--;
    wait(mutex);  // 只有一个位置
    // add the item to the buffer
    ...
    signal(mutex);
    signal(full);  // full->val++;
} while (TRUE)

// cosumer
do {
    wait(full);
    wait(mutex);
    // remove an item from buffer
    ...
    signal(mutex);
    signal(empty);
    // consume the item
    ...
} while (TRUE);
```

## Readers-writers problem

A data set is shared among a number of concurrent processes

* readers: only read the data set; they do not perform any updates
* <u>writers: can both read and write</u>

The readers-writers problem:

* <u>allow multiple readers to read at the same time (shared access)</u>
* <u>only one single writer can access the shared data (exclusive access)</u>



要求

1. reader只会在有writer要写的情况下等待
2. 一旦writer准备要写，那么不会有新的reader开始读操作

要求导致的starvation问题

1. 要求一可能导致writer饥饿
2. 要求二可能导致reader饥饿



Solution:

* semaphore `mutex` initialized to 1
    * Only for确保对后两个的修改的原子性
* semaphore `wrt` initialized to 1
    * 给writer作为互斥信号量，仅会被第一个进入和最后一个退出CS的readers所使用
    * 如果被reader加到wait里，writer就得一直等到reader signal了为止
* integer `read_count` initialized to 0

```c
/*
mutex = 1; wrt = 1; read_count = 0;
*/
// writer
do {
    wait(wrt);
    //write the shared data
    ...
    signal(wrt);
} while(TRUE);

// reader
do {
    wait(mutex);
    readcount++;
    if (readcount == 1)  // 第一个进入的
        wait(wrt);
    signal(mutex)

    //reading data
    ...
    wait(mutex);
    readcount--;
    if (readcount == 0)  // 最后一个退出的
        signal(wrt);
    signal(mutex);
} while(TRUE);
```

## Dining-philosophers problem

有五位哲学家围坐在一张圆形餐桌旁。吃东西的时候，他们就停止思考，思考的时候也停止吃东西。假设哲学家必须用两只餐叉吃东西。他们只能使用自己左右手边的那两只餐叉。

哲学家从来不交谈，这就很危险，可能产生死锁，每个哲学家都拿着左手的餐叉，永远都在等右边的餐叉（或者相反）。

即使没有死锁，也有可能发生资源耗尽。例如，假设规定当哲学家等待另一只餐叉超过五分钟后就放下自己手里的那一只餐叉，并且再等五分钟后进行下一次尝试。这个策略消除了死锁（系统总会进入到下一个状态），但仍然有可能发生“活锁”。如果五位哲学家在完全相同的时刻进入餐厅，并同时拿起左边的餐叉，那么这些哲学家就会等待五分钟，同时放下手中的餐叉，再等五分钟，又同时拿起这些餐叉。

**最烂方案**

```c
// i-th philosopher
do {
    wait(chopstick[i]);
    wait(chopStick[(i+1)%5]);
    // eat
    signal(chopstick[i]);
    signal(chopstick[(i+1)%5]);
    // think
} while (TRUE); 
```

**防止死锁的方案** (看代码)

* 最多4个哲学家
* 仅左右筷子都空闲时才使用（并且拿起两只筷子的过程必须在critical section，防止拿起一只另一只被抢）
* 非对称解决方案：偶数的先拿右再拿左，奇数的先拿左再拿右
    * dphil_2[_sleep].c
* dphil_1[_sleep].c：sleep的情况即每个人都拿自己左边然后锁死的情况，没sleep也有锁死的概率



注意：没有deadlock并不代表没有starvation

# Linux Sync

* semaphores
    * on single-cpu system, spinlocks replaced by enabling/disabling kernel preemption
* Spinlocks
* atomic integers
* reader-writer locks

# POSIX Sync

* 配合上面哲学家吃饭的例程食用
* 编译要加`-pthread`

## mutex locks

Def

```c
int WINPTHREAD_API pthread_mutex_init(pthread_mutex_t *m, const pthread_mutexattr_t *a);
int WINPTHREAD_API pthread_mutex_lock(pthread_mutex_t *m);
int WINPTHREAD_API pthread_mutex_unlock(pthread_mutex_t *m);
```

Usage

```c
#include <pthread.h>
pthread_mutex_t mutex;

pthread_mutex_init(&mutex, NULL);
pthread_mutex_lock(mutex);
pthread_mutex_unlock(mutex);
```

## semaphores

### Named

Def

```c
/* yes, it returns a semaphore (or SEM_FAILED) */
sem_t * WINPTHREAD_SEMA_API sem_open(const char * name, int oflag, mode_t mode, unsigned int value);
```

Usage

```c
#include <fcntl.h>  // O_CREAT
#include <semaphore.h>
sem_t *sem;

sem = sem_open("SEM", O_CREAT, 0666, 1);
sem_wait(sem);
// Crtical Section
sem_post(sem);
```

### Unnamed

Def

```c
int WINPTHREAD_SEMA_API sem_init(sem_t * sem, int pshared, unsigned int value);
```

Usage

```c
#include <semaphore.h>
sem_t sem;

sem_init(&sem, 0, 1);
sem_wait(&sem);
// Crtical Section
sem_post(&sem);
```

## condition variable 

POSIX condition variables are associated with a POSIX mutex lock to provide mutual exclusion

Def

```c
typedef union
{
  struct __pthread_cond_s __data;
  char __size[__SIZEOF_PTHREAD_COND_T];
  __extension__ long long int __align;
} pthread_cond_t;

/* Wait for condition variable COND to be signaled or broadcast.
   MUTEX is assumed to be locked before.

   This function is a cancellation point and therefore not marked with
   __THROW.  */
extern int pthread_cond_wait (pthread_cond_t *__restrict __cond, pthread_mutex_t *__restrict __mutex)
     __nonnull ((1, 2));

/* Wake up one thread waiting for condition variable COND.  */
extern int pthread_cond_signal (pthread_cond_t *__cond)
     __THROWNL __nonnull ((1));
```

Usage

```c
// Global Init
pthread_mutex_t mutex;
pthread_cond_t cond_var;
pthread_mutex_init(&mutex, NULL);
pthread_cond_init(&cond_var, NULL);

// Thread1: wait for a==b
pthread_mutex_lock(&mutex);
while(a != b)
    pthread_cond_wait(&cond_var, &mutex);
pthread_mutex_unlock(&mutex);

// Thread2: signal Thread1
pthread_mutex_lock(&mutex);
a = b;
pthread_cond_signal(&cond_var);
pthread_mutex_unlock(&mutex);
```

