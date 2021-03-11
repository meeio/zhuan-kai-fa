# 常用指令

# telnet 

`telnet` 是用来创建TCP\IP链接的指令：

```
telnet localhost 18882
```

# valgrind

`vargrind` 可用于检测内存泄漏

```
valgrind --leak_cheack=yes ./executable
```

# memory

## top

```
top -p pid
```

## /proc

```
cat /proc/pid/status
```

# 查看 PID & TID

## 查看 PID

```
ps -u | grep name
```

## 查看 TID

```
ps -T -p pid
```
# 查看网络端口

```
lsof -i tcp -a -p pid
```

# 在不同的主机间传文件 SCP

```
scp /shop.tar.gz root@192.168.230.128:/home/root
```