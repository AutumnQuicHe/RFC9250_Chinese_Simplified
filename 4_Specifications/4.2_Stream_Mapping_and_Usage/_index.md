---
title: "4.2. 流的映射与用法"
anchor: "4.2_Stream_Mapping_and_Usage"
weight: 420
rank: "h2"
---

The mapping of DNS traffic over QUIC streams takes advantage of the QUIC stream features detailed in Section 2 of [RFC9000], the QUIC transport specification.

基于QUIC流的DNS流量映射利用了在QUIC传输规范《[RFC9000]()》的[第2章]()中介绍的QUIC流特性。

DNS query/response traffic [RFC1034] [RFC1035] follows a simple pattern in which the client sends a query, and the server provides one or more responses (multiple responses can occur in zone transfers).

DNS查询/响应的流量（详见《[RFC1034]()》和《[RFC1035]()》）遵循着简单的模式：由客户端发送一条查询，然后服务器提供一条或多条响应（多条响应会出现在区域传送场景中）。

The mapping specified here requires that the client select a separate QUIC stream for each query. The server then uses the same stream to provide all the response messages for that query. In order for multiple responses to be parsed, a 2-octet length field is used in exactly the same way as the 2-octet length field defined for DNS over TCP [RFC1035]. The practical result of this is that the content of each QUIC stream is exactly the same as the content of a TCP connection that would manage exactly one query.

本文规定的映射需要客户端为每次查询选择一条单独的QUIC流。接着服务器使用同一条流来为该查询提供所有响应消息。为了使得数条响应都能得到解析，会用到一个2字节的长度字段，其用法与在基于TCP的DNS（详见《[RFC1035]()》）中定义的2字节字段完全一致。该操作的作用是使得每条QUIC流的内容与传递单条查询的TCP连接的内容完全一致。

All DNS messages (queries and responses) sent over DoQ connections MUST be encoded as a 2-octet length field followed by the message content as specified in [RFC1035].

经由DoQ连接发送的所有DNS消息（查询与响应）**必须**以一个2字节的长度字段加上在《[RFC1035]()》中规定的消息内容的形式构造。

The client MUST select the next available client-initiated bidirectional stream for each subsequent query on a QUIC connection, in conformance with the QUIC transport specification [RFC9000]. Packet losses and other network events might cause queries to arrive in a different order. Servers SHOULD process queries as they arrive, as not doing so would cause unnecessary delays.

客户端**必须**为同一条QUIC连接上的连续查询，以符合QUIC传输规范《[RFC9000]()》的形式，接连选择下一条可由客户端发起的双向流。丢包和其他网络事件可能使得查询乱序抵达。服务器**应该**随时处理刚刚抵达的查询，因为不这么做就会引入不必要的延迟。

The client MUST send the DNS query over the selected stream and MUST indicate through the STREAM FIN mechanism that no further data will be sent on that stream.

客户端**必须**在所选的流上发送DNS查询，且**必须**通过流的FIN置位机制来表明它不会再在流上发送更多数据。

The server MUST send the response(s) on the same stream and MUST indicate, after the last response, through the STREAM FIN mechanism that no further data will be sent on that stream.

服务器**必须**在同一条流上发送响应，且**必须**在发送完最后一条响应后通过流的FIN置位机制来表明它不会再在流上发送更多数据。

Therefore, a single DNS transaction consumes a single bidirectional client-initiated stream. This means that the client's first query occurs on QUIC stream 0, the second on 4, and so on (see Section 2.1 of [RFC9000]).

因此，一次DNS事务会消耗掉一条由客户端发起的双向流。这意味着客户端的首条查询会出现在QUIC流`0`上，第二条则在流`4`，以此类推（详见《[RFC9000]()》的[第2.1章]()）。

Servers MAY defer processing of a query until the STREAM FIN has been indicated on the stream selected by the client.

服务器**可以**推迟处理查询，直到客户端在所选的流上进行了流的FIN置位。

Servers and clients MAY monitor the number of "dangling" streams. These are open streams where the following events have not occurred after implementation-defined timeouts:

服务器和客户端**可以**监视“悬而未决”的流数量。这类流指的是到实现指定的超时时间为止出现了以下事件的开放流：

* the expected queries or responses have not been received or,

* 未接收到期望的查询或响应，或

* the expected queries or responses have been received but not the STREAM FIN

* 接收到了期望的查询或响应，但缺少流的FIN置位。

Implementations MAY impose a limit on the number of such dangling streams. If limits are encountered, implementations MAY close the connection.

实现**可以**对此类“悬而未决”的流数量设置上限。如果数量到达了上限，那么实现**可以**关闭连接。
