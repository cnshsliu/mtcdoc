# 通过 WebAPI 接收流程流转信息

MetatoCome 支持通过调用 WebAPI 向客户系统提供流程流程信息。
流转信息包括 User 和 Server 两种模式。
User 模式，只在标准 MTC SaaS 中，在用户完成一项工作时，发起提交。支持内网地址
Server 模式，在流程流转的每个节点，包括后台逻辑节点，从 MTC 服务端发送，不支持内网地址，除非内网可以从互联网访问到。

流程设计者，在设计流程时，在流程设计器的菜单中的流程属性中，指定接收流程流转信息的 WebAPI endpoint， 必须是 https 安全协议。

同时，可以指定,仅发送 User 模式，Server 模式，还是两者都发送，如下图所示

![设置流程模版属性](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/tplprop.png)

# 与企业微信 WeCom 继承

# 通过 MTC PaaS API 继成 MTC 功能

开发者可以使用任意现代计算机语言，来调用 MetatoCome 的所有功能。
MetaToCome 提供 WebAPI， 理论上，无论开发 Web 应用，Android/IOS APP，还是桌面应用，都可以通过调用 MetaToCome WebAPI，在应用中集成完整的 HyperAutomation 能力

详见 MetaToCome API

# 通过 MTC PaaS SDK 继成 MTC 功能

MetatoCome 以 NPM 包形式提供 Node.js 开发调用，NPM SDK 包功能与 PaaS API 相同。
后续会持续推出其它语言的 SDK

详见 MetaToCome Node.js SDK
