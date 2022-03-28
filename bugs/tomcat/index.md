# Tomcat




## 修改Tomcat的默认端口和启动项目

### 修改默认端口

1.首先查看80端口是否被占用

```java
netstat -lntp | grep 80
```

2.进入tomcat安装目录下的conf文件夹修改server.xml文件

 将端口修改为80

![image-20211110215912362](https://i.loli.net/2021/11/10/jzU1WfCoY3kuy4S.png)

但是这个时候输入netstat -lntp | grep 80 查看发现只监听了 127.0.0.1:80 没有打开:::80,也就是0.0.0.0:80

导致外部无法访问80端口,最后重启了云服务器之后在查看发现可以了.

### 修改默认启动项目

在host下面添加 context的内容,docBase就是项目路径名称,默认的绝对路径是上面配置的name和appBase

![image-20211110220143155](https://i.loli.net/2021/11/10/hpOXobgnI8VUPAY.png)

