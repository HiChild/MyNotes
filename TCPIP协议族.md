太厉害了，终于有人能把TCP/IP协议讲的明明白白了！

【TCP/IP】https://developer.51cto.com/art/201906/597961.htm

# 计算机网络

### 计算机网络分层

计算机被OSI分为七层，由上至下分别为

- 应用层（HTTP/HTTPS）
- 表示层
- 会话层
- 传输层（TCP/UDP）
- 网络层（IP）
- 数据链路层 （CSMA/CD）
- 物理层

起还有工业上的四层模型

- 应用层
- 传输层
- 网络层
- 链路层

其中每一层为了提供相关的功能都有相关的协议;

计算机的层次结构被设计成两头宽中间窄的形式，而中间的窄腰的部分就是网络层，由ip协议承担承上启下的作用;

### 传输层协议

#### TCP协议和UDP

- tcp:面向连接，保证数据传输的可靠性（序号，确认号，超时重传，流量控制，拥塞控制，快重传，快回复）
- udp：面向用户，不保证数据的可靠性，结构简单，传输速度快，一般用于即时通讯，广播等情况;

#### TCP协议

发送的数据单位为段：MSS（消息长度）

可能遇见的问题：粘包，拆包，丢包问题;



#### UDP协议

可能遇到的问题：丢包;



### 网络层协议

#### ip协议

作用：

主要用于计算机终端到终端之间的节点通信，又称为“点对点通信协议”；

其大致又分为三个功能：ip寻址，路由，ip分包和组包;

##### ip的分类

ip地址又分为ipv4，和ipv6，这里着重介绍ipv4

**ipv4**：

由32位二进制数字组成，为了人类的表示方便，我们一般每八位进行合并，表示成十进制；

如xxxxxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx

合并为0-255.0-255.0-255.0-255；

ip地址分为4类

**A 类 IP 地址是首位以 “0” 开头的地址。**

**B 类 IP 地址是前两位 “10” 的地址。**

**C 类 IP 地址是前三位为 “110” 的地址。**

**D 类 IP 地址是前四位为 “1110” 的地址。**

广播地址：将 IP 地址中的主机地址部分全部设置为 1，就成了广播地址。

**子网掩码**：

现在一个 IP 地址的网络标识和主机标识已不再受限于该地址的类别，而是由一个叫做“子网掩码”的识别码通过子网网络地址细分出比 A 类、B 类、C 类更小粒度的网络。

表示形式如：xxxxxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx /24 

前24位则为网络地址，后面剩下的8位为主机地址；

不一定必须为24。17，19，20也是可以的，这里只是举一个例子;

相关协议：

- DNS
- ARP
- ICMP
- IP隧道
- NAT

# DNS

**什么是DNS**

就是将人类便于记忆的网址翻译成ip地址的一个域名解析系统;是一个分布式的数据库；

其包含的主要层级关系有四层：

- 根域名服务器
- 顶级域名服务器
- 权威域名服务器（二级域名服务器）
- 三级域名服务器

DNS的查询过程如下所示：

1、在浏览器中输入www . qq .com 域名，操作系统会先检查自己本地的hosts文件是否有这个网址映射关系，如果有，就先调用这个IP地址映射，完成域名解析。
2、如果hosts里没有这个域名的映射，则查找本地DNS解析器缓存，是否有这个网址映射关系，如果有，直接返回，完成域名解析。
3、如果hosts与本地DNS解析器缓存都没有相应的网址映射关系，首先会找TCP/ip参数中设置的首选DNS服务器，在此我们叫它本地DNS服务器，此服务器收到查询时，如果要查询的域名，包含在本地配置区域资源中，则返回解析结果给客户机，完成域名解析，此解析具有权威性。
4、如果要查询的域名，不由本地DNS服务器区域解析，但该服务器已缓存了此网址映射关系，则调用这个IP地址映射，完成域名解析，此解析不具有权威性。
5、如果本地DNS服务器本地区域文件与缓存解析都失效，则根据本地DNS服务器的设置（是否设置转发器）进行查询，如果未用转发模式，本地DNS就把请求发至13台根DNS，根DNS服务器收到请求后会判断这个域名(.com)是谁来授权管理，并会返回一个负责该顶级域名服务器的一个IP。本地DNS服务器收到IP信息后，将会联系负责.com域的这台服务器。这台负责.com域的服务器收到请求后，如果自己无法解析，它就会找一个管理.com域的下一级DNS服务器地址(http://qq.com)给本地DNS服务器。当本地DNS服务器收到这个地址后，就会找http://qq.com域服务器，重复上面的动作，进行查询，直至找到www . qq .com主机。
6、如果用的是转发模式，此DNS服务器就会把请求转发至上一级DNS服务器，由上一级服务器进行解析，上一级服务器如果不能解析，或找根DNS或把转请求转至上上级，以此循环。不管是本地DNS服务器用是是转发，还是根提示，最后都是把结果返回给本地DNS服务器，由此DNS服务器再返回给客户机。
从客户端到本地DNS服务器是属于递归查询，而DNS服务器之间就是的交互查询就是迭代查询。

#### 关于CNAME和A记录

1.**什么是A记录**

A (Address) 记录是用来指定主机名(或域名)对应的IP地址记录。用户可以将该域名下的网站服务器指向到自己的web server上。同时也可以设置您域名的二级域名。（我理解：就像一个1对1的字典，存贮域名的对应的ip地址，关系为1对1）

2.**什么是CNAME**

即：别名记录。这种记录允许您将多个名字映射到另外一个域名。通常用于同时提供WWW和MAIL服务的计算机。例如，有一台计算机名为“host.juming.com”(A记录)。它同时提供WWW和MAIL服务，为了便于用户访问服务。可以为该计算机设置两个别名(CNAME)：WWW和MAIL。这两个别名的全称就 http://www.juming.com/和“mail.juming.com”。实际上他们都指向 “host.juming.com”。（我理解：添加www.前缀和mail前缀的别名功能，多个网址名对应同一个A记录,关系为n对1）;

3.**使用A记录和CNAME进行域名解析的区别**

A记录就是把一个域名解析到一个IP地址(Address，特制数字IP地址)，而CNAME记录就是把域名解析到另外一个域名。其功能是差不多，CNAME将几个主机名指向一个别名，其实跟指向IP地址是一样的，因为这个别名也要做一个A记录的。但是使用CNAME记录可以很方便地变更IP地址。如果一台服务器有100个网站，他们都做了别名，该台服务器变更IP时，只需要变更别名的A记录就可以了。

A记录也有一些好处，例如可以在输入域名时不用输入WWW.来访问网站哦!从SEO优化角度来看，一些搜索引擎如alex或一些搜索查询工具网站等等则默认是自动去掉WWW.来辨别网站，CNAME记录是必须有如：WWW(别名)前缀的域名，有时候会遇到这样的麻烦，前缀去掉了默认网站无法访问。

　　有人认为，在SEO优化网站的时候，由于搜索引擎找不到去掉WWW.的域名时，对网站权重也会有些影响。因为有些网民客户也是不喜欢多写三个W来访问网站的，网站无法访问有少量网民客户会放弃继续尝试加WWW.访问域名了，因此网站访问浏览量也会减少一些。

　　也有人认为同一个域名加WWW.和不加WWW.访问网站也会使网站权重分散，这也是个问题。但是可以使用301跳转把不加WWW.跳转到加WWW.的域名，问题就解决了。

**个人理解**：CNAME就像是a记录和ip地址之间的一个缓存，CNAME指向A记录，并可以多个域名对应一个a记录，保持了网站的稳定性，易于修改网站的ip地址;

参考文章：https://www.juming.com/zx/4656.html

**浅谈原理**：http://dockone.io/article/8348

部分内容如下：

【编者的话】域名系统（Domain Name System）是整个互联网的电话簿，它能够将可被人理解的域名翻译成可被机器理解 IP 地址，使得互联网的使用者不再需要直接接触很难阅读和理解的 IP 地址。

我们在这篇文章中的第一部分会介绍 DNS 的工作原理以及一些常见的 DNS 问题，而第二部分我们会介绍 DNS 服务 CoreDNS 的架构和实现原理。

### DNS

域名系统在现在的互联网中非常重要，因为服务器的 IP 地址可能会经常变动，如果没有了 DNS，那么可能 IP 地址一旦发生了更改，当前服务器的客户端就没有办法连接到目标的服务器了，如果我们为 IP 地址提供一个『别名』并在其发生变动时修改别名和 IP 地址的关系，那么我们就可以保证集群对外提供的服务能够相对稳定地被其他客户端访问。
DNS 其实就是一个分布式的树状命名系统，它就像一个去中心化的分布式数据库，存储着从域名到 IP 地址的映射。

#### 工作原理

在我们对 DNS 有了简单的了解之后，接下来我们就可以进入 DNS 工作原理的部分了，作为用户访问互联网的第一站，当一台主机想要通过域名访问某个服务的内容时，需要先通过当前域名获取对应的 IP 地址。这时就需要通过一个 DNS 解析器负责域名的解析，下面的图片展示了 DNS 查询的执行过程：

1. 本地的 DNS 客户端向 DNS 解析器发出解析 draveness.me 域名的请求；
2. DNS 解析器首先会向就近的根 DNS 服务器 . 请求顶级域名 DNS 服务的地址；
3. 拿到顶级域名 DNS 服务 me. 的地址之后会向顶级域名服务请求负责 dravenss.me. 域名解析的命名服务；
4. 得到授权的 DNS 命名服务时，就可以根据请求的具体的主机记录直接向该服务请求域名对应的 IP 地址；


DNS 客户端接受到 IP 地址之后，整个 DNS 解析的过程就结束了，客户端接下来就会通过当前的 IP 地址直接向服务器发送请求。

对于 DNS 解析器，这里使用的 DNS 查询方式是迭代查询，每个 DNS 服务并不会直接返回 DNS 信息，而是会返回另一台 DNS 服务器的位置，由客户端依次询问不同级别的 DNS 服务直到查询得到了预期的结果；另一种查询方式叫做递归查询，也就是 DNS 服务器收到客户端的请求之后会直接返回准确的结果，如果当前服务器没有存储 DNS 信息，就会访问其他的服务器并将结果返回给客户端。

### 域名层级

域名层级是一个层级的树形结构，树的最顶层是根域名，一般使用`.`来表示，这篇文章所在的域名一般写作 draveness.me，但是这里的写法其实省略了最后的`.`，也就是全称域名（FQDN）dravenss.me.。

根域名下面的就是 com、net 和 me 等顶级域名以及次级域名 draveness.me，我们一般在各个域名网站中购买和使用的都是次级域名、子域名和主机名了。

#### 域名服务器

既然域名的命名空间是树形的，那么用于处理域名解析的 DNS 服务器也是树形的，只是在树的组织和每一层的职责上有一些不同。DNS 解析器从根域名服务器查找到顶级域名服务器的 IP 地址，又从顶级域名服务器查找到权威域名服务器的 IP 地址，最终从权威域名服务器查出了对应服务的 IP 地址。

```
$ dig -t A draveness.me +trace
```

我们可以使用 dig 命令追踪 draveness.me 域名对应 IP 地址是如何被解析出来的，首先会向预置的 13 组根域名服务器发出请求获取顶级域名的地址：

```
.            56335   IN  NS  m.root-servers.net.
.           56335   IN  NS  b.root-servers.net.
.           56335   IN  NS  c.root-servers.net.
.           56335   IN  NS  d.root-servers.net.
.           56335   IN  NS  e.root-servers.net.
.           56335   IN  NS  f.root-servers.net.
.           56335   IN  NS  g.root-servers.net.
.           56335   IN  NS  h.root-servers.net.
.           56335   IN  NS  i.root-servers.net.
.           56335   IN  NS  a.root-servers.net.
.           56335   IN  NS  j.root-servers.net.
.           56335   IN  NS  k.root-servers.net.
.           56335   IN  NS  l.root-servers.net.
.           56335   IN  RRSIG   NS 8 0 518400 20181111050000 20181029040000 2134 . G4NbgLqsAyin2zZFetV6YhBVVI29Xi3kwikHSSmrgkX+lq3sRgp3UuQ3 JQxpJ+bZY7mwzo3NxZWy4pqdJDJ55s92l+SKRt/ruBv2BCnk9CcnIzK+ OuGheC9/Coz/r/33rpV63CzssMTIAAMQBGHUyFvRSkiKJWFVOps7u3TM jcQR0Xp+rJSPxA7f4+tDPYohruYm0nVXGdWhO1CSadXPvmWs1xeeIKvb 9sXJ5hReLw6Vs6ZVomq4tbPrN1zycAbZ2tn/RxGSCHMNIeIROQ99kO5N QL9XgjIJGmNVDDYi4OF1+ki48UyYkFocEZnaUAor0pD3Dtpis37MASBQ fr6zqQ==
;; Received 525 bytes from 8.8.8.8#53(8.8.8.8) in 247 ms
```

> 根域名服务器是 DNS 中最高级别的域名服务器，这些服务器负责返回顶级域的权威域名服务器地址，这些域名服务器的数量总共有 13 组，域名的格式从上面返回的结果可以看到是 .root-servers.net，每个根域名服务器中只存储了顶级域服务器的 IP 地址，大小其实也只有 2MB 左右，虽然域名服务器总共只有 13 组，但是每一组服务器都通过提供了镜像服务，全球大概也有几百台的根域名服务器在运行。

在这里，我们获取到了以下的 5 条 NS 记录，也就是 5 台 me. 定义域名 DNS 服务器：

```
me.          172800  IN  NS  b0.nic.me.
me.         172800  IN  NS  a2.nic.me.
me.         172800  IN  NS  b2.nic.me.
me.         172800  IN  NS  a0.nic.me.
me.         172800  IN  NS  c0.nic.me.
me.         86400   IN  DS  2569 7 1 09BA1EB4D20402620881FD9848994417800DB26A
me.         86400   IN  DS  2569 7 2 94E798106F033500E67567B197AE9132C0E916764DC743C55A9ECA3C 7BF559E2
me.         86400   IN  RRSIG   DS 8 1 86400 20181113050000 20181031040000 2134 . O81bud61Qh+kJJ26XHzUOtKWRPN0GHoVDacDZ+pIvvD6ef0+HQpyT5nV rhEZXaFwf0YFo08PUzX8g5Pad8bpFj0O//Q5H2awGbjeoJnlMqbwp6Kl 7O9zzp1YCKmB+ARQgEb7koSCogC9pU7E8Kw/o0NnTKzVFmLq0LLQJGGE Y43ay3Ew6hzpG69lP8dmBHot3TbF8oFrlUzrm5nojE8W5QVTk1QQfrZM 90WBjfe5nm9b4BHLT48unpK3BaqUFPjqYQV19C3xJ32at4OwUyxZuQsa GWl0w9R5TiCTS5Ieupu+Q9fLZbW5ZMEgVSt8tNKtjYafBKsFox3cSJRn irGOmg==
;; Received 721 bytes from 192.36.148.17#53(i.root-servers.net) in 59 ms
```


当 DNS 解析器从根域名服务器中查询到了顶级域名 .me 服务器的地址之后，就可以访问这些顶级域名服务器其中的一台 b2.nic.me 获取权威 DNS 的服务器的地址了：

```
draveness.me.        86400   IN  NS  f1g1ns1.dnspod.net.
draveness.me.       86400   IN  NS  f1g1ns2.dnspod.net.
fsip6fkr2u8cf2kkg7scot4glihao6s1.me. 8400 IN NSEC3 1 1 1 D399EAAB FSJJ1I3A2LHPTHN80MA6Q7J64B15AO5K  NS SOA RRSIG DNSKEY NSEC3PARAM
fsip6fkr2u8cf2kkg7scot4glihao6s1.me. 8400 IN RRSIG NSEC3 7 2 8400 20181121151954 20181031141954 2208 me. eac6+fEuQ6gK70KExV0EdUKnWeqPrzjqGiplqMDPNRpIRD1vkpX7Zd6C oN+c8b2yLoI3s3oLEoUd0bUi3dhyCrxF5n6Ap+sKtEv4zZ7o7CEz5Fw+ fpXHj7VeL+pI8KffXcgtYQGlPlCM/ylGUGYOcExrB/qPQ6f/62xrPWjb +r4=
qcolpi5mj0866sefv2jgp4jnbtfrehej.me. 8400 IN NSEC3 1 1 1 D399EAAB QD4QM6388QN4UMH78D429R72J1NR0U07  NS DS RRSIG
qcolpi5mj0866sefv2jgp4jnbtfrehej.me. 8400 IN RRSIG NSEC3 7 2 8400 20181115151844 20181025141844 2208 me. rPGaTz/LyNRVN3LQL3LO1udby0vy/MhuIvSjNfrNnLaKARsbQwpq2pA9 +jyt4ah8fvxRkGg9aciG1XSt/EVIgdLSKXqE82hB49ZgYDACX6onscgz naQGaCAbUTSGG385MuyxCGvqJdE9kEZBbCG8iZhcxSuvBksG4msWuo3k dTg=
;; Received 586 bytes from 199.249.127.1#53(b2.nic.me) in 267 ms
```


这里的权威 DNS 服务是作者在域名提供商进行配置的，当有客户端请求 draveness.me 域名对应的 IP 地址时，其实会从作者使用的 DNS 服务商 DNSPod 处请求服务的 IP 地址：

```
draveness.me.        600 IN  A   123.56.94.228
draveness.me.       86400   IN  NS  f1g1ns2.dnspod.net.
draveness.me.       86400   IN  NS  f1g1ns1.dnspod.net.
;; Received 123 bytes from 58.247.212.36#53(f1g1ns1.dnspod.net) in 28 ms
```


最终，DNS 解析器从 f1g1ns1.dnspod.net 服务中获取了当前博客的 IP 地址 123.56.94.228，浏览器或者其他设备就能够通过 IP 向服务器获取请求的内容了。

从整个解析过程，我们可以看出 DNS 域名服务器大体分成三类，根域名服务、顶级域名服务以及权威域名服务三种，获取域名对应的 IP 地址时，也会像遍历一棵树一样按照从顶层到底层的顺序依次请求不同的服务器。

#### 胶水记录

在通过服务器解析域名的过程中，我们看到当请求 me. 顶级域名服务器的时候，其实返回了 b0.nic.me 等域名：

```
me.          172800  IN  NS  b0.nic.me.
me.         172800  IN  NS  a2.nic.me.
me.         172800  IN  NS  b2.nic.me.
me.         172800  IN  NS  a0.nic.me.
me.         172800  IN  NS  c0.nic.me.
...
```


就像我们最开始说的，在互联网中想要请求服务，最终一定需要获取 IP 提供服务的服务器的 IP 地址；同理，作为 b0.nic.me 作为一个 DNS 服务器，我也必须获取它的 IP 地址才能获得次级域名的 DNS 信息，但是这里就陷入了一种循环：

1. 如果想要获取 dravenss.me 的 IP 地址，就需要访问 me 顶级域名服务器 b0.nic.me
2. 如果想要获取 b0.nic.me 的 IP 地址，就需要访问 me 顶级域名服务器 b0.nic.me
3. 如果想要获取 b0.nic.me 的 IP 地址，就需要访问 me 顶级域名服务器 b0.nic.me
4. ……


为了解决这一个问题，我们引入了胶水记录（Glue Record）这一概念，也就是在出现循环依赖时，直接在上一级作用域返回 DNS 服务器的 IP 地址：

```
$ dig +trace +additional draveness.me

...

me.         172800  IN  NS  a2.nic.me.
me.         172800  IN  NS  b2.nic.me.
me.         172800  IN  NS  b0.nic.me.
me.         172800  IN  NS  a0.nic.me.
me.         172800  IN  NS  c0.nic.me.
me.         86400   IN  DS  2569 7 1 09BA1EB4D20402620881FD9848994417800DB26A
me.         86400   IN  DS  2569 7 2 94E798106F033500E67567B197AE9132C0E916764DC743C55A9ECA3C 7BF559E2
me.         86400   IN  RRSIG   DS 8 1 86400 20181116050000 20181103040000 2134 . cT+rcDNiYD9X02M/NoSBombU2ZqW/7WnEi+b/TOPcO7cDbjb923LltFb ugMIaoU0Yj6k0Ydg++DrQOy6E5eeshughcH/6rYEbVlFcsIkCdbd9gOk QkOMH+luvDjCRdZ4L3MrdXZe5PJ5Y45C54V/0XUEdfVKel+NnAdJ1gLE F+aW8LKnVZpEN/Zu88alOBt9+FPAFfCRV9uQ7UmGwGEMU/WXITheRi5L h8VtV9w82E6Jh9DenhVFe2g82BYu9MvEbLZr3MKII9pxgyUE3pt50wGY Mhs40REB0v4pMsEU/KHePsgAfeS/mFSXkiPYPqz2fgke6OHFuwq7MgJk l7RruQ==
a0.nic.me.      172800  IN  A   199.253.59.1
a2.nic.me.      172800  IN  A   199.249.119.1
b0.nic.me.      172800  IN  A   199.253.60.1
b2.nic.me.      172800  IN  A   199.249.127.1
c0.nic.me.      172800  IN  A   199.253.61.1
a0.nic.me.      172800  IN  AAAA    2001:500:53::1
a2.nic.me.      172800  IN  AAAA    2001:500:47::1
b0.nic.me.      172800  IN  AAAA    2001:500:54::1
b2.nic.me.      172800  IN  AAAA    2001:500:4f::1
c0.nic.me.      172800  IN  AAAA    2001:500:55::1
;; Received 721 bytes from 192.112.36.4#53(g.root-servers.net) in 110 ms

...
```

也就是同时返回 NS 记录和 A（或 AAAA） 记录，这样就能够解决域名解析出现的循环依赖问题。

#### 服务发现

讲到现在，我们其实能够发现 DNS 就是一种最早的服务发现的手段，通过虽然服务器的 IP 地址可能会经常变动，但是通过相对不会变动的域名，我们总是可以找到提供对应服务的服务器。

在微服务架构中，服务注册的方式其实大体上也只有两种，一种是使用 ZooKeeper 和 etcd 等配置管理中心，另一种是使用 DNS 服务，比如说 Kubernetes 中的 CoreDNS 服务。

使用 DNS 在集群中做服务发现其实是一件比较容易的事情，这主要是因为绝大多数的计算机上都会安装 DNS 服务，所以这其实就是一种内置的、默认的服务发现方式，不过使用 DNS 做服务发现也会有一些问题，因为在默认情况下 DNS 记录的失效时间是 600s，这对于集群来讲其实并不是一个可以接受的时间，在实践中我们往往会启动单独的 DNS 服务满足服务发现的需求。



# CSMA/CD

