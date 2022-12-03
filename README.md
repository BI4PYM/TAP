# 业余加密协议 Amateur Encryption Protocol
`Amateur Encryption Protocol` is a protocol of amateur radio communication.It's abbreviated as `AEP`.

`业余加密协议`是用于业余无线电通信的协议，英文缩写为`AEP`

业余加密协议由此存储库所有者制定，此说明文档和协议具体内容均基于所有者母语`zh-hans-CN`撰写，在转译到其他文字和语言时或会出现若干错误，请以`zh-hans-CN`的内容为准。

The Amateur Encryption Protocol is developed by the owner of this repository. This description document and the specific content of the protocol are based on the owner's native language `zh-hans-CN`. Some errors may occur when translating to other written words and languages, whichever is `zh hans-CN`.

由于业余无线电通信有`除业余卫星遥控以外，不得以任何形式对信息进行加密`的要求，所以本协议的“加密”实际上是`发送方使用私钥对信息进行加密，接收方使用发送方的公钥对信息进行解密`。这样就可以实现身份认证，而不违反有关规定。

# v1.0
此协议有`AEPb` `AEPm` `AEPc` `AEPp`四个通信子协议，分别对应`基本` `信息` `证书` `封包`；还有`AEPC`和`ACSR`两个证书子协议，分别是数字证书和证书请求。

为保证用户使用`AEP`协议时的安全性和通用性，在此规定有关`AEP`的各种标准，作为应用规范，以避免因标准不一造成的冲突。

为防止版本更改过勤，版本分为大版本和小版本两部分，如`v1.0`即第一大版本的初始版本。在应用中只需传输大版本号即可。小版本主要是针对协议规范、标准的增添、删除和修订，对同一大版本兼容，基本不涉及协议的具体定义。

## AEPb
`AEPb`子协议全称`业余加密协议——基本子协议`，基本对应Basic。此子协议同时包含数字证书和信息，是其他子协议的初始。在应用中，可组合使用其他子协议而不使用此子协议，但是仍然应当对此子协议提供完全支持。

`AEPb`的详细描述文档见`minexixi/AEP/AEPb.md`

`AEPb` https://github.com/minexixi/AEP/blob/73a5343da97e44512bc51dd97b3ecdc565dcc3c7/AEPb.md
## AEPm
`AEPm`子协议全称`业余加密协议——信息子协议`，信息对应Message。此子协议包含信息和签名，是通信中主要传输的数据。

`AEPb`的详细描述文档见`minexixi/AEP/AEPm.md`

`AEPb` https://github.com/minexixi/AEP/blob/73a5343da97e44512bc51dd97b3ecdc565dcc3c7/AEPm.md
## AEPc
