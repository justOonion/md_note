```shell
# 查看 超过100M大文件
find / -xdev -size +100M -exec ls -l {} \;

# 查看磁盘占用
df -h
# 查看当前的磁盘分区信息(主要是分区表信息)
disk -l

# 查看内存使用情况
free -h
```

