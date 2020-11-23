[TOC]

# ISA

==教材第五版 附录A==

## ISA Classes

### By internal storage

这里的internal storage指的是处理器里面从内存/缓存存取的数据

* stack
* accumulator
* reg

### stack architecture

所有数据都存在内存

1. Push A: <img src="assets/image-20201012110833719.png" style="zoom: 33%;" />
2. Push B: <img src="assets/image-20201012110900057.png" style="zoom:33%;" />
3. Add: <img src="assets/image-20201012110917950.png" style="zoom:33%;" />
    * first operand removed from stack, second op <u>replaced</u> by the result
4. Pop C: <img src="assets/image-20201012110944442.png" style="zoom:33%;" />

### accumulator architecture

* one implicit operand: the accumulator (这里是已经读进来作为accumulator的A)
* one explicit operand: mem location (这里是还在内存里的B)

1. Load A: <img src="assets/image-20201012110731123.png" style="zoom:33%;" />
2. Add B: <img src="assets/image-20201012110642572.png" style="zoom:33%;" />
3. Store : <img src="assets/image-20201012111019157.png" style="zoom:33%;" />



### GPR

general-purpose register architecture

* Only explicit operands
    * registers
    * memory locations
* Operand access:
    * direct memory access
    * loaded into temporary storage first







#### Two Classes

* register-memory architecture
    * any instruction can access memory
    * C=A+B
        1. load r1, A: <img src="assets/image-20201012112506224.png" style="zoom:33%;" />
        2. add r2, r1, B: <img src="assets/image-20201012112533082.png" style="zoom:33%;" />
        3. store r3, C: <img src="assets/image-20201012112607673.png" style="zoom:33%;" />
* load-store architecture
    * only load and store instructions can access memory
    * <u>every new arch designed after 1980 uses a load-store architecture</u>
        * effieciency
    * C=A+B
        1. load r1, A: <img src="assets/image-20201012131334309.png" style="zoom:33%;" />
        2. load r2, B: <img src="assets/image-20201012131354060.png" style="zoom: 33%;" />
        3. add r3, r1, r2: <img src="assets/image-20201012131412757.png" style="zoom:33%;" />
        4. store r3, C: <img src="assets/image-20201012131425910.png" style="zoom:33%;" />

#### GPR Classification

![](assets/image-20201012113215678.png)

![](assets/image-20201012113343339.png)



# Steps of exec

==课后看ppt==

## Get Operands

### Interpret Mem Address

* 大小端
* 是否对齐

### Addressing Modes

* const
* reg
* memory location

![](assets/image-20201013141828209.png)

## Operate Operands



## Control flow instructions

### Addressing

* PC-relative addr
* Dynamic addr

### Branch

Conditional Branch

![](assets/image-20201013142549112.png)

## How do hardware understand instructions?

### Compiler Opt

# RISC

信息源熵：$\Large H = -\sum p_i\log p_i$