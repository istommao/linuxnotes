# 性能分析优化

- Linux Performance Tools https://static001.geekbang.org/resource/image/9e/7a/9ee6c1c5d88b0468af1a3280865a6b7a.png
http://www.brendangregg.com/Perf/linux_perf_tools_full.png


## Linux性能分析脑图
- Linux性能优化脑图 https://static001.geekbang.org/resource/image/0f/ba/0faf56cd9521e665f739b03dd04470ba.png

### CPU

*进程和CPU原理*

- 进程与线程
- CPU调度
- 中断系统
- CPU缓存
- NUMA

*性能指标*

- 平均负载
- CPU使用率
    - 用户CPU
    - 系统CPU
    - 软中断
    - 硬中断
    - 窃取CPU
    - 客户CPU
- 上下文切换
    - 自愿上下文切换
    - 非自愿上下文切换
- CPU缓存命中率

*性能剖析*
- top/ps
- vmstat
- mpstat
- sar
- pidstat
- strace
- perf
- execsnoop
- proc文件系统

*调优方法*

- CPU绑定
- 进程CPU资源限制
- 进程优先级调整
- 中断负载均衡
- CPU缓存
- NUMA优化

### 内存

*内存原理*
- 地址空间
- 虚拟内存
- 内存分配与回收
- 缓存与缓冲区
- SWAP

*性能指标*
- 系统内存使用量
- 进程内存使用量
- 缓存与缓冲区命中率
- SWAP使用量

*性能剖析*
- free
- top
- sar
- vmstat
- cachestat
- cachetop
- memleak
- proc文件系统

*调优方法*
- 利用缓存与缓冲区
- 减少SWAP使用
- 减少动态内存分配
- 优化NUMA
- 限制进程内存资源
- 使用HugePage

### 文件系统

*文件系统原理*
- 虚拟文件系统
- 文件系统I/O栈
- 文件系统缓存
- 文件系统种类

*性能指标*
- 容量
- IOPS
- 缓存命中率

*性能剖析 *
- df
- strace
- vmstat
- sar
- perf
- proc文件系统

*调优方法*
- 文件系统选型
- 利用文件系统缓存
- I/O隔离

### Linux内核

*内核原理*
- 内核态

*性能剖析*
- BPF
- perf
- proc文件系统

*性能调优*
- 内核选项

### 应用程序

*性能指标*
- 吞吐量
- 响应时间
- 资源使用率

*性能剖析 *
- USE方法
    - 使用率
    - 饱和度
    - 错误
- 进程剖析
    - 进程状态
    - 资源使用率
    - I/O剖析
    - 系统调用
    - 热点函数
    - 动态追踪
- APM

*调优方法*
- 逻辑优化
- 编程语言
- 算法优化
- 非阻塞 I/O
- 利用缓存与缓冲区
- 异步处理与并发
- 垃圾回收

### 架构设计

*空间换时间*
- 缓存
- 缓冲区
- 冗余数据

*时间换空间
- 压缩编码
- 页面交换

*并行处理*
- 多线程
- 多进程
- 分布式

*异步处理*
- 异步I/O
- 消息队列
- 事件通知

### 网络

*网络原理*
- 网络配置
- TCP/IP协议
- 网络收发流程
- 高级路由
- 网络QoS
- 网络防火墙
- C10K与C100K

*性能指标*
- 吞吐量
    - BPS
    - QPS
    - PPS
- 延迟
- 丢包
- TCP重传

*性能剖析*
- ethtool
- sar
- ping
- netstat/ss
- ifstat
- ifconfig
- tcpdump
- wireshark
- iptables
- traceroute
- ipcontrack
- perf

*调优方法*
- 网卡调优
    - MTU
    — 队列长度
    - 链路聚合
- 协议调优
    - HTTP
    - TCP
    - Overlay
- 资源控制
    - QoS
- 内核调优
    - NAT调优
    - 功能卸载
    - 负载均衡
    - DPDK

### 性能测试

*明确需求*
- 系统资源需求
- 应用程序需求

*环境假设*
- 合理的假设
- 生产环境模拟
- 生产负载模拟

*性能测试*
- 基准测试
- 负载测试
- 压力测试

*结果分析*
- 应用程序瓶颈
- 数据库瓶颈
- 系统资源瓶颈

### 磁盘IO

*磁盘原理*
- 磁盘管理
- 磁盘类型
- 磁盘接口
- 磁盘I/O栈

*性能指标*
- 使用率
- IOPS
- 吞吐量
- IOWAIT

*性能剖析*
- dstat
- sar
- iostat
- pidstat
- iotop
- iolatency
- blktrace
- fio
- perf

*调优方法*
- 系统调用
- I/O资源控制
- 充分利用缓存
- RAID
- I/O隔离


### 性能监控

*时间序列分析*
- 历史趋势分析
- 性能模型构建
- 未来趋势预测

*服务调用追踪*
- 服务调用流程跟踪
- 服务调用性能分析
- 服务调用链拓扑展示

*数据可视化*
- 趋势图
- 散点图
- 热图
- 饼图

*告警通知*
- 阈值选择
- 报警策略
- 通知渠道


## CPU负载

uptime

> 简单来说，平均负载是指单位时间内，
> 系统处于可运行状态和不可中断状态的平均进程数，
> 也就是平均活跃进程数，它和 CPU 使用率并没有直接关系

```bash
# 查看cpu数
grep 'model name' /proc/cpuinfo | wc -l
```

### 测试

- stress 是一个 Linux
系统压力测试工具，这里我们用作异常进程模拟平均负载升高的场景
- sysstat 包含了常用的 Linux 性能工具，用来监控和分析系统的性能。我们的案例会用到这个包的两个命令 mpstat 和 pidstat。
    - mpstat 是一个常用的多核 CPU 性能分析工具，用来实时查看每个 CPU 的性能指标，以及所有 CPU 的平均指标。
    - pidstat 是一个常用的进程性能分析工具，用来实时查看进程的 CPU、内存、I/O 以及上下文切换等性能指标

*上下文切换查看*

vmstat

- cs（context switch）是每秒上下文切换的次数。
- in（interrupt）则是每秒中断的次数。
- r（Running or Runnable）是就绪队列的长度，也就是正在运行和等待 CPU 的进程数。
- b（Blocked）则是处于不可中断睡眠状态的进程数。

pidstat -w 5

sysbench 模拟系统多线程调度切换的工具

```bash
# -d 参数表示高亮显示变化的区域
$ watch -d cat /proc/interrupts
           CPU0       CPU1
...
RES:    2450431    5279697   Rescheduling interrupts
...
```
