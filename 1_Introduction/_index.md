---
title: "1. 引言"
anchor: "1_Introduction"
weight: 100
rank: "h1"
---

Domain Name System (DNS) concepts are specified in "Domain names - concepts and facilities" [RFC1034]. The transmission of DNS queries and responses over UDP and TCP is specified in "Domain names - implementation and specification" [RFC1035].

域名系统（DNS）这一概念是在《域名——概念与基础设施》（详见《[RFC1034]()》）中建立的。《域名——实现与规范》（详见《[RFC1035]()》）则介绍了DNS查询与响应基于UDP和TCP的传输方式。

This document presents a mapping of the DNS protocol over the QUIC transport [RFC9000] [RFC9001]. DNS over QUIC is referred to here as DoQ, in line with "DNS Terminology" [DNS-TERMS].

本文档描述了DNS协议的一种基于QUIC传输（详见《[RFC9000]()》和《[RFC9001]()》）的映射。下文使用DoQ来指代基于QUIC的DNS，这是符合“DNS术语”（详见《[DNS-TERMS]()》）的。

The goals of the DoQ mapping are:

DoQ映射的目标是：

1. Provide the same DNS privacy protection as DoT [RFC7858]. This includes an option for the client to authenticate the server by means of an authentication domain name as specified in "Usage Profiles for DNS over TLS and DNS over DTLS" [RFC8310].

1. 提供与DoT（详见《[RFC7858]()》）一致的DNS隐私保护。这包括为客户端提供一项可选的使用认证域名来验证服务器身份的机制，《基于TLS和DTLS的DNS的配置用例》（详见《[RFC8310]()》）中介绍了这项机制。

2. Provide an improved level of source address validation for DNS servers compared to classic DNS over UDP.

2. 比起传统的基于UDP的DNS，为DNS服务器提供经过改进的源地址验证机制。

3. Provide a transport that does not impose path MTU limitations on the size of DNS responses it can send.

3. 提供一种不会根据路径MTU来限制DNS响应尺寸的传输方式。

In order to achieve these goals, and to support ongoing work on encryption of DNS, the scope of this document includes:

为了达成以上目标，以及支持正在进行的DNS加密方面的工作，本文档讨论的范畴包含：

* the "stub to recursive resolver" scenario (also called the "stub to recursive" scenario in this document)

* “存根解析器与递归解析器交互”场景（本文档中也会称之为“存根与递归”场景）；

* the "recursive resolver to authoritative nameserver" scenario (also called the "recursive to authoritative" scenario in this document), and

* “递归解析器与权威域名服务器交互”场景（本文档中也会称之为“递归与权威”场景）；以及

* the "nameserver to nameserver" scenario (mainly used for zone transfers (XFR) [RFC1995] [RFC5936]).

* “域名服务器与域名服务器交互”场景（主要被用于区域传送（XFR，详见《[RFC1995]()》和《[RFC5936]()》））。

In other words, this document specifies QUIC as a general-purpose transport for DNS.

换句话说，本文档将QUIC视作DNS的一种通用传输方式。

The specific non-goals of this document are:

以下内容并非本文档的目标：

1. No attempt is made to evade potential blocking of DoQ traffic by middleboxes.

1. 致力于回避中间设备可能引发的DoQ流量阻塞。

2. No attempt to support server-initiated transactions, which are used only in DNS Stateful Operations (DSO) [RFC8490].

2. 支持由服务器发起的事务，这只会在DNS有状态操作（DSO，详见《[RFC8490]()》）中被用到。

Specifying the transmission of an application over QUIC requires specifying how the application's messages are mapped to QUIC streams, and generally how the application will use QUIC. This is done for HTTP in "Hypertext Transfer Protocol Version 3 (HTTP/3)" [HTTP/3]. The purpose of this document is to define the way DNS messages can be transmitted over QUIC.

要为基于QUIC的应用制定传输方式，需要规定如何将应用消息映射到QUIC流上，以及指定应用如何使用QUIC。《超文本传输协议版本3（HTTP/3）》（详见《[HTTP/3]()》）中就是这么做的。本文档的意图是定义如何经由QUIC来传输DNS消息。

DNS over HTTPS (DoH) [RFC8484] can be used with HTTP/3 to get some of the benefits of QUIC. However, a lightweight direct mapping for DoQ can be regarded as a more natural fit for both the recursive to authoritative and zone transfer scenarios, which rarely involve intermediaries. In these scenarios, the additional overhead of HTTP is not offset by, for example, benefits of HTTP proxying and caching behavior.

可以结合使用HTTP/3和经由HTTPS的DNS（DoH，详见《[RFC8484]()》）来利用QUIC的部分优势。然而，直接且轻量的DoQ映射天生更适用于权威性递归和区域传送的场景，这些场景几乎不会牵涉到中间设备。而在这些场景中，HTTP的额外开销并不会因为HTTP代理或缓存行为而被消除。

In this document, Section 3 presents the reasoning that guided the proposed design. Section 4 specifies the actual mapping of DoQ. Section 5 presents guidelines on the implementation, usage, and deployment of DoQ.

在本文档中，[第3章]()介绍了本设计是如何成型的。[第4章]()规定了DoQ实际的映射方式。[第5章]()指导了如何实现、使用和部署DoQ。
