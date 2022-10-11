---
title: "7. 隐私上的考量"
anchor: "7_Privacy_Considerations"
weight: 700
rank: "h1"
---

The general considerations of encrypted transports provided in "DNS Privacy Considerations" [RFC9076] apply to DoQ. The specific considerations provided there do not differ between DoT and DoQ, and they are not discussed further here. Similarly, "Recommendations for DNS Privacy Service Operators" [RFC8932] (which covers operational, policy, and security considerations for DNS privacy services) is also applicable to DoQ services.

QUIC incorporates the mechanisms of TLS 1.3 [RFC8446], and this enables QUIC transmission of "0-RTT" data. This can provide interesting latency gains, but it raises two concerns:

* Adversaries could replay the 0-RTT data and infer its content from the behavior of the receiving server.

* The 0-RTT mechanism relies on TLS session resumption, which can provide linkability between successive client sessions.

These issues are developed in Sections 7.1 and 7.2.
