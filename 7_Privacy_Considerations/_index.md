---
title: "7. 关于隐私的考量"
anchor: "7_Privacy_Considerations"
weight: 700
rank: "h1"
---

在《关于DNS隐私的考量》（详见《[RFC9076](https://www.rfc-editor.org/info/rfc9076)》）中提供的对于加密传输通用的考量对DoQ是适用的。
其中提供的几点的考量并不区分DoT还是DoQ，这里不再赘述。
类似地，《有关DNS隐私服务操作符的推荐》（详见《[RFC8932](https://www.rfc-editor.org/info/rfc8932)》，它涵盖了DNS隐私服务的可操作性、策略，及其安全方面的考量）同样适用于DoQ服务。

QUIC内部使用的是TLS 1.3（详见《[RFC8446](https://www.rfc-editor.org/info/rfc8446)》）的机制，这使得QUIC能够传输“0-RTT”数据。
它在延迟方面能提供吸引人的收益，但是它引发了两点担忧：

* 攻击者可以重放0-RTT数据，并从接收到数据的服务器的行为中推断数据内容。

* 0-RTT机制依赖于TLS的会话恢复特性，这使得客户端相继的数次会话被关联起来。

[第7.1章](#7.1_Privacy_Issues_with_0-RTT_data)和[第7.2章](#7.2_Privacy_Issues_with_Session_Resumption)中展开讨论了这些问题。
