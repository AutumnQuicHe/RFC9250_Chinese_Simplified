---
title: "6. 关于安全性的考量"
anchor: "6_Security_Considerations"
weight: 600
rank: "h1"
---

《[RFC3833](https://www.rfc-editor.org/info/rfc3833)》中可以找到对于域名系统的威胁分析。
这份分析写于DoT、DoH和DoQ的开发之前，可能需要更新。

DoQ在安全性方面的考量应该与DoT（详见《[RFC7858](https://www.rfc-editor.org/info/rfc7858)》）中的考量相当。
《[RFC7858](https://www.rfc-editor.org/info/rfc7858)》中定义的DoT仅讨论了存根至递归场景，但是有关中间人攻击、中间设备，以及来自明文连接的数据缓存的考量同样适用于DoQ的递归至权威场景。
正如[第5.1章](#5.1_Authentication)指出的那样，对于保护使用DoQ的区域传送的认证要求与基于DoT的区域传送的要求一致；因此，安全性上的一般考量与《[RFC9103](https://www.rfc-editor.org/info/rfc9103)》中描述的考量完全类似。

DoQ依赖于QUIC，后者依赖于TLS1.3，且默认支持对《[BCP195](https://www.rfc-editor.org/info/bcp195)》中描述的降级攻击进行抵御。
《[RFC9000](../RFC9000_Chinese_Simplified)》的[第21章](../RFC9000_Chinese_Simplified/#21_Security_Considerations)介绍了特定于QUIC的问题及其抵御方法。
