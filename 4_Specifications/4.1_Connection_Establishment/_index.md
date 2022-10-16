---
title: "4.1. 连接的建立"
anchor: "4.1_Connection_Establishment"
weight: 410
rank: "h2"
---

DoQ connections are established as described in the QUIC transport specification [RFC9000]. During connection establishment, DoQ support is indicated by selecting the Application-Layer Protocol Negotiation (ALPN) token "doq" in the crypto handshake.

DoQ连接是按照QUIC传输规范《[RFC9000]()》来建立的。
在连接建立期间，通过在加密握手中选择应用层协议协商（ALPN）词汇`doq`来表明对DoQ的支持。
