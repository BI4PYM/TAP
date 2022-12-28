# 业余加密协议——信息子协议 Amateur Encryption Protocal message-Subprotocol (AEPm)
此子协议包含信息和签名，是通信中主要传输的数据。

This subprotocol contains information and signatures and is the primary data transmission in communication.

采用`TLV`格式描述

## v1.0
### 内容规定 (如无特殊说明，数字默认16进制)
| 名称 | T | L | V | 说明 |
|:-------:|:----:|:---:|:----:|:----:|
| 协议头 | F101  | 02 | 0x4560 Em|  |
| 版本号 | 06 | 01 | 0x01 | v1 |
| 标志位 | 06 | 01 | 见说明 | 见表`标志位` |
| 压缩方式（若有） | 18 | 不定 | UTF-8 | 应从预设的`压缩方式列表`里选择 |
| 加密方式（若有） | 19 | 不定 | UTF-8 | 应与证书登记的`加密方式`相同 |
| 信息 | (F6)(F7)不定 | 不定 | 不定 | 信息内容不定 |
| 签名 | 2A | 不定 | UTF-8 | base64 |

对签名的要求请参考`AEP规范:签名规范`
