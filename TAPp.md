# 可信业余协议——分组子协议 Trusted Amateur Protocol packet-Subprotocol (TAPp)
`TAPp`是用于直接交换TAP数据和其他数据的子协议.

`TAPp`是一种分组无线网协议，提供了在不同站点之间有效交换数据的方法。一次标准的`TAPp通信`应包含`TX``RX`双方，且均能够收发数据。`TX``RX`仅作为`链路发起者``链路确认者`的代称。

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

用户在收到包后检查TLV`0x2C`有无自己的地址，如果有，则认为自己是目的地址，挑选出来并强调显示。一个用户可识别多个地址，如`CQ,ALL,CN,SOS,AEP_TEAM`。`DX`地址应判断自己是否属于DX，即`自己地区`不等于`源地址地区`。

如果需要中继，应在目的地址后加中继地址，最后加TLV`0x2E`，标注最大转发次数。可加入多个中继地址，转发次数应是中继数。

#### 特殊中继地址
| NULL | ANY,ALL | ANYONE | MAILREP | USER:{user_address} |
|:----:|:----:|:----:|:----:|:----:|
| 无目的中继 | 所有中继 | 所有人和中继 | 支持电子邮件的人和中继 | 通过用户转发 |

其中的`MAILREP`在使用时源地址应包含发信人电子邮箱地址，目的地址包含收信人电子邮箱地址。在转发时应当由转发者自己的电子邮箱发送，主题为`AMP MailRepeat.From {Sender_Address}:{Sender_Email_Address},To {Addressee_Email_Address}`，内容为整个原始帧的base64编码。

源地址和目的地址若为电子邮箱地址，应添加前缀`F2`以区分。

`{Sender_Address}`为发信人呼号，`{Addressee_Address}`为收信人呼号；`{Sender_Email_Address}`为发信人邮箱地址，`{Addressee_Email_Address}`为收信人邮箱地址。

通过无线电传输电子邮件时应将邮件正文base64解密后的数据作为数据帧主体，将发信邮箱地址和自己中继地址（前缀`0xF4`）写入源地址中，将主题`AMP MailRepeater.From {Sender_Address},To {Addressee_Address}:{Addressee_Email_Address}`中的`{Sender_Address}`添加进源地址中,`{Addressee_Email_Address}`和`{Addressee_Address}`加入目的地址中。

中继和用户在收到帧后检查TLV`0x2D`有无自己的地址，如果有，则将TLV`0x2E`的值减1，在自己中继地址的标签前加上前缀`0xF4`，在自己中继地址值的末尾加上`0x5E ^`，后附TLV`0x2E`的十进制UTF-8值（如原始包`0x2D0170 "p" 2E0102`，处理后为`0xF42D03705E31 "p^1" 0x2E0101`），重新校验后转发；若TLV`0x2E`的原始值为0，则不转发。

### 帧类型
(如无特殊说明，数字默认16进制)

| 类型号 | 说明 | 类型号 | 说明 | 类型号 | 说明 | 类型号 | 说明 |
|:---:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|
| 00 | Null(NUL)空帧 | 40 |  | 80 |  | C0 |  |
| 01 | Calling(CL)呼叫 | 41 |  | 81 |  | C1 |  |
| 02 | Heared(HR)到达确认 | 42 |  | 82 |  | C2 |  |
| 03 | BusyWait(BW)忙，稍后 | 43 |  | 83 |  | C3 |  |
| 04 | BusyStop(BS)忙，结束 | 44 |  | 84 |  | C4 |  |
| 05 | LinkSetup(LS)请求建立链路 | 45 |  | 85 |  | C5 |  |
| 06 | LinkAccept(LA)同意建立链路 | 46 |  | 86 |  | C6 |  |
| 07 | LinkOver(LO)链路正常结束 | 47 |  | 87 |  | C7 |  |
| 08 | LinkStop(LP)链路被迫结束 | 48 |  | 88 |  | C8 |  |
| 09 | Linkless(LL)请使用非链路连接 | 49 |  | 89 |  | C9 |  |
| 0A | LinkRefused(LRE)拒绝链路 | 4A |  | 8A |  | CA |  |
| 0B | Non-linkCalling(NCL)非链路呼叫 | 4B |  | 8B |  | CB |  |
| 0C | Non-linkReply(NR)非链路应答 | 4C |  | 8C |  | CC |  |
| 0D | Non-linkRefused(NRE)非链路拒绝 | 4D |  | 8D |  | CD |  |
| 0E | Non-linkOver(NO)非链路正常结束 | 4E |  | 8E |  | CE |  |
| 0F | Non-linkStop(NS)非链路被迫结束 | 4F |  | 8F |  | CF |  |
| 10 | Message(M)未定义的信息帧 | 50 |  | 90 |  | D0 |  |
| 11 | EMessage(EM)加密信息帧 | 51 |  | 91 |  | D1 |  |
| 12 | SigningOnlyMessage(SM)仅签名信息帧 | 52 |  | 92 |  | D2 |  |
| 13 | UnEMessage(UEM)非加密信息帧 | 53 |  | 93 |  | D3 |  |
| 14 | ARQ(ARQ)重发请求 | 54 |  | 94 |  | D4 |  |
| 15 | ACK(ACK)确认 | 55 |  | 95 |  | D5 |  |
| 16 | Ping(PING)延迟测试 | 56 |  | 96 |  | D6 |  |
| 17 | Pong(PONG)延迟测试返回 | 57 |  | 97 |  | D7 |  |
| 18 | TimeRequest(TR)时间请求 | 58 |  | 98 |  | D8 |  |
| 19 | TimeSync(TS)时间同步 | 59 |  | 99 |  | D9 |  |
| 1A |  | 5A |  | 9A |  | DA |  |
| 1B |  | 5B |  | 9B |  | DB |  |
| 1C |  | 5C |  | 9C |  | DC |  |
| 1D |  | 5D |  | 9D |  | DD |  |
| 1E |  | 5E |  | 9E |  | DE |  |
| 1F | EmergencyMessage(EMM)紧急信息帧(非加密可签名) | 5F |  | 9F |  | DF |  |
| 20 | ERequest(ER)加密请求 | 60 |  | A0 |  | E0 |  |
| 21 | EAccept(EA)同意加密 | 61 |  | A1 |  | E1 |  |
| 22 | EDenial(ED)拒绝加密 | 62 |  | A2 |  | E2 |  |
| 23 | EFail(EF)加密失败 | 63 |  | A3 |  | E3 |  |
| 24 | ESuccess(ES)加密成功 | 64 |  | A4 |  | E4 |  |
| 25 | SignRequest(SR)签名请求 | 65 |  | A5 |  | E5 |  |
| 26 | SAccept(SA)同意签名 | 66 |  | A6 |  | E6 |  |
| 27 | SDenial(SD)拒绝签名 | 67 |  | A7 |  | E7 |  |
| 28 | SFail(SF)签名失败 | 68 |  | A8 |  | E8 |  |
| 29 | SSuccess(SS)签名成功 | 69 |  | A9 |  | E9 |  |
| 2A | CertificateRequest(CR)证书请求 | 6A |  | AA |  | EA |  |
| 2B | CAccept(CA)同意证书 | 6B |  | AB |  | EB |  |
| 2C | CDenial(CD)拒绝证书 | 6C |  | AC |  | EC |  |
| 2D | CSigningReq(CSR)证书签名请求 | 6D |  | AD |  | ED |  |
| 2E | CTransmit(CT)证书传送 | 6E |  | AE |  | EE |  |
| 2F |  | 6F |  | AF |  | EF |  |
| 30 |  | 70 |  | B0 |  | F0 |  |
| 31 |  | 71 |  | B1 |  | F1 |  |
| 32 |  | 72 |  | B2 |  | F2 |  |
| 33 |  | 73 |  | B3 |  | F3 |  |
| 34 |  | 74 |  | B4 |  | F4 |  |
| 35 |  | 75 |  | B5 |  | F5 |  |
| 36 |  | 76 |  | B6 |  | F6 |  |
| 37 |  | 77 |  | B7 |  | F7 |  |
| 38 |  | 78 |  | B8 |  | F8 |  |
| 39 |  | 79 |  | B9 |  | F9 |  |
| 3A |  | 7A |  | BA |  | FA |  |
| 3B |  | 7B |  | BB |  | FB |  |
| 3C |  | 7C |  | BC |  | FC |  |
| 3D |  | 7D |  | BD |  | FD |  |
| 3E |  | 7E |  | BE |  | FE |  |
| 3F |  | 7F |  | BF |  | FF |  |

### 信息类型
信息类型描述了信息数据所使用的协议。
