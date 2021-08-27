# Mac git的安装与配置

## 安装

访问git官网https://git-scm.com/

点击官网的下载按钮

![image-20210715100515194](/Users/easongguo/Library/Application Support/typora-user-images/image-20210715100515194.png)

选择MacOS

![image-20210715100712951](/Users/easongguo/Library/Application Support/typora-user-images/image-20210715100712951.png)

这里有很多种的git下载安装方式，我们此处选择二进制文件安装，点击如图所示installer按钮

![image-20210715100826032](/Users/easongguo/Library/Application Support/typora-user-images/image-20210715100826032.png)

下载后根据提示完成安装即可

![image-20210715100957163](/Users/easongguo/Library/Application Support/typora-user-images/image-20210715100957163.png)

最后访问终端，使用一下命令测试

```
git --version
```

出现如下图所示即为安装成功

![image-20210715101204268](/Users/easongguo/Library/Application Support/typora-user-images/image-20210715101204268.png)

## 配置(基于ssh)

1.在本地添加‘’username‘’和''email''

```
git config --global user.name "username"

git config --global user.email "email"
```

2.生成ssh密钥

1. 检查

> 首先检查电脑是否生成过密钥

```
cd ~/.ssh
```

若打开该文件夹为空，则表示没有生成过秘钥，进入第二步。（~表示根目录）

2. 生成

```
ssh-keygen -t rsa -C "Title"（随便起一个title）
```

命令要求输入密码，这个时候不用输入，直接三个回车即可

3. 查看

> 执行成功后，会在主目录.ssh路径下生成两个文件：id_rsa私钥文件；id_rsa.pub公钥文件

分别执行一下命令

```
cd ~/.ssh  
ls
cat id_rsa.pub
```

文件会被打开，并复制其中的内容

4. 添加

在git上

- 点击setting
- 点击SSH Keys
- 点击add SSH key

在远程仓库gitlab上添加title和key，和本地的一致；

title可以自己取一个容易区分的名字(最好和上面的保持一致)，key为id_rsa.pub中的内容（全部复制，可用“cat id_rsa.pub”命令打开）



## 完成

这样我们Mac电脑的安装和配置就完成了！



本文部分参考自
[MAC配置Git环境](https://www.jianshu.com/p/910fdc2a0362)



