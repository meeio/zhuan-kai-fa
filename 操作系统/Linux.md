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

# mmap & read

- **read()** 要读取磁盘上某个文件，那么这文件内容首先拷贝到内存中作为 `page cache`，然后再从 `page cache` 拷贝到用户指定的 `buffer` 中。也就是说，在数据已经加载到 `page cache` 后，还需要一次内存拷贝操作和一次系统调用。

- **mmap()** 会直接建立 `虚拟内存` 到文件内容的映射（在 `page cache` 上），当真正独写文件时，会产生 `page fault` ，将这部分文件内容从磁盘拷贝到内存中。

# malloc 的底层机制

- 使用 `brk` 与 `mmap` 申请内存，当申请的内存小于128kb时。直接使用 `brk` 调整栈顶指针；当大于128kb时，使用 `mmap` 申请一块虚拟内存。

- 不断调用 `brk` 与 `mmap` 会不断调用系统函数，所以malloc会使用「内存池」进行管理。在程序开始时，申请一组 `bin`, 每个 bin 里存放大小相近的 `chunk`。当申请内存时，会给用户分配一个大小合适的chunk。

