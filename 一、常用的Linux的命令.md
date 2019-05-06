# 一、常用的Linux的命令

## 1.1.top 整机命令

```bash
Processes: 308 total, 2 running, 306 sleeping, 1543 threads            22:39:15
Load Avg: 1.22, 1.31, 1.39  CPU usage: 4.0% user, 4.11% sys, 91.87% idle
SharedLibs: 153M resident, 44M data, 19M linkedit.
MemRegions: 82233 total, 2184M resident, 74M private, 1310M shared.
PhysMem: 8021M used (2397M wired), 168M unused.
VM: 1398G vsize, 1297M framework vsize, 61825612(0) swapins, 63621059(0) swapout
Networks: packets: 12710134/15G in, 13085032/1676M out.
Disks: 9233794/376G read, 5216946/303G written.

PID    COMMAND      %CPU TIME     #TH   #WQ  #PORTS MEM    PURG   CMPRS  PGRP
67098  top          11.0 00:00.38 1/1   0    25+    3812K+ 0B     0B     67098
67093  distnoted    0.0  00:00.01 2     1    32     1972K  0B     0B     67093
67092  secinitd     0.0  00:00.07 2     2    38     3352K  0B     0B     67092
67091  com.apple.ge 0.0  00:00.39 4     1    57     9000K  20K    0B     67091
67066  mdworker_sha 0.0  00:00.08 4     1    61     8004K  0B     0B     67066
67064  com.apple.iC 0.0  00:00.08 2     1    55     3328K  0B     0B     67064
67061  IMDPersisten 0.0  00:00.04 2     1    65     2716K  48K    0B     67061
67057  analyticsd   0.0  00:00.06 2     1    63     1920K  220K   0B     67057
67013  bash         0.0  00:00.00 1     0    21     668K   0B     0B     67013
67012  login        0.0  00:00.02 2     1    32     1084K  0B     0B     67012
67010  iTerm2       0.0  00:00.04 2     1    33     3152K  0B     0B     67010
```

其中：**Load Avg: 1.86, 1.65, 1.66**表示的是系统在1分钟，5分钟，15分钟的的平均负载值，三个数字的平均值高于60%就说明电脑的压力太大。

```bash
luokanguandeMBP:~ luokangyuan$ top
luokanguandeMBP:~ luokangyuan$ uptime
22:11  up 38 days, 22:40, 2 users, load averages: 2.96 2.05 1.83
```

## 1.2.vmstar CPU命令

```bash
[root@localhost /]# vmstat -n 2 3
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 279328   2108 132700    0    0  1898   166  307 1134  2  7 90  1  0
 0  0      0 279332   2108 132692    0    0     0     0   32   59  0  0 100  0  0
 0  0      0 279332   2108 132692    0    0     0     0   29   55  0  0 100  0  0
[root@localhost /]#
```

`vmstat`命令说明：第一个参数是指采样的时间间隔秒数，第二个参数是采样的次数。

结果说明：

* **procs：**
  * **r：**运行和等待`CPU`时间片的进程数目，当整个系统的运行队列数目超过`CPU` 的总核数的2倍。
  * **b：**等待资源数，比如等IO，等网络IO等。
* **CPU：**
  * **us：**用户进程消耗`CPU`时间百分比，`us`越高，用户进程消耗时间多。
  * **sy：**表示内核进程消耗的`CPU`时间百分比。

## 1.3.free 内存命令

```bash
[root@localhost /]# free
              total        used        free      shared  buff/cache   available
Mem:         498892       84948      278980        4620      134964      373584
Swap:        839676           0      839676
[root@localhost /]# free -m
              total        used        free      shared  buff/cache   available
Mem:            487          82         272           4         131         364
Swap:           819           0         819
[root@localhost /]# free -g
              total        used        free      shared  buff/cache   available
Mem:              0           0           0           0           0           0
Swap:             0           0           0
[root@localhost /]#
```

查看每一个进程的内存消耗命令：

```bash
[root@localhost /]# pidstat -p 5101 -r 2
```

## 1.4.df 硬盘命令

```bash
[root@localhost /]# df
文件系统                  1K-块    已用   可用 已用% 挂载点
/dev/mapper/centos-root 6486016 6170920 315096   96% /
devtmpfs                 237512       0 237512    0% /dev
tmpfs                    249444       0 249444    0% /dev/shm
tmpfs                    249444    4620 244824    2% /run
tmpfs                    249444       0 249444    0% /sys/fs/cgroup
/dev/sda1               1038336  134924 903412   13% /boot
tmpfs                     49892       0  49892    0% /run/user/0
[root@localhost /]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root  6.2G  5.9G  308M   96% /
devtmpfs                 232M     0  232M    0% /dev
tmpfs                    244M     0  244M    0% /dev/shm
tmpfs                    244M  4.6M  240M    2% /run
tmpfs                    244M     0  244M    0% /sys/fs/cgroup
/dev/sda1               1014M  132M  883M   13% /boot
tmpfs                     49M     0   49M    0% /run/user/0
[root@localhost /]#
```

## 1.5.iostat 磁盘IO命令

```bash
[root@localhost /]# iostat -xdk 2 3
Linux 3.10.0-957.el7.x86_64 (localhost.localdomain) 	2019年05月06日 	_x86_64_	(1 CPU)

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               0.02     0.07    4.47    0.56   121.51    20.95    56.59     0.02    3.75    3.14    8.57   1.33   0.67
dm-0              0.00     0.00    3.81    0.58   114.33    19.77    61.16     0.02    3.62    2.64   10.10   1.03   0.45
dm-1              0.00     0.00    0.05    0.00     1.42     0.00    54.67     0.00    0.48    0.48    0.00   0.39   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
dm-0              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
dm-1              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
dm-0              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
dm-1              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00

[root@localhost /]#
```

> 提示：如果出现 iostat: 未找到命令，使用`yum install sysstat`安装命令

```bash
[root@localhost /]# iostat
-bash: iostat: 未找到命令
```

*  rkB/s：每一秒读取数据量。
* wkB/s：每秒写入数据量。
* svctm：IO请求的平均服务时间，单位毫秒。
* await：IO请求的平均等待时间。如果await远高于svctm，则需要优化程序或者更换磁盘。

## 1.6.ifstat 网络命令

```bash
[root@localhost ifstat-1.1]# ifstat 1
#kernel
Interface        RX Pkts/Rate    TX Pkts/Rate    RX Data/Rate    TX Data/Rate
                 RX Errs/Drop    TX Errs/Drop    RX Over/Rate    TX Coll/Rate
```

