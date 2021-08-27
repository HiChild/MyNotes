# Mac jdk1.8安装及配置

## 一.访问官网下载jdk

访问官网https://www.oracle.com/index.html找到对应的下载安装包

点击上方导航栏的Products,然后点击java

![image-20210714134744124](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714134744124.png)

点击右侧的DownLoad java

![image-20210714134931425](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714134931425.png)

在接下来的页面中找到jdk8，并点击如图所示按钮

![image-20210714135051153](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714135051153.png)

找到对应的操作系统，下载并安装

![image-20210714135209840](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714135209840.png)

## 二.JDK的安装

安装时需要提供Mac电脑开机的密码

不断点击下一步即可完成安装

> 此处有疑惑，不知道为什么**还没配置环境变量 java就可以正常使用了**

安装完成后我们启动终端进行测试发现 java已经可以使用了

在终端测试如下

![image-20210714140004404](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714140004404.png)

​																								测试1

![image-20210714140112789](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714140112789.png)

​																								测试2

在IDEA测试如下：

新建普通项目

![image-20210714141425181](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714141425181.png)

测试结果显示正常

![image-20210714141554957](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714141554957.png)



原本我以为IDEA新建项目时还是无法自动寻找到jdk，所以我们还需要进行环境变量的配置

但是我测试发现第二次创建项目时，IDEA已经找到了默认的JDK

无论是普通项目，还是springBoot项目

![image-20210714141636474](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714141636474.png)

springboot项目

![image-20210714141803008](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714141803008.png)

不过此处的测试还不够完善，也许还不能完全说明问题。

> 对于此种情况出现的原因我的思考如下

1.可能是mac电脑的系统配置环境变量没有删干净，是上一任机主留下的环境变量的配置？

2.可能是mac系统高版本对开发者的优化？

## 三.环境变量的配置

在控制台输入以下代码

```java
/usr/libexec/java_home -V
```

得到java home的真实路径

![image-20210714173222506](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714173222506.png)

得到以下信息

```
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home
```

在home路径下输入 vim .bash_profile

将以上信息输入

按:wq 回车保存退出即可

## 四.验证jdk安装配置成功

这时 mac电脑的java环境已经成功安装了

![image-20210714174138501](/Users/easongguo/Library/Application Support/typora-user-images/image-20210714174138501.png)

