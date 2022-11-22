# 业余加密协议 Amateur Encryption Protocol
`Amateur Encryption Protocol` is a protocol of amateur radio communication.It's abbreviated as `AEP`.

`业余加密协议`是用于业余无线电通信的协议，英文缩写为`AEP`

业余加密协议由此存储库所有者制定，此说明文档和协议具体内容均基于所有者母语`zh-hans-CN`撰写，在转译到其他文字和语言时或会出现若干错误，请以`zh-hans-CN`的内容为准。

The Amateur Encryption Protocol is developed by the owner of this repository. This description document and the specific content of the protocol are based on the owner's native language `zh-hans-CN`. Some errors may occur when translating to other written words and languages, whichever is `zh hans-CN`.

# v1.0
此协议有`AEPb` `AEPm` `AEPc`三个通信子协议，分别对应`基本` `信息` `证书`；还有`AEPC`和`ACSR`两个证书子协议，分别是数字证书和证书请求。
## AEPb
`AEPb`子协议全称`业余加密协议——基本子协议`，基本对应Basic。此子协议同时包含数字证书和信息，是其他子协议的初始。在应用中，可组合使用其他子协议而不使用此子协议，但是仍然应当对此子协议提供完全支持。

`AEPb`的详细描述文档见`minexixi/AEP/AEPb.md`

`AEPb` https://github.com/minexixi/AEP/blob/73a5343da97e44512bc51dd97b3ecdc565dcc3c7/AEPb.md
## AEPm
`AEPm`子协议全称`业余加密协议——信息子协议`，信息对应Message。此子协议包含信息和签名，是通信中主要传输的数据。

`AEPb`的详细描述文档见`minexixi/AEP/AEPm.md`

`AEPb` https://github.com/minexixi/AEP/blob/73a5343da97e44512bc51dd97b3ecdc565dcc3c7/AEPm.md
## AEPc
