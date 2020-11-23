# Cache

<img src="https://miro.medium.com/max/795/1*BE7wrx1HjlHiTBULXnAiUA.png">

[SRAM and DRAM (SDRAM). Everyone of us are familiar with RAM… | by Arunkumar Krishnan | Medium](https://medium.com/@emailarunkumar/sram-and-dram-sdram-9b6d01f09eb7)



**Cache Locality**

* Temporal locality 
    * need the requested word again soon
* Spatial locality
    * likely need other data in the block soon



**Cache Miss**

Time required for cache miss depends on: 

* **Latency**: the time to retrieve the first word of the block (寻址)
    * depends on queueing, cpu scheduling, dram scheduling, etc…
* **Bandwidth**: the time to retrieve the rest of this block
    * depends on how many data can be transmitted per time unit, so the more data can transmit per time unit, the faster to transmit a fixed amount of data.
* 优化时两者基本上没有关系



**Cache Miss Metrics**

* Memory stall cycles per mem access
    * the number of cycles during processor is stalled waiting for a mem access
* Miss rate
    * number of misses over 
    * number of accesses
* Miss penalty
    * the cost per miss (number of extra clock cycles to wait)



**Cache Performance**