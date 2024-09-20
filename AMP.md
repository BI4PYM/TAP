# 业余邮件协议 Amateur Mail Protocal (AMP)
`业余邮件协议`是`TAP`体系下用于业余无线电传输电子邮件的通信协议，也可独立于`TAP`存在。

## v1.0
`业余邮件协议`是应用层协议，一封邮件由邮件头和邮件体两部分组成，文字均使用`UTF-8`编码。
### 邮件格式（XML）
```
<mail>
<head>邮件头</head>
<body>邮件体</body>
</mail>
```
传输时应使用base64编码。

### 邮件头
邮件头包括以下内容（星号`*`表示必填）：
| 名称 | 说明 |
|:-------:|:----:|
|Return-Path|邮件的回复地址|
|Received|`Received from A by B from C`（A为发送方，B为接收方，C为收件人地址）是在传递过程中加上的，内容由接收邮件的终端自动填写|
|From|*发件人地址|
|To|*指定收件人地址|
|subject|邮件的主题|
|date|*发送时间戳(ms)，由发送邮件的终端自动填写|
|cc|抄送地址|
|bcc|暗送地址|
例:
```
From:"BA1AA@radio".
To:"BA1HAM@radio","AA@AA.com"
date:1674363665630
```
### 邮件体
邮件体可以为空，可以插入附件，但附件大小不宜过大。

附件包括图片、二进制文件等，在正文中有先后顺序。

插入附件格式：
```
<file id="1"><MIME>type/subtype</MIME><filename>a.txt"</filename><filesize>12</filesize> '以字节为单位，不足一字节补齐一字节
<file_UUID>680ca564-9f60-90b5-c4e0-d3460dba5d51</file_UUID></file>
```
在邮件末尾：
```
<file uuid="680ca564-9f60-90b5-c4e0-d3460dba5d51">
<file_DATA>base64编码的文件数据</file_DATA></file>
```
`MIME类型表`参照https://www.iana.org/assignments/media-types/media-types.xhtml
### 邮箱地址
在使用无线电传输邮件时，邮箱地址为`callsign@radio`
### 协议转换
在AMP转换到SMTP时应当由转发者自己的电子邮箱发送，主题为`AMP MailRepeater.From {Sender_Address}:{Sender_Email_Address},To {Addressee_Email_Address}`，正文为邮件原始数据文本（非base64）。

`{Sender_Address}`为发信人呼号（可选），`{Addressee_Address}`为收信人呼号（可选）；`{Sender_Email_Address}`为发信人邮箱地址（可选），`{Addressee_Email_Address}`为收信人邮箱地址（必选）。

通过SMTP转换到AMP时应将主题`AMP MailRepeat.From {Sender_Address},To {Addressee_Address}:{Addressee_Email_Address}`还有SMTP邮件信息的各项分离开来，添加进AMP邮件中。
