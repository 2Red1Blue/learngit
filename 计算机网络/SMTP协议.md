### 概念与约定

+ **简单邮件传输协议 (SMTP)** 是事实上的在Internet传输email的标准。
+ **邮局协议**（**POP**）是TCP/IP协议族中的一员，主要用于支持使用**客户端**远程管理在服务器上的电子邮件。最新版本为**POP3**，而提供了[SSL](https://zh.wikipedia.org/wiki/SSL)加密的POP3协议被称为**POP3S**。
+ **Internet Message Access Protocol**（缩写为**IMAP**）是一个应用层协议，用来从本地邮件客户端（如Microsoft Outlook、Outlook Express、Foxmail、Mozilla Thunderbird）访问远程服务器上的邮件。
+ POP3和IMAP的区别主要是：
  + POP3是比较老的protocol，主要为了解决本地机器和远程邮件服务器链接的问题，每次邮件会download到本地机器，然后从远程邮件服务器上删掉（当然特殊config除外），然后进行本地编辑。这样的问题是如果从多个终端链接服务器，只有第一个下载的能看到，现在pop4正在讨论中。
  + IMAP是相对比较新的protocol，可以将邮件分文件夹整理，然后这些信息也存在远程的邮件服务器上，读取邮件后，服务器上不删除。原理上IMAP应该是相当于oneline编辑，但现在的mail client基本都有在本地存copy的功能。
+ **多用途互联网邮件扩展**（**MIME**）是一个互联网标准，它扩展了电子邮件标准，使其能够支援：
  - 非ASCII字符文本；
  - 非文本格式附件（二进制、声音、图像等）；
  - 由多部分（multiple parts）组成的消息体；
  - 包含非ASCII字符的头信息（Header information）
+ 此外，HTTP协议中也使用了MIME的框架，标准被扩展为互联网媒体类型。

### SMTP概述

+ SMTP用于将邮件从发送方推送到接收方的**服务器**。(SMTP负责发，POP3/IMAP负责收)
+ SMTP是一个“推”的协议，它不允许根据需要从远程服务器上“拉”来消息。要做到这点，邮件客户端必须使用[POP3](https://zh.wikipedia.org/wiki/%E9%83%B5%E5%B1%80%E5%8D%94%E5%AE%9A)或[IMAP](https://zh.wikipedia.org/wiki/IMAP)。
+ SMTP的局限之一在于它没有对发送方进行身份验证的机制。因此，后来定义了SMTP-AUTH扩展。
+ 只支持7位ASCII码，因此发送非文本之前必须经过一次编码，收到之后必须经过解码才能查看。

### 与HTTP的对比

#### 共同点

1. 都是用于传输文件的
2. 都使用持续连接

#### 不同点

1. HTTP是拉协议，SMTP是推协议，即TCP连接发起者不一样，前者由接收方发起，后者由发送方发起。
2. HTTP无数据限制，SMTP只支持7位ASCII编码格式。
3. 对于富文本，HTTP把该文档拆分成多个部分，分开文本和其他媒体文件，分别发送；而SMTP则直接全部进行ASCII编码，全部放在一个报文之中。

### 邮件报文格式和MIME

通用邮件报文格式如下：

1. From字段：发送者
2. To字段：接受者
3. Subject字段：首部行
4. (空白行)
5. 正文报文体