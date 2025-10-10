---
title: "记一次文件系统性能测试"
date: 2023-11-27
author: morsuning
categories: ["Share"]
tags: ["Storage"]
---
记录一次视频监控场景下对文件系统进行性能测试及简单调优的过程及思路，相关内容已脱敏

## 1 性能分析方法

可通过日志和检测工具来监控基于fuse实现的用户态文件系统性能

以JuiceFS为例，它提供了访问日志和性能日志以及命令行工具监控文件系统性能，使用方式分别如下：

```shell
$ cat /path/to/your/.accesslog

$ cat /path/to/your/.perfmetric

$ juicefs stats /path/to/your
```

## 2 性能分析思路

在视频监控场景下，文件为多路监源连续追加写入文件系统

假设网络无瓶颈，文件系统内所有存储介质写入速率一致，理想情况下最大带宽：

- 单节点：服务端从本地写入JuiceFS速度为该节点的最大写入速度
- 3节点：期望最大写入速度为单节点最大写入速度 * 3

测试得服务端单节点从本地写入JuiceFS速度如下：

```plaintext
fiotest: (g=0):rw=write,bs=(R)16.0MiB-16.0MiB, (W) 16.0MiB-16.0MiB, (T) 16.0MiB-16.0MiB,ioengine=libaio, iodepth=1
fio-3.29
Starting 1 process
fiotest: Laying out I0 file (1 file / 4096MiB)
Jobs: 1 (f=1): [W(1)][100.0%][w=256MiB/s][w=16 IOPS][eta 00m:00s]
fiotest: (groupid=0, jobs=1): err= 0: pid=528243: Sun Nov 26 02:26:37 2023
write: IOPS=16, BW=257MiB/s (269MB/s)(4096MiB/15937msec); 0 zone resets
slat (usec): min=8557, max=18135, avg=9782.97, stdev=718.53
clat (nsec) : min=2590, max=19341, avg=6322.75, stdev=2224.38
lat (usec): min=8560, max=18145, avg=9791.82, stdev=719.24
clat percentiles (nsec):
1.00th=[ 2928].
5.00th=[ 3376], 10.00th=[ 3856], 20.00th=[ 4448],
30.00th=[ 5152],
40.00th=[ 5728], 50.00th=[ 6176], 60.00th=[ 6624],
70.00th=[ 7264], 80.00th=[ 7776],
90.00th=[ 8512], 95.00th=[ 8896],
99.00th=[15936],
99.50th=[18560], 99.90th=[19328], 99.95th=[19328],
99.99th=[19328]
bw ( KiB/s) : min=229376, max=294912, per=100.00%, avg=263201.03, stdev=15791.93, samples=31
iops: min= 14, max= 18, avg=16.06, stdev= 0.96, samples=31
lat (usec)
: 4=11.33%, 10=85.16%, 20=3.52%
fsync/fdatasync/sync_file_range:
sync (nsec) : min=220, max=2550, avg=1182.90, stdev=474.05
sync percentiles (nsec):
1.00th[3301],
5.00th[410], 10.00th[524], 20.00th[700],
30.00th[900], 40.00th[1064], 50.00th[1176],
60.00th[1336], 70. 00th[1432],
80.00th[1608], 90.00th[1848], 95.00th[2008],
99.00th[2192],
99.50th[2352], 99.90th[2544], 99.95th[2544],
99.99th[2544]
cpu
usr=1.26%, sys=0.51%, ctx=4608, majf=0, minf=11
IO depths
: 1=199.6%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >64=0.0%
submit
: 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >64=0.0%
complete
: 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >64=0.0%
issued rwts: total=0, 256, 0, 255 short=0, 0, 0, 0 dropped=0, 0, 0, 0
latency
: target=0, window=0, percentile=100.00%, depth=1
Run status group 0 (all jobs):
WRITE: bw=257MiB/s (269MB/s), 257MiB/s-257MiB/s (269MB/s-269MB/s), io=4096MiB (4295MB), run=15937-15937msec
```

**io路径分析**

nfs → 通过前端网络 → JuiceFS → 通过后端网络 → 分布式持久层 → 磁盘

nfs向JuiceFS并发发送io请求，JuiceFS将收到的数据暂写入缓存，达到指定大小后写缓存分布式持久层，分布式持久层返回后该次落盘操作才算成功

因此上述过程中存在2个时延，第一个时延为JuiceFS等待io请求，直到io请求达到指定大小；第二个时延为JuiceFS等待分布式持久层返回

理论上这两个时延时间一致且周期一致，性能最优

## 3 性能分析环境配置及操作

### 3.1 测试环境

- 服务端配置：曙光Rack/R6440H0 * 3; 2U 32Core; RAM 128G
- 网络：fio和mdtest测试：3服务端，1客户端，均组bond6_front，网络最大带宽20G/s

### 3.2 测试配置

首先JuiceFS在块大小 >= 16m时会直接将数据写对象，因此测试块大小16M的文件写入时需要调大写对象块大小上限，同时打开文件元数据缓存和目录项缓存超时时间来提高连续写入效率

需修改的配置

```yaml
juicefs:
  cacheDir: /var/powercache
  uploadDelay: 10m
  trashDays: 0
  logDir: "/var/log/juicefs"
  xxxClientPackagePath: "/opt/xxx/xxx_client.tar"
  xxxClientPort: 33999
  backupMeta: 24h
  attr-cache: 1 # 修改项
  entry-cache: 1 # 修改项
  dir-entry-cache: 1 # 修改项
  namespaceRootDir: "/xxx"
  disable-lock: false
  cache-type:
    free-ratio-raw: 0.15
    free-ratio-staging: 0.10
  extendedParams : "--upload-block-size 32 max-uploads 32 --flush-by-no-write 100" # 修改项
  bucketName: xxx-03e89688
```

测试前准备

1. 在服务端view上共享1个NFS文件夹nfstest
2. 在客户端挂载该目录: `mount -t nfs -o vers=3 业务网:/xxx/nfstest /mnt/nfstest`
3. 在客户端执行fio和mdtest进行测试

### 3.3 测试操作

1. 执行cp复制3.5G文件结果

```shell
[root@node1]# time cp openEuler-22.03-LTS-SP1-x86_64-dvd.iso /mnt/nfstest/testos
```

* real 0m8.210s
* user 0m0.000s
* sys 0m2.236s

```shell
[root@node1]# ls -1lh openEuler-22.03-LTS-SP1-x86_64-dvd.iso
```

root root 3.5G Nov 25 09:58 openEuler-22.03-LTS-SP1-x86_64-dvd.iso

2. 执行2个fio测试，参数及结果如下

   (1)4K随机写入:

```shell
fio -directory=/mnt/nfstest/test1 -ioengine=libaio -iodepth=1 -direct=1 -fsync=1 -bs=4K -flesize=8M --rw=write -numjobs=56 -name=fiotest -group_reporting -output=4k_randw.data
```

结果：

```plaintext
fiotest: (groupid=0, jobs=56): err= 0: pid=1668325: Sat Nov 25 16:48:17 2023
write: I0PS=2444, BW=9778KiB/s (10.0MB/s)(448MiB/46919msec); 0 zone resets
slat (nsec): min=1551, max=q7180, avg=3033.72, stdev=2019.74
clat (msec): min=6, max=438, avg=21.69, stdev=22.42
Lat (msec): min=6, max=438, avg=21.70, stdev=22.42
clat percentiles (msec):
1 1.00th=[q], 5.00th=[11], 10.00th=[12], 20.00th=[13],
30.00th=[15], 40.00th=[16], 50.00th=[18], 60.00th=[20],
70.00th=[22], 80.00th=[25], 90.00th=[34], 95.00th=[42],
99.00th=[79], 99.50th=[220], 99.90th=[239], 99.95th=[251],
99.99th=[430]
bw ( KiB/s): 
min= 1752, max=19184, per=100.00%, avg=10366.19, stdev=53.86,
samples=4947
iops
: 
min= 438, max= 4796, avg=2591.49, stdev=13.46, samples=4947
lat (msec)
: 10=4.31%，20=60.64%，50=32.90%，100=1.18%，250=0.90%
lat (msec)
: 500=0.05%
fsync/fdatasync/sync_file_range:
sync (nsec): min=16, max=4702, avg=40.81, stdev=30.54
sync percentiles (nsec):
1.00th=[22]， 5.00th=[23], 10.00th=[26], 20.00th=[32],
30.00th=[33], 40.00th=[34], 50.00th=[34], 60.00th=[39]
70.00th=[43],80.00th=[47], 90.00th=[54], 95.00th=[64],
99.00th=[145], 99.50th=[151], 99.90th=[193]，99.95th=[338],
99.99th=[532]
cpu
: usr=0.01%, sys=0.03%, ctx=114944, majf=0, minf=56
I0 depths 
: 1=200.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
submit
: 0=0.0%， 4=100.0%， 8=0.0%， 16=0.0%， 32=0.0%， 64=0.0%， >=64=0.0%
complete : 0=0.0%， 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
issued rwts: total=0,114688,0,114632 short=0,0,0,0 dropped=0,0,0,0
Latency
 : target=0, window=0, percentile=100.00%, depth=1
Run status group 0 (all jobs):
WRITE: bw=9778KiB/s (10.0MB/s), 9778KiB/s-9778KiB/s (10.0MB/s-10.0MB/s), io=448MiB (470MB), run=46919-46919msec
```

(2)16M大文件连续写入：

```shell
fio -directory=/mnt/nfstest/test2 -ioengine=libaio -iodepth=1 -direct=1 -fsync=1 -bs=16M -filesize=1G --rw=write -numjobs=16 -name=fiotest -group_reporting -output=16_w.data
```

结果

```plaintext
fiotest2: (groupid=0, jobs=16): err= 0: pid=166q561: Sat Nov 25 16:49:02 2023
write: I0PS=38, BW=608MiB/s (638MB/s)(16.0GiB/26934msec); 0 zone resets
slat (usec): min=1144, max=3439, avg=1612.21, stdev=208.03
clat (msec): min=142, max=643, avg=414.0q, stdev=41.20
lat (msec): min=144, max=644, avg=415.70, stdev=41.15
clat percentiles (msec):
 1.00th=[284]， 5.00th=[355], 10.00th=[368], 20.00th=[384], 
 30.00th=[397], 40.00th=[409], 50.00th=[418], 60.00th=[426],
 70.00th=[439], 80.00th=[447], 90.00th=[460], 95.00th=[468],
 99.00th=[489], 99.50th=[502], 99.90th=[542], 99.95th=[642],
 99.99th=[642]
bw ( KiB/s): min=523436, max=1048576, per=100.00%, avg=624827.94, stdev=12902.57, samples=845
iops:
min=25，max=64, avg=38.00, stdev=0.80, samples=845
lat (msec)
: 250=0.68%，500=98.83%, 750=0.49%
fsync/fdatasync/sync_file_range:
sync (nsec): min=16, max=604, avg=159.23, stdev=85.32
sync percentiles (nsec):
 1.00th=[35]， 5.00th=[56], 10.00th=[70], 20.00th=[129] ,
 30.00th=[139], 40.00th=[147], 50.00th=[151], 60.00th=[157] ,
 70.00th=[161], 80.00th=[169], 90.00th=[191], 95.00th=[342],
 99.00th=[540], 99.50th=[580], 99.90th=[604], 99.95th=[604],
 99.99th=[604]
cpu
usr=0.17%, sys=0.21%, ctx=1082, majf=0, minf=16
I0 depths 
: 1=198.4%， 2=0.0%， 4=0.0%， 8=0.0%， 16=0.0%， 32=0.0%， >=64=0.0%
submit
： 0=0.0%， 4=100.0%， 8=0.0%， 16=0.0%， 32=0.0%， 64=0.0%， >=64=0.0%
complete
: 0=0.0%， 4=100.0%， 8=0.0%， 16=0.0%， 32=0.0%， 64=0.0%， >=64=0.0%
issued rwts: total=0,1024,0,1008 short=0,0,0,0 dropped=0,0,0,0
Latency
 : target=0, window=0, percentile=100.00%, depth=1
Run status group 0 (all jobs):
WRITE: bw=608MiB/s (638MB/s), 608MiB/s-608MiB/s (638MB/s-638MB/s)， io=16.0GiB (17.2GB), run=26934-26934msec
```

3. 执行3次mtest元数据测试
   ```shell
   $ mpirun --allow-run-as-root localhost:64 -np 64 mdtest -I 20 -z 2 -b 10 -w 131072 -e 131072 -d /mnt/nfstest/perf1 -t -u

   $ mpirun --allow-run-as-root localhost:56 -np 56 mdtest -I 50 -z 2 -b 10 -w 524288 -e 524288 -d /mnt/nfstest/perf2 -t -u

   $ mpirun --allow-run-as-root localhost:32 -np 32 mdtest -I 20 -z 2 -b 10 -w 1048576 -e 1048576 -d /mnt/nfstest/perf3 -t -u
   ```

结果略

## 4 测试中产生的部分数据

iostat - 每块磁盘IO情况

mpstat - 系统CPU资源占用情况

juicefs stat - JuiceFS实时性能指标

juicefs .perfmetric - JuiceFS单位时间内IO情况

juicefs .accesslog - JuiceFS访问日志

具体数据略

## 5 测试结果分析

观察到2个问题

1. 从perfmetric看出，每次JuiceFS写分布式持久层大小(write_cache)不均，理论应该是统一大小
2. 从accesslog看出，刷新时间不均衡，连续写文件理论上间隔应该统一

针对问题1，推测可能是NFS问题，尝试在服务器本地写JuiceFS，观察perfmetric日志，发现写入大小一致，证明推测成立，是NFS客户端单位时间内发出的IO大小不一致导致

针对问题2，推测有其他机制触发刷新，将强制刷新时间更改为100s，再次写入数据查看accesslog，发现还是有不均匀的刷新，支持推测成立
