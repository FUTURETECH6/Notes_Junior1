[TOC]

# Basis

## VMem Alloc (Pg&Seg)

* Paged virtual memory

    * page: fixed-size block
    * #Page | Offset

* Segmented virtual memory(类似于OS中的Contiguous Allocation)

    * segment: variable-size block
    * #Seg + Offset

* <div align="left"><img src="assets/image-20201207115836331.png" style="zoom:35%;" /></div>

自己看[OS的笔记](../OS/9_MainMem.md)吧



**Paged Segments**

混合式策略

* Segment = a number of pages
* Need not be contiguous
* Simplify replacement

## Four Q

### Placement

允许全相联，因为VM miss的代价实在太高了（进磁盘）

### Finding

PA = {VPN\=\=\>PPN | VPO\=\=PPO}

反转式页表

### Replacement on VM miss

### Wriet

# TLB

# Page Size

# Protection