# The Transport Service ( 传输服务

* **Transport layer services** (传输服务) 
    * To provide efficient, reliable, and cost effective service to its users, normally processes in the application layer.
    * To make use of the services provided by the network layer.
* **The transport entity** (传输实体): the hardware and/or software within the transport layer that does the work. Its positions:
    * in the OS kernel, in a separate user process, in a library package bound to network applications, or
    * on the network interface card (NIC).



**OSI model** (注意右侧的unit的名字)

![OSI_model.png](assets/OSI_model.png)

* There are two types of transport services
    * Connection-oriented transport service
    * Connectionless transport service
* The **similarities** between transport services and network services
    * The connection-oriented service is similar to the connection oriented network service in many ways:
        * Establishment --> data transfer --> release;
        * Addressing;
        * Flow control
    * The connectionless transport service is also similar to the connectionless network service.
* The **differences** between transport services and network services. Why are there two distinct layers?
    * The transport code runs entirely on the user’s machines, but the network layer mostly runs on the routers which are usually operated by the carrier.
    * Network layer has problems (losing packets, router crashing, …)
    * The transport layer improves the QoS of the network layer.
    * \=\=\> The transport service is <u>more reliable</u> than the network service.
    * \=\=\> Application programmers can write code <u>according to a standard set of transport service primitives</u> and have these programs work on a wide variety of networks.

## Hypothetical Primitives

![](assets/image-20201202111807442.png)

## Berkeley Sockets

![](assets/image-20201202112620171.png)

# Elements of Transport Protocols ( 传输协议的若干问题

Transport protocol and data link protocol

* Similarities : Error control, sequencing, flow control,
* The key difference : environments the two protocols operate
    * Link layer: two routers communicate directly via a physical channel
    * Transport layer: physical channel --> entire network
* Other differences
    * Addressing:
        * Link: implicit
        * Transport: explicit
    * Connection establishment
        * Link: simple
        * Transport: complicated
    * Storage capacity:
        * Link: no
        * Transport: yes => a network can delay a packet
    * Buffering
        * Link: allocate a fixed number of buffers to each line
        * Transport: a large number of connections and variations in the bandwidth may require a dynamic and complicated approach.

## Addressing



## Connection Establishment

## Connection Release

## Error Control and Flow Control

## Multiplexing

## Crash Recovery

# Congestion Control ( 拥塞控制

# The Internet Transport Protocols (Internet 传输协议 ): UDP

# The Internet Transport Protocols (Internet 传输协议 ): TCP

# Performance Issues ( 性能问题

# Delay Tolerant Networking ( 容延网络