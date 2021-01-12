[TOC]

# Evolving tredns

## Tech

* Integrated circuit logic technology
* Semiconductor DRAM
* Semiconductor Flash
* Magnetic disk technology
* Network technology: Switches(交换机), Transmission system

**Evaluation Performance**

* Bandwidth/Througput(megabytes per second for a disk transfer)
* Latency/Response Time(millionseconds for a disk access)

## Power

* Dynamic: 猜测是需要刷新的，比如DRAM
* Static: Need power to maintain the storage values (e.g., SRAM)

$\large{\text{Dynamic energy} = \begin{cases} CU^2 & 1\rightarrow0\rightarrow1 \text{ or } 0\rightarrow1\rightarrow0 \\ \frac{CU^2}2 & 1\rightarrow0 \text{ or } 0\rightarrow1 \end{cases} \\ \text{Dynamic power} = \frac{CU^2f}2 \text{, where }f\text{ is the freq of switched} \\ \text{Static power = IU}}$

**Improove Enengy Efficiency**

* Do nothing: turn off inacive modules
* Dynamic volage-frequency scaling: scale down clk freq and voltage during periods of low ativity
* Design for typical case: PMD often idle mem and flash
* Overclocking: Turbo

## Cost

Price = Cost + Margin

Margin: R&D, marketing, sales, manufacturing equipment maintenance, building rental, cost of financing, pretax profits, and taxes

## Dependenbility/Reliability

MTB(etween)F(ailures) = MTTFailure + MTTRepair

$\text{Module availability} = \frac{\text{MTTF}}{\text{MTTF}+\text{MTTR}}$，单位：FIT(Failer per <u>billion</u> hours)

### Example

**Ex.1**

> Assume a disk subsystem with the following components and MTTF:
>
> * 10 disks, each read at 10^6^-hour MTTF;
> * 1 ATA controller, 500,000-hour MTTF;
> * 1 power supply, 200,000-hour MTTF;
> * 1 fan, 200,000-hour MTTF; 
> * 1 ATA cable, 1,000,000-hour MTTF;
>
> Assume expo-distributed lifetimes and independent failures
>
> Compute MTTF of the whole system?

The sum of the failure rates:
$$
\begin{aligned}
\text { Failure rate }_{\text {system }} &=10 \times \frac{1}{1,000,000}+\frac{1}{500,000}+\frac{1}{200,000}+\frac{1}{200,000}+\frac{1}{1,000,000} \\
&=\frac{10+2+5+5+1}{1,000,000 \text { hours }}=\frac{23}{1,000,000}=\frac{23,000}{1,000,000,000 \text { hours }}
\end{aligned}
$$
The MTIF of the system:
$$
\text { MTTF }_{\text {system }}=\frac{1}{\text { Failure rate }_{\text {system }}}=\frac{1,000,000,000 \text { hours }}{23,000}=43,500 \text { hours }
$$
**Ex.2**

> Assume 1000 disks with a 10,000,000-hour MTTF and used 24hr/day, upon disk failures, replace with disks of the same dependability;
>
> The number of failed disks per year?

$$
\begin{aligned}
\text { Failed disks } &=\frac{\text { Number of disks } \times \text { Time period }}{\text { MTTF }} \\
&=\frac{1000 \text { disks } \times 8760 \text { hours } / \text { drive }}{1,000,000 \text { hours } / \text { failure }}=9
\end{aligned}
$$

从概率的角度来说单个硬盘失效是小概率事件(8760/10^6^)，因此不考虑换掉一个之后再坏掉的情况

**Ex.3** 使用备用电源电源

中文课本实际第45页

> 添加条件：$\text{MTTR}_\text{power} = 24\text{ hours}$

$$
{
    \begin{array}{}
        \text{MTTF}_\text{pair} & = \frac{\text{MTTF}_\text{power}/2}{\text{MTTR}_\text{power}/\text{MTTF}_\text{power}} = \frac{\text{MTTF}_\text{power}^2}{2 \times \text{MTTR}_\text{power}} 
        \\
        &= \frac{200,000^2}{2\times24} = 833,333,333\text{ hours}
        \\
        \text { MTTF }_{\text {sys}} & = \frac{1}{\frac{23}{1,000,000} - \frac{1}{200,000} + \frac{48}{200,000^2}} = 55,551\text{ hours}
    \end{array}
}
$$



# Measure Performance

* Execution/response time
    * the time between the start and the completion of an event
* Throughput
    * the total amount of work done in a given time



X is *n* times faster than Y, if 
$$
n=\frac{\text { Execution time }_{\mathrm{Y}}}{\text { Execution time }_{\mathrm{X}}}
=\frac{\frac1{\text { Performance }_{\mathrm{Y}}}}{\frac1{\text { Performance }_{\mathrm{X}}}}
=\frac{\text { Performance }_{\mathrm{X}}}{\text { Performance }_{\mathrm{Y}}} 
$$


# <u>Quantitative Principles</u>

* **Parallelism**

    * ILP: Instruction-Level Parallelism
    * Vector Architectures, GPUs, and MM
    * TLP: Thread-Level Parallelism
    * RLP: Request-Level Parallelism

* **Locality** (A program spends 90% of its execution time in only 10% of its code)

  * **temporal locality:** recently accessed items are likely to be accessed in the near future;
  * **spatial locality:** items whose addresses are near one another tend to be referenced close together in time

## Focus on the Common Case

### **Amdahl‘s Law** 

ref [chap1](./1_Intro.md)

> S is overall speedup, s is enhanced speedup, p is enhanced partiton

$\large S_{\text {latency }}(s)=\frac{1}{(1-p)+\frac{p}{s}}$

$\left\{\begin{array}{l}S_{\text {latency }}(s) \leq \frac{1}{1-p} \\ \lim _{s \rightarrow \infty} S_{\text {latency }}(s)=\frac{1}{1-p}\end{array}\right.$

👆speedup是指加速的倍速，没有单位 ==新的/旧的 而不是 新的/旧的-1==



1. Fraction~enhanced~: 
	* e.g., 20/60 if 20 seconds out of a 60-second program to enhance
2. Speedup~enhanced~:
  * e.g., 5/2 if enhanced to 2 seconds while originally 5 seconds

$$
\begin{array}{rcl}
    \text { Execution time }_{\text {new }}
    &=&
    \text { Execution time }_{\text {old }} \times\left(\left(1-\text { Fraction }_{\text {enhanced }}\right)+\frac{\text { Fraction }_{\text {enhanced }}}{\text { Speedup }_{\text {enhanced }}}\right)
    \\
    \text { Speedup }_{\text {overall }}
    &=&
    \frac{\text { Execution time }_{\text {old }}}{\text { Execution time }_{\text {new }}}=\frac{1}{\left(1-\text { Fraction }_{\text {enhanced }}\right)+\frac{\text { Fraction }_{\text {enhanced }}}{\text { Speedup }_{\text {enhanced }}}}
\end{array}
$$

#### Example

**Ex.1**

> to enhance the processor for web serv;
>
> * new processor w/10 times faster comp;
> * 40% computation and 60% I/O the infrequent case
>
> overall speedup?

$$
\begin{aligned}
\text { Speedup }_{\text {overall }}=\frac{\text { Execution time }_{\text {old }}}{\text { Execution time }_{\text {new }}} &=\frac{1}{\left(1-\text { Fraction }_{\text {enhanced }}\right)+\frac{\text { Fraction }_{\text {enhanced }}}{\text { Speedup }_{\text {enhanced }}}} \\
&=\frac{1}{0.6+\frac{0.4}{10}}=\frac{1}{0.64} \approx 1.56
\end{aligned}
$$

**Ex.2**

> 20% FP(floating-point) square root for graphic apps;
>
> * design 1: improve hw(hardware) for 10x speedup;
> * design 2: for all FP instructions (50% of the execution time), improve by 1.6x;
>
> vote for design 1 or design 2?

$$
\begin{array}{l}
\text { Speedup }_{\mathrm{FSQRT}}=\frac{1}{(1-0.2)+\frac{0.2}{10}}=\frac{1}{0.82}=1.22 \\
\text { Speedup }_{\mathrm{FP}}=\frac{1}{(1-0.5)+\frac{0.5}{1.6}}=\frac{1}{0.8125}=1.23
\end{array}
$$

## Processor Performance

$\Large \rm \frac{秒数}{程序}=\frac{指令数}{程序} \times \frac{时钟周期数}{指令数}(CPI) \times \frac{秒数}{时钟周期数}(clk\_time)$

