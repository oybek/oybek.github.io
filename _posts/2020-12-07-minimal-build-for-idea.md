---
layout: post
title: "Minimal build for Intellij IDEA"
tags: [hardware]
---

Laptops nowadays are very often used as desktop workstations. All my friends use their laptops
plugging them to 1 or more big monitors. And I used to do the same until the speed of my weak laptop began to annoy me.

It has become a problem. During slow compilation I was losing a lot of time when I was concentrated.

I got 2 solutions of my problem:
1. To buy more powerful laptop (which is quite expensive)
2. To build PC myself (much more cheap way)

So here are the list of basic parts of PC for comfortable programming:
* CPU (With integrated graphics)
* Motherboard (With m2 port or sata, and support for hybrid CPUs)
* RAM (About 8 or 16 Gb)
* SSD (NVME or Sata)

Tips: no need to buy graphics card, it is a waste of money in our case. Ram has to be 2 ranked or 2 sticks. One stick
with 1 rank will be the bottleneck in our system, cause CPU will use RAM also for graphics.
Of course, we will choose SSD it gives fast file access during compilation.

Here is the list of components I used and tested:

## CPU: AMD A8-9600

|-|-|
**Price** | About 3000 rub. (~40 $)
**Cores** | 4 (3.1-3.4 GHz)
**TDP** | Just 65W
**Shader cores** | 384 (900 MHz)

![](/images/cpu01.jpg)

## Motherboard: ASUS Prime a320m-k

|-|-|
**Price** | About 4000 rub. (~54 $)
**M2** | +

![](/images/mboard01.jpg)

## RAM: Kingston HyperX 8Gb (4x2) DDR4

|-|-|
**Price** | About 2000 rub. (~27 $)

![](/images/ram01.jpg)

## SSD: Samsung, Gigabyte or Kingston (240 Gb)

Other parts you could buy whichever you want. They don't affect system properties.

# Tests

The test stand is assembled (OS: Linux Mint 20 Ulyana)

![](/images/proto01.jpg)

And here are the test drive results:
0. You can use intellij idea very comfortably
1. Watch full hd 60 fps videos in youtube
2. Play CS GO with > 60 fps, StarCraft II and other such level games

The final price of the build is about 15000 rub (200$), and it is much
cheaper than buying a laptop.

Some benchmarks:

```bash
$ sysbench cpu --cpu-max-prime=200000 --num-threads=4 run
sysbench 1.0.18 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 4
Initializing random number generator from current time


Prime numbers limit: 200000

Initializing worker threads...

Threads started!

CPU speed:
    events per second:    87.56

General statistics:
    total time:                          10.0360s
    total number of events:              879

Latency (ms):
         min:                                   44.81
         avg:                                   45.61
         max:                                   80.86
         95th percentile:                       47.47
         sum:                                40087.96

Threads fairness:
    events (avg/stddev):           219.7500/1.30
    execution time (avg/stddev):   10.0220/0.01
```

```bash
$ sysbench memory run
sysbench 1.0.18 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 1
Initializing random number generator from current time


Running memory speed test with the following options:
  block size: 1KiB
  total size: 102400MiB
  operation: write
  scope: global

Initializing worker threads...

Threads started!

Total operations: 32251745 (3223461.03 per second)

31495.84 MiB transferred (3147.91 MiB/sec)


General statistics:
    total time:                          10.0002s
    total number of events:              32251745

Latency (ms):
         min:                                    0.00
         avg:                                    0.00
         max:                                    0.18
         95th percentile:                        0.00
         sum:                                 4142.32

Threads fairness:
    events (avg/stddev):           32251745.0000/0.00
    execution time (avg/stddev):   4.1423/0.00
```
