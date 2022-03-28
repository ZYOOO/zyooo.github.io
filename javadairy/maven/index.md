# Maven


Apache下的一个项目管理工具

官网地址 maven.apache.org

## 配置环境目录

1.获得maven文件夹的目录F:\software\apache-maven-3.8.3

2.在环境变量中新建一个新的 **系统变量** , MAVEN_HOME ,将文件目录给放上去

3.在Path中新建一个 %MAVEN_HOME%\bin

完成,打开cmd mvn -v检查是否安装成功,注意如果一开始是有错误的,更正之后记得重新打开cmd



## Maven仓库

Maven项目的仓库刚开始是没jar包的,当Maven项目需要jar包时先去查找本地仓库,如果没有再从中央仓库(由Maven团队设立的一个唯一的仓库)下载到本地仓库.



![image-20211010092302559](https://i.loli.net/2021/10/10/X6zYtoMHgEpZsfP.png)

远程仓库好处:可先去获取中央仓库中的jar包,提高本地仓库获取的速度,也可以放置一些公司自己写的jar包.

 cmd中在Maven项目的目录下mvn compile创建默认位置本地仓库

### 修改本地仓库位置

打开maven目录下的conf目录下的setting.xml

```
<localRepository>/path/to/local/repo</localRepository>
修改为想要的路径,例如
<localRepository>F:\software\apache-maven-3.8.3\mvn_repository</localRepository>
再加入配置文件中
```

访问中央仓库太慢的话,课访问国内的私服,例如阿里云的私服

```
	 配置到setting.xml里面mirror部分
	 <mirror>  
		  <id>alimaven</id>  
		  <name>aliyun maven</name>  
		  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		  <mirrorOf>central</mirrorOf>          
	 </mirror>
```



## 目录结构

![](https://i.loli.net/2021/10/10/xeV6EHDjUpTdqG9.png

![image-20211010095304973](https://i.loli.net/2021/10/10/DTPfzwHWKApbmFX.png)

## 常用命令

**mvn compile**

​	编译出字节码文件,在target目录下

**mvn clean**

​	删除target目录

**mvn package**

​	对于 java 工程执行 package 打成 jar 包，对于 web 工程打成 war包,同时先编译了。

**mvn install**

​	本地的当前项目安装到本地仓库中

## 生命周期

maven 对项目构建过程分为三套相互独立的生命周期，请注意这里说的是“三套”，而且“相互独立”，这三套生命周期分别是：

Clean Lifecycle 在进行真正的构建之前进行一些清理工作。

Default Lifecycle 构建的核心部分，编译，测试，打包，部署等等。

Site Lifecycle 生成项目报告，站点，发布站点。

**同一套生命周期中，执行后边的操作，会自动执行前边所有操作**



## ideal中配置maven

![image-20211010101531560](https://i.loli.net/2021/10/10/3cN2LbxqRhKlmvy.png)

![image-20211010105445627](https://i.loli.net/2021/10/10/3oibG6cKJftWmgx.png)

![image-20211010105528210](https://i.loli.net/2021/10/10/ynRXQT2zwYG587p.png)

![image-20211010105540384](https://i.loli.net/2021/10/10/DtPkEyN2xIjB64S.png)

![image-20211010105549553](https://i.loli.net/2021/10/10/EVIz19jqfhYuX7J.png)

![image-20211010105557745](https://i.loli.net/2021/10/10/fcqEviHak9jyShn.png)

![image-20211010105605450](https://i.loli.net/2021/10/10/3LNuVXWSQHODbe2.png)

![image-20211010105621801](https://i.loli.net/2021/10/10/a9B7gF4r5EPdscH.png)

![image-20211010105629475](https://i.loli.net/2021/10/10/jRpKCTLo6Zc5W39.png)

![image-20211010105637865](https://i.loli.net/2021/10/10/lqbOfEJs94vp8NQ.png)

![image-20211010105653138](https://i.loli.net/2021/10/10/x7CrNoAakQcK6zS.png)

## 坐标

```
<!--项目名称，定义为组织名+项目名，类似包名-->

<groupId>cn.itcast.maven</groupId>

<!-- 模块名称 --><artifactId>maven-first</artifactId>

<!-- 当前项目版本号，snapshot 为快照版本即非正式版本，release 为正式发布版本 -->

<version>0.0.1-SNAPSHOT</version>

<packaging > ：打包类型

jar：执行 package 会打成 jar 包

war：执行 package 会打成 war 包

pom ：用于 maven 工程的继承，通常父工程设置为 pom
```

**坐标来源** 添加依赖需要指定依赖 jar 包的坐标，但是很多情况我们是不知道 jar 包的的坐标，可以通过如下方

式查询：**从互联网搜索**

http://search.maven.org/

http://mvnrepository.com/



## 依赖范围

A 依赖 B，需要在 A 的 pom.xml 文件中添加 B 的坐标，添加坐标时需要指定依赖范围，依赖范围包

括：

​	compile：编译范围，指 A 在编译时依赖 B，此范围为默认依赖范围。编译范围的依赖会用在

编译、测试、运行，由于运行时需要所以编译范围的依赖会被打包。 

​	provided：provided 依赖只有在当 JDK 或者一个容器已提供该依赖之后才使用， provided 依 

赖在编译和测试时需要，在运行时不需要，比如：servlet api 被 tomcat 容器提供。

​	runtime：runtime 依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如：jdbc

的驱动包。由于运行时需要所以 runtime 范围的依赖会被打包。 

​	test：test 范围依赖 在编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用，

比如：junit。由于运行时不需要所以 test范围依赖不会被打包。

​	system：system 范围依赖与 provided 类似，但是你必须显式的提供一个对于本地系统中 JAR

文件的路径，需要指定 systemPath 磁盘路径，system依赖不推荐使用。

![image-20211010115659590](https://i.loli.net/2021/10/10/TUZyzWSjQV8R953.png)

测试总结：

 默认引入 的 jar 包 ------- compile 【默认范围 可以不写】（编译、测试、运行 都有效 ） 

 servlet-api 、jsp-api ------- provided （编译、测试 有效,运行时无效 防止和 tomcat 下 jar 冲突,tomcat中已经有了）

 jdbc 驱动 jar 包 ---- runtime （测试、运行 有效 ） 

 junit ----- test （测试有效）

依赖范围由强到弱的顺序是：compile>provided>runtime>test

![image-20211010115749115](https://i.loli.net/2021/10/10/7MNfeQ4OIrtzhYs.png)

## 设置jdk编译版本

```
 <build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<configuration>
			<source>1.8</source>
			<target>1.8</target>
			<encoding>UTF-8</encoding>
			</configuration>
		</plugin>
    </plugins>
</build>
```


