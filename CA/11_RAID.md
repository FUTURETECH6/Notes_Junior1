[TOC]

## RAID6

**Row-diagonal Parity**

加密方式：

* row: 00⊕11⊕22⊕33=r4
* diagonal: 01⊕11⊕31⊕r1=d1

支持两个故障盘的恢复，从只有一个故障的块开始依次使用diagonal恢复和row恢复，如下图就从disk3的0或者disk1的2开始

![](assets/image-20201228104206502.png)

