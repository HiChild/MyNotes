# MAC Maven环境变量配置

下载好maven后

**Step 1**编辑 **/etc/profile** 文件 **sudo vim /etc/profile**，在文件末尾添加如下代码：

```
export MAVEN_HOME=/usr/local/apache-maven-3.3.9
export PATH=${PATH}:${MAVEN_HOME}/bin
```

上面的MAVEN_HOME改为自己的MAVEN 地址

> 如果出现read only,进行以下操作

加权限

```
sudo chmod a+rwx /etc/profile
```

**Step 2**

保存文件

```
:wq
```

并运行如下命令使环境变量生效：

```
source /etc/profile
```

在控制台输入如下命令，如果能看到 Maven 相关版本信息，则说明 Maven 已经安装成功：

```
$ mvn -v
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)
Maven home: /usr/local/apache-maven-3.3.9
Java version: 1.8.0_31, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_31.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: ISO8859-1
OS name: "mac os x", version: "10.13.4", arch: "x86_64", family: "mac"
```

### 问题1 如果保存文件出现 return 127

command not found wq

检查强制保存是否使用了错误的“!wq”

### 不是 `:!wq`，不是 `:!wq`，不是 `:!wq`

```
//正确写法
:wq!
```

### 问题2 如果IDEA终端不可使用mvn的话

出现

```
zsh: command not found: mvn
```

并运行如下命令使环境变量生效：

```
source /etc/profile
```





