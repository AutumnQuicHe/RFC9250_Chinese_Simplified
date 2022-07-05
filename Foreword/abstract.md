---
title: "摘要"
anchor: "Abstract"
weight: 10
rank: "h2"
---

This document describes the use of QUIC to provide transport confidentiality for DNS. The encryption provided by QUIC has similar properties to those provided by TLS, while QUIC transport eliminates the head-of-line blocking issues inherent with TCP and provides more efficient packet-loss recovery than UDP. DNS over QUIC (DoQ) has privacy properties similar to DNS over TLS (DoT) specified in RFC 7858, and latency characteristics similar to classic DNS over UDP. This specification describes the use of DoQ as a general-purpose transport for DNS and includes the use of DoQ for stub to recursive, recursive to authoritative, and zone transfer scenarios.
