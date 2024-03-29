---
title: "附录A. NOTIFY服务"
anchor: "Appendix_A_The_NOTIFY_Service"
weight: 1000
rank: "h1"
---

本附录讨论了为何允许在0-RTT数据中发送NOTIFY（详见《[RFC1996](https://www.rfc-editor.org/info/rfc1996)》）。

[第4.5章](#4.5_Session_Resumption_and_0-RTT)中提到，{{< req_level MUST_NOT >}}使用0-RTT机制来发送并非“可重放”事务的DNS请求。
本规范支持在0-RTT数据中发送NOTIFY的原因是尽管技术上NOTIFY会改变接收它的服务器的状态，但是重放NOTIFY所能够造成的实际影响微乎其微。

NOTIFY消息向辅服务器发出提醒，使其根据是否有新版本的可用区域来向主服务器发送SOA查询或XFR请求。
NOTIFY可以被伪造，并且理论上可以被用来使辅服务器向主服务器发送重复的冗余请求，这是众所周知的。
基于这个原因，大多数实现都具有某种机制，从而限制因接收到NOTIFY而触发的SOA/XFR查询频次。

《[RFC9103](https://www.rfc-editor.org/info/rfc9103)》描述了与NOTIFY和SOA查询有关的隐私方面的风险，且没有涉及如何在加密区域传送场景中消除这些风险。
因此，将DoQ用于NOTIFY的做法在隐私方面的好处并不明确，但是出于相同的理由，将NOTIFY作为0-RTT数据来发送的做法在隐私方面的风险并不会高于使用明文DNS来发送它时的风险。
