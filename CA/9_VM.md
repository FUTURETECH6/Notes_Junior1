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

### Write

# TLB

## ==Addr Translation==

==TLB存的是PT entry==

![](assets/image-20201221100713616.png)

The page size is 8 KiB. The TLB is direct mapped with 256 entries（\#index = 8）. The L1 cache is a direct-mapped 8 KiB（\#index = 7）, and the L2 cache is a direct-mapped 4 MiB(\#index = 16). Both use 64-byte blocks（\#off = 6）. The virtual address is 64 bits and the physical address is 41 bits. The primary difference between this simple figure and a real cache is replication of pieces of this figure.

==Virtually-indexed physically-tagged==：在虚拟地址翻译成物理地址之前，<u>访问L1 cache的部分可以先开始</u>（<u>而本来应该是用TLB（miss就从页表里fetch）获取PA然后再开始L1的</u>）。而所谓的physically-tagged指的是L1的tag就直接用PPN了（如果是virtually-tagged则是VPN），由于PO中包含了L1 index，所以L1的访问可以和TLB的访问同时进行

* *勘误* L1 cache tag应该是28位的（PA_size - PNO_size=41-13，即PPN（注意是物理地址而不是虚拟地址的）），如果TLB的tag unmatch（TLB miss）可以用这个去组成PA
* 本来应该是TLB hit之后拿PA去mem找数据的，但由于cache的存在，可以先用PPN去cache找一波，如果找到就可以省去访问mem
* ==如果TLB miss的话是要读内存刷新之后再读的，而不是直接去内存读数据==
* 到了L2就已经有PA了，按老方式即可

Requirements

* L1 cache
    * L1\_entry\_num = page\_entry\_num = 2^7^
    * 因为cache是用phy addr去编址的，所以不能用VPN来寻，只能用PPO=VPO中的高8位作为L1_cache的index先去试一次，这才要求有上面那个条件
* L2 cache
    * L2时已经得到PA(来自TLB(re-fetch or not))了，因此可以把整个PA用来寻数据



> [英文课本第六版](../../CA/%E8%B5%84%E6%96%99/%E8%AE%A1%E7%AE%97%E6%9C%BA%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E9%87%8F%E5%8C%96%E6%96%B9%E6%B3%95%E7%AC%AC%E5%85%AD%E7%89%88%E8%8B%B1%E6%96%87%E7%89%88.pdf) 754
>
> First, the 64-bit virtual address is logically divided into a virtual page number and page offset. The former is sent to the TLB to be translated into a physical address, and the high bit of the latter is sent to the L1 cache to act as an index. <u>If the TLB match is a hit, then the **physical page number** is sent to the L1 cache tag to check for a match.</u> If it matches, it’s an L1 cache hit. The block offset then selects the word for the processor.
>
> If the L1 cache check results in a miss, the physical address is then used to try the L2 cache. The middle portion of the physical address is used as an index to the 4 MiB L2 cache. The resulting L2 cache tag is compared with the upper part of the physical address to check for a match. If it matches, we have an L2 cache hit, and the data are sent to the processor, which uses the block offset to select the desired word. On an L2 miss, the physical address is then used to get the block from memory.



[CPU cache - Wikipedia](https://en.wikipedia.org/wiki/CPU_cache#Address_translation)



## ==Vir & Phy Tagged==

* Pysically tagged
    * given that each data block has a unique physical address;
    * match tags using physical addresses;
* Virtually tagged
    * index cache sets and match tags using virtual address;
    * shared data may have different virtual addresses: aliasing

# Page Size

# Protection