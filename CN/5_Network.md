[TOC]

网络层：进行逻辑地址寻址，实现不同网络之间的路径选择。

# Network Layer Design Issues

* **Store and Forward Packet Switching**
    * 存储转发数据包交换
    * A host with a packet to send transmits it to the nearest router.
        1. The packet is received, verified, and stored .
        2. Then it is forwarded to the next router.
        3. This step can be repeated many times.
        4. Finally the packet reaches the destination host.
* **Services Provided to the Transport Layer**
    * 为传输层提供的服务
    * 目标
        * 向上提供的服务应独立于路由器技术
        * 向传输层（网络层的上一层）屏蔽路由器的数量、类型和拓扑关系
        * 传输层可用的网络地址应该有统一的编址方案，甚至可以跨越LAN和WAN
    * 分类
        * Connection-less service ( 无连接服务 )):
            * 40+ years of experience with the computer network, unreliable internet, hosts doing error control and flow control.
        * Connection oriented service ( 面向连接服务 )):
            * 100+ years of experience with the worldwide
                telephone system, quality of service.
        * Connection=less + connection oriented services .
* **Implementation of Connectionless Service**
    * 无连接服务的实现
    * 在这样的网络中，数据包被称为数据报（datagram）
    * <img src="assets/image-20201112102513391.png" style="zoom:33%;" /><br /><img src="assets/image-20201112102525078.png" style="zoom:33%;" />
        * 由于知道了AEC路上有阻塞，所以later中将EF更新了
        * 通过路由算法进行路由表的管理和更新
* **Implementation of Connection Oriented Service**
    * 面向连接服务的实现
    * 在发送数据包之前，要在src和dest之间建立一个虚电路VC (virtual circuit)
    * <img src="assets/image-20201112102548751.png" style="zoom:50%;" />
* **Comparison of Virtual Circuit and Datagram Subnets**

# Routing Algorithm

距离的定义：跳数或物理距离或延时

1. The Optimality Principle ( 最优化原则
    * 对于路由器abc，若b在从a到c的最优路径上，则b到c的最佳路径必然也在这上面
    * 汇集树
2. **Shortest Path Routing ( 最短路径路由**
3. Flooding ( 泛洪路由
    * 向除了包来的端口以外的全部端口都发送
    * 优点：可靠性高（如战争场景）、每个路由器只需要知道自己的邻居
    * 缺点：overhead
4. **Distance Vector (DV) Routing ( 距离向量路由**
5. **Link State (LS) Routing ( 链路状态路由**
6. **Hierarchical Routing ( 分层路由**
7. Broadcast Routing ( 广播路由
8. Multicast Routing ( 组播路由
9. Anycast Routing ( 选播路由
10. Routing for Mobile Hosts ( 移动主机的路由
11. Routing in Ad Hoc Networks ( 自组织网络的路由



Forwarding和routing的区别：

* FWD：数据本身的传播
* Routing：路由表的更新

## Shortest Path

Dij

## Distance Vector

* 每个路由器上有个向量，每个item存到所有路由器的最优距离、跳到那个路由器的下一个路由器
* 每个路由器和邻居交换距离向量，对于每个dest，将距离更新为通过这个邻居到dest的最短路径（假设自己到邻居的距离是已知的，且不能不更新（比如某个站停机了，没更新会出问题）），就更新向量
    * <img src="assets/image-20201112110050371.png" style="zoom:80%;" />

### Count to infinity

将整个网络的最佳路径的寻找过程称为收敛（convergence）。

DV算法存在的问题：对于好消息的反应很迅速，但是对于坏消息的反应很慢

**Ex**. 当A忽然停机了，B还会从C那里得到错的距离信息

<img src="assets/image-20201112113017922.png" style="zoom:50%;" />

**Poisoned reverse**

* If C routes through B to get to A:
    * C tells B its distance to A is infinite
* will this completely solve count to infinity problem?
    * No
    * Can you give an example?

## Link State

设计思路（每个路由器必须完成的事情）：

1. 发现邻居节点，并了解其网络地址
    * 发送HELLO包，同时要保证自己的名字必须是全局唯一的
    * 
2. 设置到每个邻居节点的距离或成本度量值
    * 小网络可以让成本与带宽成反比，如1Gbps为1，100Mbps为10
    * 地理上分散的网络可以用延迟作为成本，使用ECHO数据包的往返时间/2
3. 构造一个包含所有获知的链路信息的包
    * 包含Seq（唯一）、Age（）和到每个邻居的延时
    * <img src="assets/image-20201112121031781.png" style="zoom: 25%;" />
4. 将这个包发给所有其他路由器，并接受来自所有其他路由器的包
5. 计算到每个其他路由器的最短路径

第四步每个路由器已经获得了完整的网络拓扑结构，只要使用Dij就可以找出自己到其他所有路由器的最短路。



## Hierarchical Routing

降低成本，由于路由表用的是

# Internetworking



# The Network Layer in the Internet