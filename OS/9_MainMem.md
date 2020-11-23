# Protection

## Hardware

* CPU must check every memory access generated in user mode to be sure it is between base and limit for that user
* the instructions to loading the base and limit registers are privileged

![](assets/image-20201112092440846.png)

## Address Binding

### Binding of Instructions and Data to Memory

* **source code** addresses are usually <u>symbolic</u> (e.g., temp)
* **compiler** binds symbols to <u>relocatable addresses</u>
    * e.g., “14 bytes from beginning of this module” (偏移量)
* **linker** (or loader) binds relocatable addresses to <u>absolute addresses</u>
    * e.g., 0x0e74014
* Each binding maps one address space to another

### MMU



### Dynamic Loading

swap



不需要加载整个库，只需要用stub+PLT定位即可



## Memory Allocation: Vaiable Partition

三种方法现在不重要了



### Fragmentation

碎片化

* 外部碎片化：空闲内存加起来够用，但是不连续
    * sol
        * 
        * 分页
* 内部碎片化：分配的比需要的还大（例如要500有502，为了不maintain2字节的信息，会把整个502一起分配；实际中都是以页为单位进行分配的，所以即使只有1B也得到1页）



### Paging

使内存在虚拟内存上连续（物理不一定连续）



页小：利于处理碎片化，但是页表的空间占用会变大

页大：DMA需要页大，由于内存空间很大



#### Frame Table

追踪管理哪些frame（物理）是已分配的



#### Free Frames



### Hardware Support



Alternative Way

* 页表在Mem里



#### TLB

用[associative memory](https://en.wikipedia.org/wiki/Content-addressable_memory)（并行查询优化）实现



由于TLB是硬件结构，进程切换的时候需要flush TLB。现代TLB中有[ASID](https://stackoverflow.com/questions/52713940/purpose-of-address-spaced-identifiersasids)会存储PID，直接

* 在Arm、x86等传统架构是由硬件flush的，在MIPS中有软中断抛出异常由软件实现（硬件结构简单）



#### EAT

**Effective Access Time**





### Memroy Protecion of Page



其他protection位：NX, PXN (Privilege Exec Never)



## Page Sharing

例如对于libc，里面有code段、data段，其中code段是x的，data段是rw的。且data段在不同进程之间不可共享（通过page的隔离实现）



double mapping的问题：P1的code段在P2是rw的，危险



## PT Hierarchy

**Multilevel Paging**

* 节省空间
    * 如果设计得当，一级页表可以存在一个页里，因此每次只要访问一个页就能获得一级的Frame Number



**Hashed PT**

用的是链表的那种（如果不是的话那个两倍的要求会导致页表太大）



**Inverted PT**

由Frame->Page的索引，要求物理内存比较小（因为要遍历整个页表）