# 业余加密协议——证书子协议 Amateur Encryption Protocal certificate-Subprotocol (AEPc)
此子协议专门用于传递数字证书。

This subprotocol is dedicated to the delivery of digital certificates.

采用`TLV`格式描述

## v1.0
### 内容规定
(如无特殊说明，数字默认16进制)

| 名称 | T | L | V | 说明 |
|:-------:|:----:|:---:|:----:|:----:|
| 协议头 | F101  | 02 | 0x4563 Ec|  |
| 版本号 | 06 | 01 | 0x01 | v1 |
| 标志位 | 06 | 01 | 见说明 | 见表`标志位` |
| 压缩方式（若有） | 18 | 不定 | UTF-8 | 应从预设的`压缩方式列表`里选择 |
| 证书类型 | 06 | 01 | 见说明 | 见表`证书类型` |
| 证书 | 26 | 不定 | UTF-8 | base64 |
| 签名 | 2A | 不定 | UTF-8 | base64。应使用`已被信任的证书`来签名 |

对签名的要求请参考`AEP规范:签名规范`