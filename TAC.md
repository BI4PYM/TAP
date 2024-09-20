# 可信业余证书 Trusted Amateur Certificate (TAC)
此子协议描述了业余数字证书`TAC`的内容和使用规范。

This subprotocol describes the content and usage specifications of Amateur Digital Certificate `TAC'.

采用`TLV`格式描述

## v1.0
### 内容规定 (如无特殊说明，数字默认16进制)
| 名称 | T | L | V | 说明 |
|:-------:|:----:|:---:|:----:|:----:|
| 证书头 | F101  | 02 | 0x4143 AC|  |
| 版本号 | 06 | 01 | 0x01 | v1 |
| 签名时间戳 | F50E | 不定 | 不定 |  |
| 签名算法 | 19 | 不定 | UTF-8 | 应从预设的`签名算法列表`里选择 |
| 用户公钥 | 22 | 不定 | UTF-8 |  |
| 颁发者 | FB10 | 不定 | UTF-8 |  |
| 被颁发者 | FC10 | 不定 | UTF-8 |  |
| 生效时间戳 | F40E | 不定 | 不定 |  |
| 失效时间戳 | F30E | 不定 | 不定 |  |
| 用途 | 01 | 不定 | UTF-8 |  |
| 证书UUID | 20 | 不定 | 不定 |  |
| 颁发者公钥 | 22 | 不定 | UTF-8 | base64 |
| 颁发者签名 | 2A | 不定 | UTF-8 | base64 |

对签名的要求请参考`TAP规范:签名规范`
