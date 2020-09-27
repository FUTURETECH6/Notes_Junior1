建立在传输媒介之上的层

* Guided Transmission Media (Terrestrial 大地的
* Wireless Transmission (Terrestrial 大地的
* Digital Modulation and Multiplexing
    * OFDM

## Fourier Analysis

## Data Com

带宽：最高频的减最低频的，例如人说话的带宽是20000-20Hz，在这里指的是傅里叶展开后的最高频和最低频

bandwidth-limited：

截止(cutoff)频率：因为bandwidth有限，所以不能无限传输



Given a bit rate b bits/sec,

* the time to send 1 bit is 1/b ,
* the time required to send n bits is n/b sec,
* the frequency of the m-th harmonic ( 第m次谐波 ) is $\large\frac{mb}{n}$



> Denotation: B: data rate (bits/s), S: symbol rate (symbol/s),

**Nyquist** (no noise)

Max data rate = 2H•log~2~V bps
Max symbol rate = 2H ps
where H is the bandwith, V is the discrete levels(0, 1, 2, ..., V-1)



**Shannon** (with random noise)

Max data rate = H•log~2~(1+S/N) bps
dB = 10•lg(S/N)
where S/N is the signal to noise ratio(信噪比, 也可直接用xx dB来表示)
\\	S/N=10->10dB, S/N=100->20dB, ...

## Guided Trans Media

### Magnetic media (磁介质)

将数据装磁盘，磁盘装卡车，卡车高速路上跑，带宽高得很，数据中心的迁移就可以这么搞

### Twisted pair (双绞线)

UTP是没有屏蔽层的

### Coaxial cable (同轴电缆)

早期📺，已淘汰

### Power Lines (电力线)

电线复用

### Fiber optics (光纤)

