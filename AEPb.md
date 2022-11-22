# 业余加密协议——基本子协议 Amateur Encryption Protocal basic-Subprotocol (AEPb)
此子协议同时包含数字证书和信息，是其他子协议的初始。在应用中，可组合使用其他子协议而不使用此子协议，但是仍然应当对此子协议提供完全支持。

This sub-protocol contains both digital certificates and information and is the initial of other sub-protocols. In applications, you can combine other subprotocols without using this subprotocol, but this subprotocol should still be fully supported.
## v1.0
### 内容规定
| 字节序号 | 位数 | 名称 | 类型 | 描述 |
|:-------:|:----:|:---:|:----:|:----:|
| 0x0000 | 16  | 协议头 | 数字 |0x4562 Eb|
| 0x0002 | 8     |版本号|数字|协议版本号1~255|
| 0x0003 | 6     |保留  |保留  |保留|
