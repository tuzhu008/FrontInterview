DNS( Domain Name System)是“域名系统”的英文缩写，是一种组织成**域层次结构**的计算机和网络服务命名系统，它用于TCP/IP网络，它所提供的服务是用来**将主机名和域名转换为IP地址**的工作。

![image.png](/computer/network/dns/dns-1.png)

将_域名转换为对应的 IP 地址_的过程叫做**域名解析**。

![image.png](/computer/network/dns/dns-2.png)

## DNS 域名

![image.png](/computer/network/dns/dns-3.png)

## DNS 解析器
从应用程序的角度看，访问 DNS 是通过一个叫**解析器**（ Resolver ）的应用程序来完成的。发送一个 TCP 或 UDP 数据包之前，解析器必须将域名（主机名）转换为 IP 地址。一个解析器至少要注册一个域名服务器的 IP 地址。通常，它至少包括本地域名服务器的 IP 地址。
## DNS 域名服务器
DNS 域名空间的层次结构，允许不同的域名服务器管理域名空间的不同部分。**域名服务器**是指管理域名的主机及软件，它**可以管理所在分层的域**。其所管理的分层叫做**区域**（ zone ）。一个 zone 是 DNS 域名空间的一棵子树，它可以单独管理而不受其它 zone 影响。**_每层都设有一个域名服务器。_**

![image.png](/computer/network/dns/dns-4.png)

根所设置的 DNS 叫做**根域名服务器**，它对 DNS 的检索数据功能起着至关重要的作用。**根域名服务器中注册了顶级域名服务器的 IP 地址**。如果想要新增一个一级域名，或者修改已有的顶级域名，就要在根域名服务器中进行新增或变更。
类似的，**顶级域名服务器中注册了二级域名服务器的 IP 地址。**如果域名服务器下面没有其它分层，就可以自由地指定主机名称。**如果想重新设置域名服务器的 IP 地址或修改域名，必须在上一层的域名服务器中进行修改。**
域名和域名服务器都需要按照分层进行设置。如果域名服务器出现故障，那么针对这个域的 DNS 查询就无法正常工作。因此，为了提高可用性，至少设置两台域名服务器。一旦第一台域名服务器无法提供查询时，就会自动转到第二个甚至第三个域名服务器上进行。
## DNS 域名服务器类型
### 本地域名服务器
互联网接入服务运营商或者一个大的网络机构，像公司、大学等都有一台或多台可以自行管理的域名服务器，这类域名服务器称为[本地域名服务器](https://www.zhihu.com/search?q=%E6%9C%AC%E5%9C%B0%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2051992590%7D)，也称为_默认域名服务器_。本地域名服务器离客户端较近。当一个 DNS客户端发出 DNS 查询时，首先送到本地域名服务器。如果本地域名服务器数据库中有对应的域名信息，会将查询的域名转换为 IP 地址返回客户端。如果没有，它会以 DNS 客户端的身份向根域名服务器进行查询。[根域名服务器](https://www.zhihu.com/search?q=%E6%A0%B9%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2051992590%7D)收到本地域名服务器的查询后，会返回相关域名服务器的 IP 地址，本地域名服务器再向相关域名服务器发送查询请求。

![image.png](/computer/network/dns/dns-5.png)

### 根域名服务器
通常**根域名服务器**用来管理顶级域，本身并不对域名进行解析，但它知道相关域名服务器的 IP 地址。IPv4 根域名服务器全球有 13 台，主机名分别为 A ~ M 。1 台为主根服务器在美国，其余 12 个均为[辅根服务器](https://www.zhihu.com/search?q=%E8%BE%85%E6%A0%B9%E6%9C%8D%E5%8A%A1%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2051992590%7D)，其中 9 个在美国，2 个在欧洲，位于英国和瑞典，1 台在亚洲，位于日本。**_所有的域名服务器都必须注册根域名服务器的 IP 地址_**，因为 DNS 根据 IP 地址进行检索时，需要按顺序从根域名服务器开始。

![image.png](/computer/network/dns/dns-6.png)

### 授权域名服务器
互联网上的主机在域名服务器上进行注册，这个域名服务器就是主机的**授权域名服务器**。通常，主机的[授权域名服务器](https://www.zhihu.com/search?q=%E6%8E%88%E6%9D%83%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2051992590%7D)就是_本地域名服务器_。实际上，主机会有两个授权域名服务器，防止单点故障。授权域名服务器上有注册主机域名与 IP 地址的映射信息，当查询注册主机域名时，它会返回相应主机的 IP 地址。如果主机域名和 IP 地址需要进行变更，只需要在授权域名服务器处理即可，不用再向其它域名服务器进行申请或报告。

![image.png](/computer/network/dns/dns-7.png)

### 主域名服务器
**主域名服务器**是完成一个或多个区域域名解析工作的主要域名服务器，通常也是一个或多个区域的授权域名服务器。主域名服务器有区域内主机地址信息的源数据文件，并且是区域传送中区域数据的唯一来源。
### 辅助域名服务器
**辅助域名服务器**可以协助主域名服务器提供域名查询服务，在主机很多的情况下，可以有效分担主域名服务器的压力。辅助域名服务器也有冗余功能，当主域名服务器故障时，辅助域名服务器能够在数据有效期内继续为主机提供域名解析服务。

![image.png](/computer/network/dns/dns-8.webp)

一台主域名服务器可以有多台辅助域名服务器，一台辅助域名服务器也可以是其它区域的主域名服务器。辅助域名服务器中包含区域内主机地址数据的授权信息，通过区域配置_文件副本_的方式存储。辅助域名服务器也是区域的授权域名服务器，可以完成本区域内域名查询的授权回答。
辅助域名服务器并不建立数据文件，它获得区域数据的唯一途径就是通过_区域传送_的方式，从主域名服务器上获得区域数据的最新副本。获得数据副本的方式有两种，第一种是辅助域名服务器定期主动获得主域名服务器的副本，或更新副本中的数据。第二种是主域名服务器在区域数据发生变化时，及时通知辅助域名服务器更新副本中的数据。
## DNS 域名完整解析过程

![image.png](/computer/network/dns/dns-9.png)

DNS 客户端进行域名 [http://www.tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//www.tsinghua.edu.cn) 的解析过程如下：

1. **DNS 客户端**向本地域名服务器发送请求，查询 [http://www.tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//www.tsinghua.edu.cn) 主机的 IP 地址；
2. **本地域名服务器**查询数据库，发现没有域名为 [http://www.tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//www.tsinghua.edu.cn) 的主机，于是将请求发送给根域名服务器；
3. **根域名服务器**查询数据库，发现没有这个主机域名记录，但是根域名服务器知道 cn 域名服务器可以解析这个域名，于是将 cn 域名服务器的 IP 地址返回给本地域名服务器；
4. **本地域名服务器**向 cn 域名服务器查询 [http://www.tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//www.tsinghua.edu.cn) 主机的 IP 地址；
5. **cn 域名服务器**查询数据库，也没有相关记录，但是知道 [http://edu.cn](https://link.zhihu.com/?target=http%3A//edu.cn) 域名服务器可以解析这个域名，于是将 [http://edu.cn](https://link.zhihu.com/?target=http%3A//edu.cn) 域名服务器的 IP 地址返回给本地域名服务器；
6. **本地域名服务器**再向 [http://edu.cn](https://link.zhihu.com/?target=http%3A//edu.cn) 域名服务器查询 [http://www.tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//www.tsinghua.edu.cn) 主机 IP 地址；
7. [http://edu.cn](https://link.zhihu.com/?target=http%3A//edu.cn)**域名服务器**查询数据库，也没有相关记录，但是知道 [http://tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//tsinghua.edu.cn) 域名服务器可以解析这个域名，于是将 [http://tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//tsinghua.edu.cn) 的域名服务器 IP 地址返回给本地域名服务器；
8. **本地域名服务器**向 [http://tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//tsinghua.edu.cn) 域名服务器查询 [http://www.tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//www.tsinghua.edu.cn) 主机的 IP 地址；
9. [http://tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//tsinghua.edu.cn)**域名服务器**查询数据库，发现有主机域名记录，于是给本地域名服务器返回 [http://www.tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//www.tsinghua.edu.cn) 对应的 IP 地址；
10. 最后**本地域名服务器**将 [http://www.tsinghua.edu.cn](https://link.zhihu.com/?target=http%3A//www.tsinghua.edu.cn) 的 IP 地址返回给客户端，整个解析过程完成。

## DNS 解析方式

DNS解析流程分为递归查询和迭代查询，DNS客户端和本地名称服务器是递归，而本地名称服务器和其他名称服务器之间是迭代。

### 递归

递归查询是以本地名称服务器为中心查询，递归查询是默认方式。

![递归](/computer/network/dns/digui.png)

### 迭代

迭代查询是以DNS客户端，也就是客户机器为中心查询。

![迭代](/computer/network/dns/diedai.png)