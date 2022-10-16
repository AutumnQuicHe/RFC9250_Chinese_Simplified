---
title: "6. 关于安全性的考量"
anchor: "6_Security_Considerations"
weight: 600
rank: "h1"
---

A Threat Analysis of the Domain Name System is found in [RFC3833]. This analysis was written before the development of DoT, DoH, and DoQ, and probably needs to be updated.

《[RFC3833]()》中可以找到对于域名系统的威胁分析。这份分析写于DoT、DoH和DoQ的开发之前，可能需要更新。

The security considerations of DoQ should be comparable to those of DoT [RFC7858]. DoT as specified in [RFC7858] only addresses the stub to recursive scenario, but the considerations about person-in-the-middle attacks, middleboxes, and caching of data from cleartext connections also apply for DoQ to the resolver to authoritative server scenario. As stated in Section 5.1, the authentication requirements for securing zone transfer using DoQ are the same as those for zone transfer over DoT; therefore, the general security considerations are entirely analogous to those described in [RFC9103].

DoQ在安全性方面的考量应该与DoT（详见《[RFC7858]()》）中的考量相当。《[RFC7858]()》中定义的DoT仅讨论了存根至递归场景，但是有关中间人攻击、中间设备，以及来自明文连接的数据缓存的考量同样适用于DoQ的递归至权威场景。正如[第5.1章]()指出的那样，对于保护使用DoQ的区域传送的认证要求与基于DoT的区域传送的要求一致；因此，安全性上的一般考量与《[RFC9103]()》中描述的考量完全类似。

DoQ relies on QUIC, which itself relies on TLS 1.3 and thus supports by default the protections against downgrade attacks described in [BCP195]. QUIC-specific issues and their mitigations are described in Section 21 of [RFC9000].

DoQ依赖于QUIC，后者依赖于TLS1.3，且默认支持对《[BCP195]()》中描述的降级攻击进行抵御。《[RFC9000]()》的[第21章]()介绍了特定于QUIC的问题及其抵御方法。
