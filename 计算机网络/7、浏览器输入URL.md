## 页面从输入URL到页面加载显示完成，这个过程中都发生了什么？

![img](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-11/url%E8%BE%93%E5%85%A5%E5%88%B0%E5%B1%95%E7%A4%BA%E5%87%BA%E6%9D%A5%E7%9A%84%E8%BF%87%E7%A8%8B.jpg)



![http](https://pic.xiaohuochai.site/blog/httpUrl1.png)

总体来说分为以下几个过程:

1. DNS解析
2. TCP连接
3. 发送HTTP请求
4. 服务器处理请求并返回HTTP报文
5. 浏览器解析渲染页面
6. 连接结束

## 一、构建请求

**DNS解析URL地址**

**生成HTTP请求报文**

**构建TCP连接**

**使用IP协议选择传输路线**

**数据链路层保证数据的可靠传输**

**物理层将数据转换成电子、光学或微波信号进行传输**

### （1）解释一下URL

> 协议、域名、端口、路径、查询、其他的信息片段

- Scheme: 通信协议，一般为http、https等；
- Host: 服务器的域名主机名或ip地址；
- Port: 端口号，此项为可选项，默认为80；
- Path: 目录，由“/”隔开的字符串，表示的是主机上的目录或文件地址；
- Query: 查询，此项为可选项，可以给动态网页传递参数，用“&”隔开，每个参数的名和值用“=”隔开；
- Fragment: 信息片段，对一个带有章节的大型文本文档来说，资源的 URL 会指向整个文本文档，但是我们可以根据片段来显示我们感兴趣的章节，片段表示一小片或一部分资源的名字，用 # 与其他组件隔开

### （2）DNS域名解析

> 我们知道在地址栏输入的域名并不是最后资源所在的真实位置，域名只是与IP地址的一个映射。网络服务器的IP地址那么多，我们不可能去记一串串的数字，因此域名就产生了，域名解析的过程实际是将域名还原为IP地址的过程。

在解析过程中，按照`浏览器缓存`、`系统缓存`、`路由器缓存`、`ISP(运营商)DNS缓存`、`递归搜索`【`根域名服务器`、`顶级域名服务器`、`主域名服务器`】的顺序，逐步读取缓存，直到拿到IP地址

这里使用DNS预解析，可以根据浏览器定义的规则，提前解析之后可能会用到的域名，使解析结果缓存到`系统缓存`中，缩短DNS解析时间，来提高网站的访问速度

### （3）应用层生成HTTP请求报文

> 接着，应用层生成针对目标WEB服务器的HTTP请求报文

HTTP请求报文包括`起始行`、`请求头部`、`空行`和`请求主体`

请求方的http报头结构：**通用报头|请求报头|实体报头**

+ 如果访问的google.com，则起始行可能如下

  + ```
    GET https://www.google.com/ HTTP/1.1
    ```

+ 请求头部可能如下

  + Host：请求的主机名，允许多个域名同处一个IP地址，即虚拟主机。

  + User-Agent：用户浏览器的类型版本信息

  + Accept：浏览器能识别的网络文件的类型

  + Accept-Charset：浏览器能够识别的字符集

  + Accept-Encoding：浏览器能识别的内容压缩编码格式

  + Accept-Language：浏览器能识别的语言

  + Connection：浏览器与服务器之间连接的类型，如`keep-alive`

  + Cookie：当前页面设置的任何Cookie

  + Referer：发出请求的页面的URL

  + Cache-Control：指定请求和响应遵循的缓存机制，如`max-age=0`、`no-cache`

  + Content-Length：请求的内容长度

  + Date：请求发送的日期和时间

  + If-Modified-Since：如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码

  + 与请求数据相关的最常使用的请求头是Content-Type和Content-Length。

  + **Content-Type**：用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件，告诉客户端实际返回的内容的内容类型。

    +  发送端（客户端|服务器）发送的实体数据的数据类型。
  
    + | 常见的媒体格式类型如下： |             |
      | ------------------------ | ----------- |
      | text/html                | HTML格式    |
      | text/plain               | 纯文本格式  |
      | text/xml                 | XML格式     |
      | image/gif                | gif图片格式 |
    | image/jpeg               | jpg图片格式 |
      | image/png                | png图片格式 |
  
    + | 以application开头的媒体格式类型： |                                                              |
      | --------------------------------- | ------------------------------------------------------------ |
      | application/xhtml+xml             | XHTML格式                                                    |
      | application/xml                   | XML数据格式                                                  |
      | application/atom+xml              | Atom XML聚合格式                                             |
      | application/json                  | JSON数据格式                                                 |
      | application/pdf                   | pdf格式                                                      |
      | application/msword                | Word文档格式                                                 |
    | application/octet-stream          | 二进制流数据（如常见的文件下载）                             |
      | application/x-www-form-urlencoded | form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式） |
  
    + | 另一种常见的媒体格式是上传文件之时使用的： |                                              |
    | ------------------------------------------ | -------------------------------------------- |
      | multipart/form-data                        | 需要在表单中进行文件上传时，就需要使用该格式 |
  
  + Cookie：Cookie是一小段文本，当用户访问站点时，可以对用户的浏览器种下一个cookie用以保存用户的相关信息，然后当用户下次进行访问时，站点会先读取用户的cookie，然后呈现相应的页面（例如：登陆一次后，下次再进该网页就自动登录）

### （4）传输层建立TCP连接

> 传输层传输协议分为UDP和TCP两种
>
> UDP是无连接的协议，而TCP是可靠的有连接的协议，主要表现在：接收方会对收到的数据进行确认、发送方会重传接收方未确认的数据、接收方会将接收到数据按正确的顺序重新排序，并删除重复的数据、提供了拥挤控制的机制

​	**由于HTTP协议使用的是TCP协议，为了方便通信，将HTTP请求报文按序号分为多个报文段(segment)，并对每个报文段进行封装。**

​	使用本地一个大于1024以上的随机TCP源端口(这里假设是1030)建立到目的服务器TCP80号端口(HTTPS协议对应的端口号是443)的连接，TCP源端口和目的端口被加入到报文段中，学名叫协议数据单元(Protocol Data Unit, PDU)。因TCP是一个可靠的传输控制协议，传输层还会加入序列号、确认号、窗口大小、校验和等参数，共添加20字节的头部信息

​	TCP协议是面向连接的，所以它在开始传输数据之前需要先建立连接。要建立或初始化一个连接，两端主机必须同步双方的初始序号。同步是通过交换连接建立数据分段和初始序号来完成的，在连接建立数据分段中包含一个SYN(同步)的控制位。同步需要双方都发送自己的初始序号，并且发送确认的ACK。此过程就是三次握手

![img](https://images2015.cnblogs.com/blog/1034346/201703/1034346-20170329145607592-1103856922.png)

构建TCP请求会增加大量的网络时延，常用的优化方式如下所示

+ 资源打包，合并请求
+ 多使用缓存，减少网络传输
+ 使用keep-alive建立持久连接
+ 使用多个域名，增加浏览器的资源并发加载数
+ 使用HTTP2的管道化连接的多路复用技术

### （5）网络层使用IP协议来选择路线

处理来自传输层的数据段segment，将数据段segment装入数据包packet，填充**包头**，主要就是添加源和目的IP地址，然后发送数据。在数据传输的过程中，IP协议负责选择传送的路线，称为路由功能

### （6）数据链路层实现网络相邻结点间可靠的数据通信

为了保证数据的可靠传输，把数据包packet封装成帧(Frame)，并按顺序传送各帧。由于物理线路的不可靠，发出的数据帧有可能在线路上出错或丢失，于是为每个数据分块计算出CRC(循环冗余检验)，并把CRC添加到帧中，这样接收方就可以通过重新计算CRC来判断数据接收的正确性。一旦出错就重传

将数据包packet封装成帧(Frame)，包括**帧头**和**帧尾**。帧尾是添加被称做CRC的循环冗余校验部分。帧头主要是添加数据链路层的地址，即数据链路层的源地址和目的地址，即网络相邻结点间的源MAC地址和目的MAC地址

### （7）物理层传输数据

数据链路层的帧(Frame)转换成二进制形式的比特(Bit)流，从网卡发送出去，再把比特转换成电子、光学或微波信号在网络中传输



## 二、网络传输

**从客户机到服务器需要通过许多网络设备， 一般地，包括集线器、交换器、路由器等**

#### 【集线器】

　　集线器是物理层设备，比特流到达集线器后，集线器简单地对比特流进行放大，从除接收端口以外的所有端口转发出去

#### 【交换机】

　　交换机是数据链路层设备，比特流到达交换机，交换机除了对比特流进行放大外，还根据源MAC地址进行学习，根据目的MAC地址进行转发。交换机根据数据帧中的目的MAC地址査询MAC地址表，把比特流从对应的端口发送出去

#### 【路由器】

　　路由器是网络层设备，路由器收到比特流，转换成帧上传到数据链路层，路由器比较数据帧的目的MAC地址，如果有与路由器接收端口相同的MAC地址，则路由器的数据链路层把数据帧进行解封装，然后上传到路由器的网络层，路由器找到数据包的目的IP地址，并查询路由表，将数据从入端口转发到出端口。接着在网络层重新封装成数据包packet，下沉到数据链路层重新封装成帧frame，下沉到物理层，转换成二进制比特流，发送出去

![http](https://pic.xiaohuochai.site/blog/HTTP_transport6.jpg)



## 三、服务器处理及反向传输

​	服务器接收到这个比特流，把比特流转换成帧格式，上传到数据链路层，服务器发现数据帧中的目的MAC地址与本网卡的MAC地址相同，服务器拆除数据链路层的封装后，把数据包上传到网络层。服务器的网络层比较数据包中的目的IP地址，发现与本机的IP地址相同，服务器拆除网络层的封装后，把数据分段上传到传输层。传输层对数据分段进行确认、排序、重组，确保数据传输的可靠性。数据最后被传到服务器的应用层

​	HTTP服务器，如nginx通过反向代理，将其定位到服务器实际的端口位置，如8080。比如，8080端口对应的是一个NodeJS服务，生成响应报文，报文主体内容是google首页的HTML页面

　接着，通过传输层、网络层、数据链路层的层层封装，最终将响应报文封装成二进制比特流，并转换成其他信号，如电信号到网络中传输

#### 常见状态码

- 200：OK（响应成功）；
- 301：Moved Permanently（页面已重定向）；
- 304：not modified（页面未修改）；
- 403：Forbidden（无权限访问）；
- 404：Not Found（服务器找到，但是没找到页面）；
- 500：Internal Server Error（服务器出错）；
- 502：bad getway（服务器未链接上）
- 特别的 304 Not Modified
  - 304码是页面未改变的意思，说明当前页面已访问过，且缓存在有效期内，则服务器回应last-modified给浏览器，浏览器进行判断后若缓存未改变，页面将直接从本地缓存中提取；
    - cache-control：max-age=30000（该页面缓存的有效时间秒数）
    - expires：mon 02 nov 2015 03：02：54 GMT   （该页面缓存过期时间）
  - 若页面缓存被清理，那么就重新发送请求；
  - 若缓存超过了有效期，则验证last-modified（最后修改时间）或者etag
    - last-modified：页面最后修改的时间；
    - etag：文本内容加密出来的一串字符串，若与之前相比未变则发送304；

## 五、浏览器渲染

见 `浏览器渲染.md` 文件

## 六、关闭TCP连接或继续保持连接

![img](https://images2015.cnblogs.com/blog/1034346/201703/1034346-20170329153945389-2019926409.png)

+ 第一次挥手是浏览器发完数据后，发送FIN请求断开连接。
+ 第二次挥手是服务器发送ACK表示同意，如果在这一次服务器也发送FIN请求断开连接似乎也没有不妥，因为考虑到服务器可能还有数据要发送，所以服务器发送FIN应该放在第三次挥手中。
+ 这样浏览器需要返回ACK表示同意，也就是第四次挥手。

