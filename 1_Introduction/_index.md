---
title: "1. 引言"
anchor: "1_Introduction"
weight: 100
rank: "h1"
---

域名系统（DNS）这一概念是在《域名——概念与基础设施》（详见《[RFC1034](https://www.rfc-editor.org/info/rfc1034)》）中建立的。
《域名——实现与规范》（详见《[RFC1035](https://www.rfc-editor.org/info/rfc1035)》）则介绍了DNS查询与响应基于UDP和TCP的传输方式。

本文档描述了DNS协议的一种基于QUIC传输（详见《[RFC9000](../RFC9000_Chinese_Simplified)》和《[RFC9001](../RFC9001_Chinese_Simplified)》）的映射。
下文使用DoQ来指代基于QUIC的DNS，这是符合“DNS术语”（详见《[DNS-TERMS](https://datatracker.ietf.org/doc/html/draft-ietf-dnsop-rfc8499bis-03)》）的。

DoQ映射的目标是：

1. 提供与DoT（详见《[RFC7858](https://www.rfc-editor.org/info/rfc7858)》）一致的DNS隐私保护。
这包括为客户端提供一项可选的使用认证域名来验证服务器身份的机制，《基于TLS和DTLS的DNS的配置用例》（详见《[RFC8310](https://www.rfc-editor.org/info/rfc8310)》）中介绍了这项机制。

2. 比起传统的基于UDP的DNS，为DNS服务器提供经过改进的源地址验证机制。

3. 提供一种不会根据路径MTU来限制DNS响应尺寸的传输方式。

为了达成以上目标，以及支持正在进行的DNS加密方面的工作，本文档讨论的范畴包含：

* “存根解析器与递归解析器交互”场景（本文档中也会称之为“存根与递归”场景）；

* “递归解析器与权威域名服务器交互”场景（本文档中也会称之为“递归与权威”场景）；以及

* “域名服务器与域名服务器交互”场景（主要被用于区域传送（XFR，详见《[RFC1995](https://www.rfc-editor.org/info/rfc1995)》和《[RFC5936](https://www.rfc-editor.org/info/rfc5936)》））。

换句话说，本文档将QUIC视作DNS的一种通用传输方式。

以下内容并非本文档的目标：

1. 致力于回避中间设备可能引发的DoQ流量阻塞。

2. 支持由服务器发起的事务，这只会在DNS有状态操作（DSO，详见《[RFC8490](https://www.rfc-editor.org/info/rfc8490)》）中被用到。

要为基于QUIC的应用制定传输方式，需要规定如何将应用消息映射到QUIC流上，以及指定应用如何使用QUIC。
《超文本传输协议版本3（HTTP/3）》（详见《[HTTP/3](https://datatracker.ietf.org/doc/html/draft-ietf-quic-http-34)》）中就是这么做的。
本文档的意图是定义如何经由QUIC来传输DNS消息。

可以结合使用HTTP/3和经由HTTPS的DNS（DoH，详见《[RFC8484](https://www.rfc-editor.org/info/rfc8484)》）来利用QUIC的部分优势。
然而，直接且轻量的DoQ映射天生更适用于权威性递归和区域传送的场景，这些场景几乎不会牵涉到中间设备。
而在这些场景中，HTTP的额外开销并不会因为HTTP代理或缓存行为而被消除。

在本文档中，[第3章](#3_Design_Considerations)介绍了本设计是如何成型的。
[第4章](#4_Specifications)规定了DoQ实际的映射方式。
[第5章](#5_Implementation_Requirements)指导了如何实现、使用和部署DoQ。
