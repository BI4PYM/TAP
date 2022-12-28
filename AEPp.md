# 业余加密通信——分组子协议 Amateur Encryption Protocol packet-Subprotocol (AEPp)
`AEPp`是针对于非AX.25协议下直接交换`AEP数据`的子协议，也可用于传输其他数据。

`AEPp`是一种分组无线网协议，提供了在不同站点之间有效的交换数据的方法。一次标准的`AEPp通信`应包含`TX``RX`双方，且均能够收发数据。`TX``RX`仅作为`链路发起者``链路确认者`的代称。

## v1.0
### 帧结构
| 头 | 源地址 | 目的地址 | 帧类型 | 信息类型 | 信息 | 帧UUID | 校验 | 尾 |
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| 0x70 p | T L V | T L V | 2Bytes | 2Bytes | T L V | T L V | T L V | 0x70 p |

### 地址格式
源地址`T`为`0x2B`，`V`为UTF-8编码的发送方呼号或IP地址、UUID编号（不建议）等，在业余无线电通信中应当使用业余无线电呼号。

目的地址`T`为`0x2C`，`V`为UTF-8编码的接收方呼号或IP地址、UUID编号（不建议）等，在业余无线电通信中应当使用业余无线电呼号。

多个源地址或目的地址应单独使用`0x2B`或`0x2C`表示，不得在一个TLV中表示多个地址。
#### 特殊目的地址
| NULL | CQ,ALL | DX | CN,JP,US,EU... | SOS | SELF | {TEAM NAME} |
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| 无目的地址 | 所有人 | 远距,非源地址地区的所有人 | 地区缩写代表地区的所有人 | 紧急呼叫所有人 | 源地址自己 | 自定义组名 |

用户在收到包后检查TLV`0x2C`有无自己的地址，如果有，则认为自己是目的地址，挑选出来并强调显示。一个用户可识别多个地址，如`CQ,ALL,CN,SOS,AEP TEAM`。`DX`地址应判断自己是否属于DX，即`自己地区`不等于`源地址地区`。
如果需要中继，应在目的地址后加中继地址，最后加TLV`0x2E`，标注最大转发次数。可加入多个中继地址，转发次数应是中继数。

#### 特殊中继地址
| NULL | ANY,ALL | ANYONE | MAILREP | USER:{user_address} |
|:----:|:----:|:----:|:----:|:----:|
| 无目的中继 | 所有中继 | 所有人和中继 | 支持电子邮件的人和中继 | 通过用户转发 |

其中的`MAILREP`在使用时源地址应包含发信人电子邮箱地址，目的地址包含收信人电子邮箱地址。在转发时应当由转发者自己的电子邮箱发送，主题为`AEP MailRepeat.From {Sender_Address}:{Sender_Email_Address},To {Addressee_Email_Address}`，内容为整个原始帧的base64编码。

源地址和目的地址若为电子邮箱地址，应添加前缀`F2`以区分。

`{Sender_Address}`为发信人呼号，`{Addressee_Address}`为收信人呼号；`{Sender_Email_Address}`为发信人邮箱地址，`{Addressee_Email_Address}`为收信人邮箱地址。

通过无线电传输电子邮件时应将邮件正文base64解密后的数据作为数据帧主体，将发信邮箱地址和自己中继地址（前缀`0xF4`）写入源地址中，将主题`AEP MailRepeat.From {Sender_Address},To  {Addressee_Address}:{Addressee_Email_Address}`中的`{Sender_Address}`添加进源地址中,`{Addressee_Email_Address}`和`{Addressee_Address}`加入目的地址中。

中继和用户在收到帧后检查TLV`0x2D`有无自己的地址，如果有，则将TLV`0x2E`的值减1，在自己中继地址的标签前加上前缀`0xF4`，在自己中继地址值的末尾加上`0x5E ^`，后附TLV`0x2E`的十进制UTF-8值（如原始包`0x2D0170 "p" 2E0102`，处理后为`0xF42D03705E31 "p^1" 0x2E0101`），重新校验后转发；若TLV`0x2E`的原始值为0，则不转发。