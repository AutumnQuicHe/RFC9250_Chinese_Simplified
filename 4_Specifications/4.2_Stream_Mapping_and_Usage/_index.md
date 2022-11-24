---
title: "4.2. 流的映射与用法"
anchor: "4.2_Stream_Mapping_and_Usage"
weight: 420
rank: "h2"
---

基于QUIC流的DNS流量映射利用了在QUIC传输规范《[RFC9000](../RFC9000_Chinese_Simplified)》的[第2章](../RFC9000_Chinese_Simplified/#2_Streams)中介绍的QUIC流特性。

DNS查询/响应的流量（详见《[RFC1034](https://www.rfc-editor.org/info/rfc1034)》和《[RFC1035](https://www.rfc-editor.org/info/rfc1035)》）遵循着简单的模式：由客户端发送一条查询，然后服务器提供一条或多条响应（多条响应会出现在区域传送场景中）。

本文规定的映射需要客户端为每次查询选择一条单独的QUIC流。
接着服务器使用同一条流来为该查询提供所有响应消息。
为了使得数条响应都能得到解析，会用到一个2字节的长度字段，其用法与在基于TCP的DNS（详见《[RFC1035](https://www.rfc-editor.org/info/rfc1035)》）中定义的2字节字段完全一致。
该操作的作用是使得每条QUIC流的内容与传递单条查询的TCP连接的内容完全一致。

经由DoQ连接发送的所有DNS消息（查询与响应）{{< req_level MUST >}}以一个2字节的长度字段加上在《[RFC1035](https://www.rfc-editor.org/info/rfc1035)》中规定的消息内容的形式构造。

客户端{{< req_level MUST >}}为同一条QUIC连接上的连续查询，以符合QUIC传输规范《[RFC9000](../RFC9000_Chinese_Simplified)》的形式，接连选择下一条可由客户端发起的双向流。
丢包和其他网络事件可能使得查询乱序抵达。
服务器{{< req_level SHOULD >}}随时处理刚刚抵达的查询，因为不这么做就会引入不必要的延迟。

客户端{{< req_level MUST >}}在所选的流上发送DNS查询，且{{< req_level MUST >}}通过流的FIN置位机制来表明它不会再在流上发送更多数据。

服务器{{< req_level MUST >}}在同一条流上发送响应，且{{< req_level MUST >}}在发送完最后一条响应后通过流的FIN置位机制来表明它不会再在流上发送更多数据。

因此，一次DNS事务会消耗掉一条由客户端发起的双向流。
这意味着客户端的首条查询会出现在QUIC流`0`上，第二条则在流`4`，以此类推（详见《[RFC9000](../RFC9000_Chinese_Simplified)》的[第2.1章](../RFC9000_Chinese_Simplified/#2.1_Stream_Types_and_Identifiers)）。

服务器{{< req_level MAY >}}推迟处理查询，直到客户端在所选的流上进行了流的FIN置位。

服务器和客户端{{< req_level MAY >}}监视“悬而未决”的流数量。
这类流指的是到实现指定的超时时间为止出现了以下事件的开放流：

* 未接收到期望的查询或响应，或

* 接收到了期望的查询或响应，但缺少流的FIN置位。

实现{{< req_level MAY >}}对此类“悬而未决”的流数量设置上限。
如果数量到达了上限，那么实现{{< req_level MAY >}}关闭连接。
