- [基础](#基础)
    - [Web](#Web)
    - [HTML](#HTML)
    - [WSGI、uwsgi、uWSGI](#WSGI、uwsgi、uWSGI)
    - [web服务器和web应用程序](#web服务器和web应用程序)
    - [Nginx](#Nginx)
        - [简介](#简介)
        - [正向代理](#正向代理)
        - [反向代理](#反向代理)
        - [负载均衡](#负载均衡)
        - [动静分离](#动静分离)
            - [访问静态网站实现](#访问静态网站实现)
            - [访问动态网站实现](#访问动态网站实现)
        - [限流](#限流)
        - [配置文件](#配置文件)
        - [Nginx常用命令](#Nginx常用命令)
    - [后端框架](#后端框架)
        - [Flask(Python)](#Flask(Python))
        - [Tornado(Python)](#Tornado(Python))
        - [Gin(Golang)](#Gin(Golang))
    - [SQLAlchemy](#SQLAlchemy)
    - [Protobuf](#Protobuf)
    - [RESTful-API](#RESTful-API)
- [面试](#面试)
    - [简述Nginx](#简述Nginx)
    - [Nginx如何做到高并发下的高效处理](#Nginx如何做到高并发下的高效处理)
    - [怎么保证Nginx的高可用](#怎么保证Nginx的高可用)
    - [sqlalchemy的优点、缺点](#sqlalchemy的优点、缺点)
    - [cookie和session的区别](#cookie和session的区别)
    - [常用的http返回状态码](#常用的http返回状态码)
    - [简述RESTful-API](#简述RESTful-API)

---
# 基础
## Web
web（World Wide Web）即全球广域网，也称为万维网，它是一种基于超文本和HTTP的、全球性的、动态交互的、跨平台的分布式图形信息系统。是建立在Internet（因特网）上的一种网络服务，为浏览者在Internet上查找和浏览信息提供了图形化的、易于访问的直观界面，其中的文档及超级链接将Internet上的信息节点组织成一个互为关联的网状结构。

因特网（Internet）是一组全球信息资源的总汇。有一种粗略的说法，认为Internet是由于许多小的网络（子网）互联而成的一个逻辑网，每个子网中连接着若干台计算机（主机）。Internet以相互交流信息资源为目的，基于一些共同的协议，并通过许多路由器和公共互联网组成，它是一个信息资源和资源共享的集合。
## HTML
超文本：不止文本，还可以有链接、图片。描述超文本的方式有很多，例如：HTML，TGML，还有markdown。超文本的用途也很多，例如：描述一个网页，或者描述一个Word文档。<br>
HTML：用于描述超文本。HTML是人与浏览器沟通的语言，告诉浏览器该怎么表现出我们要的东西。

## WSGI、uwsgi、uWSGI
* WSGI：Web服务器网关接口。是一种规范，一种通信协议，提供了一种标准，一种sever与application之间的标准，**它定义了使用web应用程序与web服务器程序之间的接口格式，实现web应用程序与web服务器程序间的解耦**。要实现WSGI协议，必须同时实现web server和web application。可以有多个实现WSGI server的服务器，也可以有多个实现WSGI application的框架，可以选择任意的server和application组合实现自己的web应用，可以根据项目实际情况搭配使用。<br>
* uwsgi：与WSGI一样是一种通信协议，是uWSGI服务器的独占协议，用于定义传输信息的类型(type of information)，每一个uwsgi packet前4byte为传输信息类型的描述，与WSGI协议是两种东西，据说该协议是fcgi协议的10倍快。
* uWSGI：uWSGI是一个Web服务器，实现了WSGI协议、uwsgi协议、http协议等。uWSGI服务器自己实现了基于uwsgi协议的server部分，只需要在uwsgi的配置文件中指定application的地址，uWSGI就能直接和应用框架中的WSGI application通信。

## web服务器和web应用程序
* web服务器：web服务器用于处理静态页面的内容，对于脚本语言产生的动态内容，它通过WSGI或者uwsgi接口交给web应用程序来处理。负责从客户端接收请求，将请求转发给web应用程序，将web应用程序返回的响应返回给客户端。<br>
* web应用程序：用来处理客户端的动态请求，并返回给web服务器。

web服务器和web应用程序之间的连接依靠WSGI和uwsgi之类的协议等。

常见的web服务器和web应用程序
* web服务器：Apache、[Nginx](#Nginx)、Gunicorn
* web应用程序：Flask、Django、PHP、Java、Asp.net等应用程序。

Django，Flask是实现了WSGI application协议的web框架。

## Nginx
[8分钟带你深入浅出搞懂Nginx](https://zhuanlan.zhihu.com/p/34943332)<br>
[连前端都看得懂的《Nginx 入门指南》](https://juejin.cn/post/6844904129987526663)<br>
[高性能 -Nginx 多进程高并发、低时延、高可靠机制在百万级缓存 (redis、memcache) 代理中间件中的应用](https://xie.infoq.cn/article/2ee961483c66a146709e7e861)<br>
[php-fpm](./php.md#php-fpm)
### 简介
Nginx是一款轻量级的Web服务器、反向代理服务器及电子邮件（IMAP/POP3）代理服务器。

**特点**
* 内存占用少
* 启动快
* 并发能力强：基于异步IO模型，事件驱动，（[epoll](./operating_system.md#epoll)，kqueue）

**主要提供的服务**
* web服务
* 反向代理
* 负载均衡
* 动静分离
* 限流
* 缓存

优点
* 高并发、高性能（这是其他web服务器不具有的）
* 可扩展性好（模块化设计，第三方插件生态圈丰富）
* 高可靠性（可以在服务器行持续不间断的运行数年）
* 热部署（这个功能对于Nginx来说特别重要，热部署指可以在不停止Nginx服务的情况下升级Nginx）
* BSD许可证（意味着可以将源代码下载下来进行修改然后使用自己的版本）

对比：
* Apache：高并发时消耗系统资源相对多一些；基于传统的[select](./operating_system.md#select)模型；
* nginx：消耗代码资源比较低；基于异步IO模型，（[epoll](./operating_system.md#epoll)，kqueue），性能强，能够支持上万并发；对小文件支持很好，性能很高（限静态小文件1M）。

### 正向代理
> 概念<br>
为客户端做代理，代替客户端去访问服务器。<br>
特点<br>
正向代理最大的特点是客户端非常明确要访问的服务器地址。服务器只清楚请求来自哪个代理服务器，而不清楚来自哪个具体的客户端。正向代理模式屏蔽或者隐藏了真实客户端信息。客户端必须要进行一些特别的设置才能使用正向代理。<br>
实例<br>
VPN。正向代理“代理”的是客户端，而且客户端是知道目标的，而目标不知道客户端是通过VPN访问的。<br>
作用<br>
访问原来无法访问的资源，如 Google。可以做缓存，加速访问资源。对客户端访问授权，上网进行认证。代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息。

### 反向代理
> 概念<br>
为服务器做代理，代替服务器接受客户端请求。此时代理服务器对外就表现为一个服务器。<br>
特点<br>
客户端明确，服务端不明确。如有多个服务器，由Nginx的[负载均衡算法](#负载均衡)决定连接到哪个服务器。<br>
实例<br>
在外网访问百度的时候，其实会进行一个转发，代理到内网去，这就是所谓的反向代理，即反向代理“代理”的是服务器端，而且这一个过程对于客户端而言是透明的。<br>
作用<br>
可以起到保护网站安全的作用，因为任何来自Internet的请求都必须先经过代理服务器。<br>
使用场景<br>
主要用于服务器集群分布式部署的情况下，反向代理隐藏了服务器的信息。

### **负载均衡**
客户端发送的、Nginx反向代理服务器接收到的请求数量，就是负载量。请求数量按照一定的规则进行分发，到不同的服务器处理的规则，就是一种均衡规则。所以将服务器接收到的请求按照规则分发的过程，称为负载均衡。**在高并发情况下需要使用，其原理就是将并发请求分摊到多个服务器执行，减轻每台服务器的压力，多台服务器(集群)共同完成工作任务，从而提高了数据的吞吐量**。

nginx支持的四种负载均衡调度算法
* weight轮询（默认）：接收到的请求按照顺序逐一分配到不同的后端服务器，即使在使用过程中，某一台后端服务器宕机，Nginx会自动将该服务器剔除出队列，请求受理情况不会受到任何影响。这种方式下，可以给不同的后端服务器设置一个权重值（weight），用于调整不同的服务器上请求的分配率。权重数据越大，被分配到请求的几率越大。该权重值，主要是针对实际工作环境中不同的后端服务器硬件配置进行调整的。
* ip_hash：每个请求按照发起客户端的ip的hash结果进行匹配，这样的算法下一个固定ip地址的客户端总会访问到同一个后端服务器，这也在一定程度上解决了集群部署环境下Session共享的问题。
* fair：智能调整调度算法，动态的根据后端服务器的请求处理到响应的时间进行均衡分配。响应时间短处理效率高的服务器分配到请求的概率高，响应时间长处理效率低的服务器分配到的请求少，它是结合了前两者的优点的一种调度算法。但是需要注意的是Nginx默认不支持fair算法，如果要使用这种调度算法，要安装upstream_fair模块。
* url_hash：按照访问的URL的hash结果分配请求，每个请求的URL会指向后端固定的某个服务器，可以在Nginx作为静态服务器的情况下提高缓存效率。同样要注意Nginx默认不支持这种调度算法，要使用的话需要安装Nginx的hash软件包。

**负载均衡可能带来的问题**<br>
负载均衡所带来的明显的问题是：一个请求，可以到A server，也可以到B server，需要注意的是：用户状态的保存问题，如Session会话信息，不能再保存到服务器上。解决方法：将Session单独存储到服务器上或者Redis中。

### 动静分离
动静分离是让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，就可以根据静态资源的特点将其做缓存操作，这就是网站静态化处理的核心思路。

静态资源和动态资源<br>
* 静态资源一般都是设计好的html页面，图片、视频等。而动态资源依靠设计好的程序来实现按照需求的动态响应。
* 静态资源的交互性差，动态资源可以根据需求自由实现。
* 在服务器的运行状态不同，静态资源不需要数据库参于程序处理，动态可能需要多个数据库的参与运算。
#### 访问静态网站实现
![](../pictures/web/visit_static_website.png)
#### 访问动态网站实现
![](../pictures/web/visit_dynamic_website.png)

### 限流
[参考](https://www.cnblogs.com/biglittleant/p/8979915.html)
* 限制单位时间内的请求数，即速率限制，采用漏桶算法。
* 限制同一时间连接数，即并发限制。
* 可以通过burst关键字开启对突发请求的缓存处理，而不是直接拒绝。

### 配置文件
[参考1](https://blog.csdn.net/finnson/article/details/82461600)
[参考2](https://blog.csdn.net/qq_39591494/article/details/78857677)

### Nginx常用命令
```bash
/usr/local/bin/nginx -s reload  # 重新载入配置文件，Nginx服务不会中断
/usr/local/bin/nginx -s reopen  # 重启 Nginx，会造成服务一瞬间的中断
/usr/local/bin/nginx -s stop    # 停止 Nginx
```

## 后端框架
### Flask(Python)
### Tornado(Python)
### Gin(Golang)

## SQLAlchemy
优点
* 对数据表的抽象，允许开发人员首先考虑数据模型，同时使得Python程序更加简洁易读。
* 对各种数据库引擎的封装，使得开发人员在面对不同数据库时，只需要做简单修改即可，工作量大大减少。

## Protobuf
Protocol buffers是一种语言中立，平台无关，可扩展的序列化数据的格式，可用于通信协议，数据存储等。<br>
Protocol buffers在序列化数据方面，是灵活的，高效的。相比于XML来说，Protocol buffers更加小巧，更加快速，更加简单。一旦定义了要处理的数据的数据结构，就可以利用Protocol buffers的代码生成工具生成相关的代码。甚至可以在无需重新部署程序的情况下更新数据结构。只需使用Protobuf对数据结构进行一次描述，即可利用各种不同语言或从各种不同数据流中对结构化数据轻松读写。<br>
Protocol buffers很适合做数据存储或RPC数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。

## **RESTful API**
RESTful API是一套协议，用来规范多种形式的前端和同一个后台的交互方式。
设计原则和规范：
* 资源：资源是网络上的一个实体，一段文本，一张图片或者一首歌曲。资源总是要通过一种载体来反映它的内容。文本可以用TXT，也可以用HTML或者XML，图片可以用JPG格式或者PNG格式，JSON是现在最常用的资源表现形式。
* 统一接口：RESTful风格的数据操作CRUD（create，read，update，delete）分别对应HTTP方法：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源，这样就统一了数据操作的接口。
* URI：可以用一个URI（统一资源定位符）指向资源，即每个URI都对应一个特定的资源。要获取这个资源访问它的URI就可以，因此URI就成了每一个资源的地址或识别符。一般的，每个资源至少有一个URI与之对应，最典型的URI就是URL。
* 无状态：所谓无状态即所有的资源都可以URI定位，而且这个定位与其他资源无关，也不会因为其他资源的变化而变化。

**RESTful API中，URL中只使用名词来指定资源，原则上不使用动词。对于资源的操作类型应该通过http动词表示，post/get/put/delete分别对应数据库的CRUD操作**。<br>
优点：行为与资源分离，更容易理解。<br>
缺点：业务逻辑有时难以被抽象为资源的增删改查。有些对象不能抽象为资源，比如登陆（login）本来就是一个动作。

# 面试
## 简述Nginx
[Nginx](#nginx)
## Nginx如何做到高并发下的高效处理
Nginx采用了Linux的epoll模型，epoll模型基于事件驱动机制，它可以监控多个事件是否准备完毕，如果OK，那么放入epoll队列中，这个过程是异步的。worker只需要从epoll队列循环处理即可。
### 怎么保证Nginx的高可用
Keepalived+Nginx实现高可用，解决单点问题。<br>
Keepalived是一个高可用解决方案，主要是用来防止服务器单点发生故障，可以通过和Nginx配合来实现Web服务的高可用。（其实，Keepalived不仅仅可以和Nginx配合，还可以和很多其他服务配合）<br>
Keepalived+Nginx实现高可用的思路：
1. 请求不要直接打到Nginx上，应该先通过Keepalived（这就是所谓虚拟IP，VIP）
2. Keepalived应该能监控Nginx的生命状态（提供一个用户自定义的脚本，定期检查Nginx进程状态，进行权重变化，从而实现Nginx故障切换）。

![](../pictures/web/keepalived_nginx.jpg)

## **后端框架的区别**
* django：同步框架。基于WSGI。相比flask更原生。必须使用内置的orm，Django ORM与数据库的交互上慢，Django自带的ORM远不如SQLAlchemy强大。部署的时候可以使用gunicorn（可以部署多进程）+gevent（协程），写同步的方式写异步。
* flask：轻量级，同步框架。基于WSGI。加上celery+redis的异步特性，性能与tornado相当。更灵活，可以自由选择数据库交互组件，扩展性好。部署的时候可以使用gunicorn（可以部署多进程）+gevent（协程），写同步的方式写异步。
* tornado：异步框架，处理http请求时速度更快，时间更短，非阻塞+[epoll](./operating_system.md#epoll)。不基于WSGI，可以作为web服务器。相比django是更原始的框架。Tornado没有提供异步的ORM工具。sqlalchemy是同步的，tornado是异步的，组合效果没有sqlalchemy+flask好。是单进程、单线程的，使用协程实现高并发。

## sqlalchemy的优点、缺点
优点：隐藏数据访问细节，切换数据库的时候方便，不需要做大量的修改，提高开发效率。<br>
缺点：不容易处理复杂查询语句。进行关系数据的映射，会影响性能，现在已经通过方法（LazyLoad，Cache）减轻这种影响了。
## **cookie和session的区别**
都是为了存储用户的相关信息。<br>
cookie以文本格式存储在浏览器上，存储量有限；而session（会话）存储在服务端，可以无限量存储多个变量并且比cookie更安全。

## **常用的http返回状态码**
状态码 | 含义
---|---
200 | 请求成功
301 | 永久性重定向
302 | 临时性重定向
401 | 未经授权
403 | 拒绝访问
404 | 找不到请求的资源
500 | 服务器内部错误
503 | 服务器忙
## **简述RESTful API**
**RESTful API中，URL中只使用名词来指定资源，原则上不使用动词。对于资源的操作类型应该通过http动词表示，post/get/put/delete分别对应数据库的CRUD操作**。<br>
优点：行为与资源分离，更容易理解。<br>
缺点：业务逻辑有时难以被抽象为资源的增删改查。有些对象不能抽象为资源，比如登陆（login）本来就是一个动作。