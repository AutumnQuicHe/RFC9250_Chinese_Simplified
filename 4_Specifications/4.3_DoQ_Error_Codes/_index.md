---
title: "4.3. DoQ错误码"
anchor: "4.3_DoQ_Error_Codes"
weight: 430
rank: "h2"
---

以下错误码被定义出来，它们可以被用于对流突然发起终止的情形，可以在中止对流的读取时被用作应用协议错误码，也可以被用于立即关闭连接的情形。

DOQ_NO_ERROR（无错误，值为`0x0`）：

:   没有错误发生。
它被用于需要关闭连接或流，但没有错误可以报告的情形。

DOQ_INTERNAL_ERROR（内部错误，值为`0x1`）：

:   DoQ实现遇到了内部错误，并且它无法继续完成事务或维持连接。

DOQ_PROTOCOL_ERROR（协议错误，值为`0x2`）：

:   DoQ实现遇到了协议错误，并且它正在强行中止连接。

DOQ_REQUEST_CANCELLED（请求被取消，值为`0x3`）：

:   DoQ客户端使用该错误码来表明它想要取消一项正在进行的事务。

DOQ_EXCESSIVE_LOAD（负载过量，值为`0x4`）：

:   DoQ实现使用该错误码来表明它正因过量的负载而关闭连接。

DOQ_UNSPECIFIED_ERROR（未指定的错误，值为`0x5`）：

:   在找不到更准确的错误码时，DoQ实现会使用该错误码。

DOQ_ERROR_RESERVED（值为`0xd098ea5e`）：

:   用于测试的备用错误码。

有关注册新错误码的细节，详见[第8.4章](#8.4_DNS-over-QUIC_Error_Codes_Registry)。
