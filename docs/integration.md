# 通过 WebAPI 接收流程流转信息

MetatoCome 支持通过调用 WebAPI 向客户系统提供流程流程信息。
流转信息包括 User 和 Server 两种模式。
User 模式，只在标准 MTC SaaS 中，在用户完成一项工作时，发起提交。支持内网地址
Server 模式，在流程流转的每个节点，包括后台逻辑节点，从 MTC 服务端发送，不支持内网地址，除非内网可以从互联网访问到。

流程设计者，在设计流程时，在流程设计器的菜单中的流程属性中，指定接收流程流转信息的 WebAPI endpoint， 必须是 https 安全协议。

同时，可以指定,仅发送 User 模式，Server 模式，还是两者都发送，如下图所示

![设置流程模版属性](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/tplprop.png)

# 与企业微信 WeCom 集成

MetatoCome 目前支持向企业微信（WeCom）发送活动完成信息
因企业微信机器人每天所支持的消息数量有限，所以支持设置多个企业微信机器人，MTC 随机选择一个，通过该机器人转发消息。理论上，依然存在消息超过限制数量的情况，但只要添加的企业微信消息机器人数量够多，一般可以避免发送失败的问题。

管理员需要设置 MTC 列表，来实现与企业微信的集成功能

    1. 在设置中，新建"wecombots_tpl"列表
    2. 在“wecombots_tpl”列表中，添加需要转发企业微信的流程模版名称为列表项
    3. 在该列表项下，添加一到多个“企业微信消息机器人”的ID

MTC 运行时，会从该列表下检查当前流程模版是否存在，如存在，取得机器人 ID 项，并随机选择一个机器人 ID，用于发送消息
![wecombot_tpl](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/wecombot_tpl.png)
上图中，一共添加了 12 个企业微信消息机器人 ID。
（企业微信中，如何添加消息机器人，请查看企业微信官方文档）

另外，流程模版设计者，有机会指定，哪个工作项节点，需要发送企业微信消息，方法是在节点属性中，选择或取消机器人设置
![wecom_checkbox](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/wecom_checkbox.png)
不自动全部发送企业微信的目的，是为了尽量节省有限的企业微信机器人消息发送数量限制。因此，建议，仅在关键的流程节点上，打开发送企业微信消息选项。

**Tips**: 建议通过使用“通过 WebAPI 接收流程流转信息”中介绍的方法，来获得流程流转信息，并自由对流程流转进行处理

# 通过 MTC PaaS API 集成 MTC 功能

开发者可以使用任意现代计算机语言，来调用 MetatoCome 的所有功能。
MetaToCome 提供 WebAPI， 理论上，无论开发 Web 应用，Android/IOS APP，还是桌面应用，都可以通过调用 MetaToCome WebAPI，在应用中集成完整的 HyperAutomation 能力

详见 [MetaToCome API](api.md)

# 通过 MTC PaaS SDK 集成 MTC 功能

MetatoCome 以 NPM 包形式提供 Node.js 开发调用，NPM SDK 包功能与 PaaS API 相同。
后续会持续推出其它语言的 SDK

详见 [MetaToCome Node.js SDK](sdk.md)
