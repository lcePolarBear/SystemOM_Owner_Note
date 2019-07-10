## 查看 CPU 与内存的占用
```
ps -eo user,pid,pcpu,pmem,args --sort=-pcpu  |head -n 10
ps -eo user,pid,pcpu,pmem,args --sort=-pmem  |head -n 10
```
-o 自定义输出格式
--sort 按某一属性排序