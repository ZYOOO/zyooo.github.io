# Cmd中mvn_compile出现的问题


在cmd上运行mvn compile时出现编译错误

No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?

输入mvn -v查看

![image-20211010094312986](https://i.loli.net/2021/10/10/r2DvgKkZqV9zWIM.png)

查看环境变量

![image-20211010094353318](https://i.loli.net/2021/10/10/SL8TVwqAkflpGtj.png)

将JAVA_HOME路径修改为jdk的

![image-20211010094549132](https://i.loli.net/2021/10/10/8SAHEKGB5yjkXJi.png)

重启cmd输入java -version检查是否配置成功

![image-20211010094625868](https://i.loli.net/2021/10/10/md67JpaTRlcL1ND.png)

mvn -v

![image-20211010094654413](https://i.loli.net/2021/10/10/BfOosz354SDZmFE.png)

再次编译,成功

![image-20211010094811522](https://i.loli.net/2021/10/10/geHFQpdL17zhPtv.png)

