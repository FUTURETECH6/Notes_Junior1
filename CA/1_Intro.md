FA，SA，DM一般用于L1, L2, L3

DM在L3时BlockNum会很大



使用VM节省空间的原因：向OS申请空间时分配的是动态的，但是并不是一直占用的，如果整个被分配的空间都被标记为使用并且让某个程序独占，则会浪费很多物理空间；另外可以将空间碎片化

使用VM进行内存保护：通过映射防止越界访问(映射机制可以记录虚拟空间对应的物理地址)



shared mem/cache：并行更新的问题



**Amdahl's law**

> S is overall speedup, s is enhanced speedup, p is enhanced partiton

$S_{\text {latency }}(s)=\frac{1}{(1-p)+\frac{p}{s}}$

$\left\{\begin{array}{l}S_{\text {latency }}(s) \leq \frac{1}{1-p} \\ \lim _{s \rightarrow \infty} S_{\text {latency }}(s)=\frac{1}{1-p}\end{array}\right.$



Type

* Personal
* Server
    * Hight throughput
    * Scalability
* Clusters/WSC
    * price-performance
    * availability
    * power
* Embedded



**Parallelism**

* DataLP
    * CLA
    * Matrix Multiplication
* InstructionLP
    * Pipline
    * Specualtive exec: Do works in advance
* Vector Architeture, GPUs, Multimedia Instruction Sets
* ThreadLP
* RequestLP

yutr