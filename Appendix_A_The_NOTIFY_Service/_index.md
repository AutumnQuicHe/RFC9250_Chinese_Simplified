---
title: "10. NOTIFY服务"
anchor: "Appendix_A_The_NOTIFY_Service"
weight: 1000
rank: "h1"
---

This appendix discusses why it is considered acceptable to send NOTIFY (see [RFC1996]) in 0-RTT data.

本附录讨论了为何允许在0-RTT数据中发送NOTIFY（详见《[RFC1996]()》）。

Section 4.5 says "The 0-RTT mechanism MUST NOT be used to send DNS requests that are not "replayable" transactions". This specification supports sending a NOTIFY in 0-RTT data because although a NOTIFY technically changes the state of the receiving server, the effect of replaying NOTIFYs has negligible impact in practice.

[第4.5章]()中提到，**必须不**使用0-RTT机制来发送并非“可重放”事务的DNS请求。
本规范支持在0-RTT数据中发送NOTIFY的原因是尽管技术上NOTIFY会改变接收它的服务器的状态，但是重放NOTIFY所能够造成的实际影响微乎其微。

NOTIFY messages prompt a secondary to either send an SOA query or an XFR request to the primary on the basis that a newer version of the zone is available. It has long been recognized that NOTIFYs can be forged and, in theory, used to cause a secondary to send repeated unnecessary requests to the primary. For this reason, most implementations have some form of throttling of the SOA/XFR queries triggered by the receipt of one or more NOTIFYs.

NOTIFY消息向辅服务器发出提醒，使其根据是否有新版本的可用区域来向主服务器发送SOA查询或XFR请求。
NOTIFY可以被伪造，并且理论上可以被用来使辅服务器向主服务器发送重复的冗余请求，这是众所周知的。
基于这个原因，大多数实现都具有某种限制因接收到NOTIFY而触发的SOA/XFR查询的频次的机制。

[RFC9103] describes the privacy risks associated with both NOTIFY and SOA queries and does not include addressing those risks within the scope of encrypting zone transfers. Given this, the privacy benefit of using DoQ for NOTIFY is not clear, but for the same reason, sending NOTIFY as 0-RTT data has no privacy risk above that of sending it using cleartext DNS.

《[RFC9103]()》描述了与NOTIFY和SOA查询有关的隐私方面的风险，且没有涉及如何在加密区域传送场景中消除这些风险。
因此，将DoQ用于NOTIFY的做法在隐私方面的好处并不明确，但是出于相同的理由，将NOTIFY作为0-RTT数据来发送的做法在隐私方面的风险并不会高于使用明文DNS来发送它时的风险。
