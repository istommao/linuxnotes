# 简介

Linux 学习笔记

## nginx 日志分析

`平均每秒req`

```shell
awk '{sec=substr($4,2,20);reqs++;reqsBySec[sec]++;} END{print reqs/length(reqsBySec)}'
```

`统计最高每秒req`

```shell
awk '{sec=substr($4,2,20);requests[sec]++;} END{for(s in requests){printf("%s%s\n", requests[s],s)}}' | sort -nr | head -n 3
```
