# Basis

**Pipelining Terminology**

* *Latency*: the time for an instruction to complete.
* *Throughput* of a CPU: the number of instructions completed per second.
* Clock cycle: time duration of one lockstep 
    * everything in CPU moves in lockstep;
* Processor Cycle: time required between moving an instruction one step down the pipeline;
    	= time required to complete a pipe stage;
    	= max(times for completing all stages);
    	= one or two clock cycles, but rarely more.
* CPI: clock cycles per instruction