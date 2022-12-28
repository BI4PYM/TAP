# 业余加密协议——基本子协议 Amateur Encryption Protocal basic-Subprotocol (AEPb)
此子协议同时包含数字证书和信息，是最基本的子协议。在应用中，可组合使用其他子协议而不使用此子协议，但是仍然应当对此子协议提供完全支持。

This sub-protocol contains both digital certificates and information and is the most basic sub-protocol. In applications, you can combine other subprotocols without using this subprotocol, but this subprotocol should still be fully supported.

采用`TLV`格式描述

## v1.0
### 内容规定
(如无特殊说明，数字默认16进制)

| 名称 | T | L | V | 说明 |
|:-------:|:----:|:---:|:----:|:----:|
| 协议头 | F101  | 02 | 0x4562 Eb|  |
| 版本号 | 06 | 01 | 0x01 | v1 |
| 标志位 | 06 | 01 | 见说明 | 见表`标志位` |
| 压缩方式（若有） | 18 | 不定 | UTF-8 | 应从预设的`压缩方式列表`里选择 |
| 证书类型 | 06 | 01 | 见说明 | 见表`证书类型` |
| 证书 | 26 | 不定 | UTF-8 | base64 |
| 信息 | (F6)(F7)不定 | 不定 | 不定 | 信息内容不定 |
| 签名 | 2A | 不定 | UTF-8 | base64 |

对签名的要求请参考`AEP规范:签名规范`
