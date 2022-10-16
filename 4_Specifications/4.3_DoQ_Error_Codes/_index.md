---
title: "4.3. DoQ错误码"
anchor: "4.3_DoQ_Error_Codes"
weight: 430
rank: "h2"
---

The following error codes are defined for use when abruptly terminating streams, for use as application protocol error codes when aborting reading of streams, or for immediately closing connections:

以下错误码被定义出来，它们可以被用于对流突然发起终止的情形，可以在中止对流的读取时被用作应用协议错误码，也可以被用于立即关闭连接的情形。

DOQ_NO_ERROR (0x0):
No error. This is used when the connection or stream needs to be closed, but there is no error to signal.

DOQ_NO_ERROR（无错误，值为`0x0`）：

:   没有错误发生。它被用于需要关闭连接或流，但没有错误可以报告的情形。

DOQ_INTERNAL_ERROR (0x1):
The DoQ implementation encountered an internal error and is incapable of pursuing the transaction or the connection.

DOQ_INTERNAL_ERROR（内部错误，值为`0x1`）：

:   DoQ实现遇到了内部错误，并且它无法继续完成事务或维持连接。

DOQ_PROTOCOL_ERROR (0x2):
The DoQ implementation encountered a protocol error and is forcibly aborting the connection.

DOQ_PROTOCOL_ERROR（协议错误，值为`0x2`）：

:   DoQ实现遇到了协议错误，并且它正在强行中止连接。

DOQ_REQUEST_CANCELLED (0x3):
A DoQ client uses this to signal that it wants to cancel an outstanding transaction.

DOQ_REQUEST_CANCELLED（请求被取消，值为`0x3`）：

:   DoQ客户端使用该错误码来表明它想要取消一项正在进行的事务。

DOQ_EXCESSIVE_LOAD (0x4):
A DoQ implementation uses this to signal when closing a connection due to excessive load.

DOQ_EXCESSIVE_LOAD（负载过量，值为`0x4`）：

:   DoQ实现使用该错误码来表明它正因过量的负载而关闭连接。

DOQ_UNSPECIFIED_ERROR (0x5):
A DoQ implementation uses this in the absence of a more specific error code.

DOQ_UNSPECIFIED_ERROR（未指定的错误，值为`0x5`）：

:   在找不到更准确的错误码时，DoQ实现会使用该错误码。

DOQ_ERROR_RESERVED (0xd098ea5e):
An alternative error code used for tests.

DOQ_ERROR_RESERVED（值为`0xd098ea5e`）：

:   用于测试的备用错误码。

See Section 8.4 for details on registering new error codes.

有关注册新错误码的细节，详见[第8.4章]()。
