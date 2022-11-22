# 业余加密协议——信息子协议 Amateur Encryption Protocal message-Subprotocol (AEPm)
此子协议包含信息和签名，是通信中主要传输的数据。

This subprotocol contains information and signatures and is the primary data transmission in communication.

## v1.0
### 内容规定
| 字节序号 | 位数 | 名称 | 类型 | 描述 |
|:-------:|:----:|:---:|:----:|:----:|
| 0x0000 | 16  | 协议头 | 数字 |0x456D Em|
| 0x0002 | 8     |版本号|数字|协议版本号1~255|
| 0x0003 | 6     |保留  |保留  |保留|
