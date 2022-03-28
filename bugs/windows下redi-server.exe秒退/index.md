# Windows下redis-Server秒退


## Windows下redi-server.exe秒退

**遇到个问题：** 在第一次正常启动之后，配置完conf里的save，再次开启server秒退。

错误信息：QForkMasterInit: system error caught. error code=0x000005af, message=VirtualAllocEx failed.: unknown error

**原因：**未设置redis最大内存

**解决：**在conf中最下面加入

```
maxmemory 268435456
maxheap 314572800
```

再次启动正常 在目录下打开cmd  输入 redis-server.exe redis.windows.conf

