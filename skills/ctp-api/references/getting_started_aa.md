# Getting Started - CTP 6.7.8 API

本节包含 8 个主题。

## 目录

- [目录](#目录) (第 1 页)
- [6.7.8_API接口说明](#6.7.8_API接口说明) (第 17 页)
  - [主页](#主页) (第 19 页)
  - [阅读指引](#阅读指引) (第 28 页)
  - [看穿式监管数据采集说明](#看穿式监管数据采集说明) (第 156 页)
    - [fens连接说明](#fens连接说明) (第 2339 页)
    - [合约状态变化说明](#合约状态变化说明) (第 2350 页)
    - [接口中一些重要序号说明](#接口中一些重要序号说明) (第 2417 页)

---

## 目录

6.7.8_API接口说明目录
> 6.7.8_API接口说明
　主页
　阅读指引
　文档版本与重要更新
　6.5.1版本更新说明
　6.5.1版本更新说明补充说明
　6.6.1P1版本更新说明
　6.6.5版本更新说明
　6.6.7版本更新说明
　6.6.8版本更新说明
　6.6.9版本更新说明
　6.7.0版本更新说明
　6.7.1版本更新说明
　6.7.2版本更新说明
　6.7.7版本更新说明
　6.7.8版本更新说明
　> 看穿式监管数据采集说明
　　常见FAQ
　　CTP-GetSystemInfo
　　CTP-GetDataCollectApiVersion
　> 行情接口
　　> CThostFtdcMdApi


　　　CreateFtdcMdApi
　　　GetApiVersion
　　　GetTradingDay
　　　Init
　　　Join
　　　RegisterFensUserInfo
　　　RegisterFront
　　　RegisterNameServer
　　　RegisterSpi
　　　Release
　　　ReqQryMulticastInstrument
　　　ReqUserLogin
　　　ReqUserLogout
　　　SubscribeForQuoteRsp
　　　SubscribeMarketData
　　　UnSubscribeForQuoteRsp
　　　UnSubscribeMarketData
　　> CThostFtdcMdSpi
　　　OnFrontConnected
　　　OnFrontDisconnected
　　　OnHeartBeatWarning
　　　OnRspError
　　　OnRspQryMulticastInstrument
　　　OnRspSubForQuoteRsp


　　　OnRspSubMarketData
　　　OnRspUnSubForQuoteRsp
　　　OnRspUnSubMarketData
　　　OnRspUserLogin
　　　OnRspUserLogout
　　　OnRtnDepthMarketData
　　　OnRtnForQuoteRsp
　> 交易接口
　　> CThostFtdcTraderSpi
　　　OnErrRtnBankToFutureByFuture
　　　OnErrRtnBatchOrderAction
　　　OnErrRtnCombActionInsert
　　　OnErrRtnExecOrderAction
　　　OnErrRtnExecOrderInsert
　　　OnErrRtnForQuoteInsert
　　　OnErrRtnFutureToBankByFuture
　　　OnErrRtnOptionSelfCloseAction
　　　OnErrRtnOptionSelfCloseInsert
　　　OnErrRtnOrderAction
　　　OnErrRtnOrderInsert
　　　OnErrRtnQueryBankBalanceByFuture
　　　OnErrRtnQuoteAction
　　　OnErrRtnQuoteInsert
　　　OnErrRtnRepealBankToFutureByFutureManual


　　　OnErrRtnRepealFutureToBankByFutureManual
　　　OnFrontConnected
　　　OnFrontDisconnected
　　　OnHeartBeatWarning
　　　OnRspAuthenticate
　　　OnRspBatchOrderAction
　　　OnRspCombActionInsert
　　　OnRspError
　　　OnRspExecOrderAction
　　　OnRspExecOrderInsert
　　　OnRspForQuoteInsert
　　　OnRspFromBankToFutureByFuture
　　　OnRspFromFutureToBankByFuture
　　　OnRspGenUserCaptcha
　　　OnRspGenUserText
　　　OnRspOptionSelfCloseAction
　　　OnRspOptionSelfCloseInsert
　　　OnRspOrderAction
　　　OnRspOrderInsert
　　　OnRspParkedOrderAction
　　　OnRspParkedOrderInsert
　　　OnRspQryAccountregister
　　　OnRspQryBrokerTradingAlgos
　　　OnRspQryBrokerTradingParams


　　　OnRspQryCFMMCTradingAccountKey
　　　OnRspQryCombAction
　　　OnRspQryCombInstrumentGuard
　　　OnRspQryContractBank
　　　OnRspQryDepthMarketData
　　　OnRspQryEWarrantOffset
　　　OnRspQryExchange
　　　OnRspQryExchangeMarginRate
　　　OnRspQryExchangeMarginRateAdjust
　　　OnRspQryExchangeRate
　　　OnRspQryExecOrder
　　　OnRspQryForQuote
　　　OnRspQryInstrument
　　　OnRspQryInstrumentCommissionRate
　　　OnRspQryInstrumentMarginRate
　　　OnRspQryInstrumentOrderCommRate
　　　OnRspQryInvestor
　　　OnRspQryInvestorPosition
　　　OnRspQryInvestorPositionCombineDetail
　　　OnRspQryInvestorPositionDetail
　　　OnRspQryInvestorProductGroupMargin
　　　OnRspQryInvestUnit
　　　OnRspQryMMInstrumentCommissionRate
　　　OnRspQryMMOptionInstrCommRate


　　　OnRspQryNotice
　　　OnRspQryOptionInstrCommRate
　　　OnRspQryOptionInstrTradeCost
　　　OnRspQryOptionSelfClose
　　　OnRspQryOrder
　　　OnRspQryParkedOrder
　　　OnRspQryParkedOrderAction
　　　OnRspQryProduct
　　　OnRspQryProductExchRate
　　　OnRspQryProductGroup
　　　OnRspQryQuote
　　　OnRspQrySecAgentACIDMap
　　　OnRspQrySecAgentCheckMode
　　　OnRspQrySecAgentTradeInfo
　　　OnRspQrySecAgentTradingAccount
　　　OnRspQrySettlementInfo
　　　OnRspQrySettlementInfoConfirm
　　　OnRspQryTrade
　　　OnRspQryTradingAccount
　　　OnRspQryTradingCode
　　　OnRspQryTradingNotice
　　　OnRspQryTransferBank
　　　OnRspQryTransferSerial
　　　OnRspQueryBankAccountMoneyByFuture


　　　OnRspQueryCFMMCTradingAccountToken
　　　OnRspQuoteAction
　　　OnRspQuoteInsert
　　　OnRspRemoveParkedOrder
　　　OnRspRemoveParkedOrderAction
　　　OnRspSettlementInfoConfirm
　　　OnRspTradingAccountPasswordUpdate
　　　OnRspUserAuthMethod
　　　OnRspUserLogin
　　　OnRspUserLogout
　　　OnRspUserPasswordUpdate
　　　OnRtnBulletin
　　　OnRtnCancelAccountByBank
　　　OnRtnCFMMCTradingAccountToken
　　　OnRtnChangeAccountByBank
　　　OnRtnCombAction
　　　OnRtnErrorConditionalOrder
　　　OnRtnExecOrder
　　　OnRtnForQuoteRsp
　　　OnRtnFromBankToFutureByBank
　　　OnRtnFromBankToFutureByFuture
　　　OnRtnFromFutureToBankByBank
　　　OnRtnFromFutureToBankByFuture
　　　OnRtnInstrumentStatus


　　　OnRtnOpenAccountByBank
　　　OnRtnOptionSelfClose
　　　OnRtnOrder
　　　OnRtnQueryBankBalanceByFuture
　　　OnRtnQuote
　　　OnRtnRepealFromBankToFutureByBank
　　　OnRtnRepealFromBankToFutureByFuture
　　　OnRtnRepealFromBankToFutureByFutureManual
　　　OnRtnRepealFromFutureToBankByBank
　　　OnRtnRepealFromFutureToBankByFuture
　　　OnRtnRepealFromFutureToBankByFutureManual
　　　OnRtnTrade
　　　OnRtnTradingNotice
　　　OnRspQryMaxOrderVolume
　　　OnRspQryClassifiedInstrument
　　　OnRspQryCombPromotionParam
　　　OnRspQryRiskSettleInvstPosition
　　　OnRspQryRiskSettleProductStatus
　　　OnRspQryTraderOffer
　　　OnRspQrySPBMFutureParameter
　　　OnRspQrySPBMOptionParameter
　　　OnRspQrySPBMIntraParameter
　　　OnRspQrySPBMInterParameter
　　　OnRspQrySPBMPortfDefinition


　　　OnRspQrySPBMInvestorPortfDef
　　　OnRspQryInvestorPortfMarginRatio
　　　OnRspQryInvestorProdSPBMDetail
　　　OnRspQryInvestorCommoditySPMMMargin
　　　OnRspQryInvestorCommodityGroupSPMMMargin
　　　OnRspQrySPMMInstParam
　　　OnRspQrySPMMProductParam
　　　OnRspQrySPBMAddOnInterParameter
　　　OnRspQryRCAMSCombProductInfo
　　　OnRspQryRCAMSInstrParameter
　　　OnRspQryRCAMSIntraParameter
　　　OnRspQryRCAMSInterParameter
　　　OnRspQryRCAMSShortOptAdjustParam
　　　OnRspQryRCAMSInvestorCombPosition
　　　OnRspQryInvestorProdRCAMSMargin
　　　OnRspQryRULEInstrParameter
　　　OnRspQryRULEIntraParameter
　　　OnRspQryRULEInterParameter
　　　OnRspQryInvestorProdRULEMargin
　　　OnRspQryInvestorPortfSetting
　　　OnRspQryInvestorInfoCommRec
　　> CThostFtdcTraderApi
　　　CreateFtdcTraderApi
　　　GetApiVersion


　　　GetTradingDay
　　　GetFrontInfo
　　　Init
　　　Join
　　　RegisterFensUserInfo
　　　RegisterFront
　　　RegisterNameServer
　　　RegisterSpi
　　　RegisterUserSystemInfo
　　　Release
　　　ReqAuthenticate
　　　ReqBatchOrderAction
　　　ReqCombActionInsert
　　　ReqExecOrderAction
　　　ReqExecOrderInsert
　　　ReqForQuoteInsert
　　　ReqFromBankToFutureByFuture
　　　ReqFromFutureToBankByFuture
　　　ReqGenUserCaptcha
　　　ReqGenUserText
　　　ReqOptionSelfCloseAction
　　　ReqOptionSelfCloseInsert
　　　ReqOrderAction
　　　ReqOrderInsert


　　　ReqParkedOrderAction
　　　ReqParkedOrderInsert
　　　ReqQryAccountregister
　　　ReqQryBrokerTradingAlgos
　　　ReqQryBrokerTradingParams
　　　ReqQryCFMMCTradingAccountKey
　　　ReqQryCombAction
　　　ReqQryCombInstrumentGuard
　　　ReqQryContractBank
　　　ReqQryDepthMarketData
　　　ReqQryEWarrantOffset
　　　ReqQryExchange
　　　ReqQryExchangeMarginRate
　　　ReqQryExchangeMarginRateAdjust
　　　ReqQryExchangeRate
　　　ReqQryExecOrder
　　　ReqQryForQuote
　　　ReqQryInstrument
　　　ReqQryInstrumentCommissionRate
　　　ReqQryInstrumentMarginRate
　　　ReqQryInstrumentOrderCommRate
　　　ReqQryInvestor
　　　ReqQryInvestorPosition
　　　ReqQryInvestorPositionCombineDetail


　　　ReqQryInvestorPositionDetail
　　　ReqQryInvestorProductGroupMargin
　　　ReqQryInvestUnit
　　　ReqQryMMInstrumentCommissionRate
　　　ReqQryMMOptionInstrCommRate
　　　ReqQryNotice
　　　ReqQryOptionInstrCommRate
　　　ReqQryOptionInstrTradeCost
　　　ReqQryOptionSelfClose
　　　ReqQryOrder
　　　ReqQryParkedOrder
　　　ReqQryParkedOrderAction
　　　ReqQryProduct
　　　ReqQryProductExchRate
　　　ReqQryProductGroup
　　　ReqQryQuote
　　　ReqQrySecAgentACIDMap
　　　ReqQrySecAgentCheckMode
　　　ReqQrySecAgentTradeInfo
　　　ReqQrySecAgentTradingAccount
　　　ReqQrySettlementInfo
　　　ReqQrySettlementInfoConfirm
　　　ReqQryTrade
　　　ReqQryTradingAccount


　　　ReqQryTradingCode
　　　ReqQryTradingNotice
　　　ReqQryTransferBank
　　　ReqQryTransferSerial
　　　ReqQueryBankAccountMoneyByFuture
　　　ReqQueryCFMMCTradingAccountToken
　　　ReqQryMaxOrderVolume
　　　ReqQuoteAction
　　　ReqQuoteInsert
　　　ReqRemoveParkedOrder
　　　ReqRemoveParkedOrderAction
　　　ReqSettlementInfoConfirm
　　　ReqTradingAccountPasswordUpdate
　　　ReqUserAuthMethod
　　　ReqUserLogin
　　　ReqUserLoginWithCaptcha
　　　ReqUserLoginWithOTP
　　　ReqUserLoginWithText
　　　ReqUserLogout
　　　ReqUserPasswordUpdate
　　　SubmitUserSystemInfo
　　　SubscribePrivateTopic
　　　SubscribePublicTopic
　　　ReqQryClassifiedInstrument


　　　ReqQryCombPromotionParam
　　　ReqQryRiskSettleInvstPosition
　　　ReqQryRiskSettleProductStatus
　　　ReqQryTraderOffer
　　　ReqQrySPBMFutureParameter
　　　ReqQrySPBMOptionParameter
　　　ReqQrySPBMIntraParameter
　　　ReqQrySPBM

---

## 6.7.8_API接口说明

6.7.8_API接口说明
请参阅：
    主页
    阅读指引
    文档版本与重要更新
    6.5.1版本更新说明
    6.5.1版本更新说明补充说明
    6.6.1P1版本更新说明
    6.6.5版本更新说明
    6.6.7版本更新说明
    6.6.8版本更新说明
    6.6.9版本更新说明
    6.7.0版本更新说明
    6.7.1版本更新说明
    6.7.2版本更新说明
    6.7.7版本更新说明
    6.7.8版本更新说明
 >> 看穿式监管数据采集说明
 >> 行情接口
 >> 交易接口
 >> 其他业务规则
回到顶部


回目录 后页 >




---

## 主页

主页
CTP-API接口说明


◇ 1.介绍
本接口说明旨在帮助开发者快速查阅综合交易平台API（CTP-
API）的使用方法、参数说明及注意事项。文中汇集了CTP-API使用
过程中常见的问题、重要参数说明及接口调用示例。
回到顶部


◇ 2.版本
本接口文档基于v6.7.8版本。具体更新内容请参照6.7.8版本更新
说明。API使用监控中心分配给上期技术CTP系统的正式密钥进行编
译，只能接入6.5.0版本及以上的front_se（看穿式监管交易前置）。
各终端需要通过期货公司评测后方能接入生产正式交易，评测
时，需要使用集成了监控中心评测密钥的评测版API进行评测，在
官网均有下载。
各版本接入规则如下（更详细的版本前后台说明请参考SFIT官
网API下载页面描述）：
6.3.11API为非看穿式监管版本，只能连接普通front前置
看穿式监管各版本API由于集成了不同的监控中心密钥，所
以：
6.3.16_T1以上的评测版API只能连接评测版front_se前置；
6.3.15以上的生产版API只能连接生产版front_se前置。
API、采集库和前置版本必须遵循以上接入规则，否则无法
连接前置或无法解密采集数据！！
API数据采集相关说明参见看穿式监管数据采集说明
API编译时使用的字符集为GBK。
回到顶部


◇ 3.接口文件列表
文件名
详情
ThostFtdcTraderApi.h
交易接口C++头文件包含交易相关的
指令，如报单
ThostFtdcMdApi.h
交易接口C++头文件包含获取行情相
关的指令。
ThostFtdcUserApiStruct.h
包含了所有用到的数据结构的头文
件。
ThostFtdcUserApiDataType.h 包含了所有用到的数据类型的头文
件。
DataCollect.h
穿透式监管采集模块头文件
thosttraderapi.dll
交易部分的动态链接库和静态链接
库。
thosttraderapi.lib
thostmduserapi.dll
行情部分的动态链接库和静态链接
库。
thostmduserapi.lib
WinDataCollect.dll
穿透式监管采集模块的动态链接库和
静态链接库
WinDataCollect.lib
error.dtd
包含所有可能的错误信息。
error.xml
TraderApi使用对象为直连CTP的终端和连接CTP的中继平台
WinDataCollect采集库的使用对象对中继平台下的客户交易终
端，他们只需调用采集库采集信息
回到顶部


◇ 4.命名规则
消息
格式
实例
请求
Req------
ReqUserLogin
响应
OnRsp------
OnRspUserLogin
查询
ReqQry------
ReqQryInstrument
查询请求的响应
OnRspQry------
OnRspQryInstrument
回报
OnRtn------
OnRtnOrder
错误回报
OnErrRtn------
OnErrRtnOrderInsert
回到顶部


◇ 5.官方网站
开发者可以通过上期技术官网http://www.sfit.com.cn/下载API接
口及说明文档。
回到顶部


◇ 6.模拟账号
开发者可以向上期技术SIMNOW官网http://www.simnow.com.cn/
申请账号并接入测试；也可以向期货公司申请仿真账号，并接入期
货公司的仿真环境测试。
回到顶部


◇ 7.综合交易平台API技术服务群
QQ群：615403410 （需通过期货公司向上期技术提交入群申
请）
回到顶部


◇ 8.更多
更多信息请关注上期技术官方微信公众号。如有意见或建议请
联系CTP-API技术支持
回到顶部
< 前页 回目录 后页 >




---

## 阅读指引

阅读指引
■ 6.7.8_API接口说明
└◆ 阅读指引
在使用CTP的API前，请务必先了解看穿式监管数据采集说明，因
为6月份就开始正式启用看穿式监管，届时只有满足监管要求的客户端
才能登录CTP系统做交易。
详见：
看穿式监管数据采集说明
常见FAQ
通过上文我们应该了解了自己要开发的客户端属于哪种模式，并
且已经按照该模式成功登录进CTP系统，下面我们就可以正式开始使
用CTP的API接口提供的各种功能了。以下两个文档提供了详细代码示
例和说明，能够帮助我们快速上手。
详见：
交易接口
行情接口
报单和报价涉及到诸多回调函数，如何理清这些回调函数的顺序
关系通常是初学者最为困扰的事情。这里例举了一些常用的报单操作
所对应的回调顺序，能够帮助我们极大的减轻测试工作。
详见：
报单回调顺序
报价回调规则


如果想要了解做市商相关功能，请参考：
做市商询价和报价
如果我们所在的期货公司提供了转发的上期所组播行情，通常这
种行情的速度要比CTP普通行情要快，要想使用这种行情，我们可以
参考二代行情接口说明。
详见：
二代行情接口
CTP的接口提供了很多丰富的功能，大部分的接口都是易于理解
和使用的，但是仍然会有部分接口有其特殊的规则，例如手续费率查
询接口并不能一次性返回所有合约的手续费率。如果想要准确正确地
使用API，并将其投入到生产中，请务必了解以下这些特殊规则（本
文档会持续更新补充）。
详见：
报单流控、查询流控和会话数控制
大商所组保
更多内容可以通过左侧菜单树的其他业务规则找到。
回到顶部
< 前页 回目录 后页 >


文档版本与重要更新
描述主要和重要更新内容。


◇ 1.版本
v2024.12.4
回到顶部


◇ 2.更新历史
2024/12/4
1. 调整ReqExecOrderAction、ReqQuoteAction、
ReqOptionSelfCloseAction、
ReqBatchOrderAction中InvestorID字段为必填项
2024/11/30
1. 调整ReqQryDepthMarketData字段ProductClass
（产品类型）为可以作为过滤字段
2. ReqQryInvestorInfoCommRec添加接口说明
2024/11/22
1. 新增TGate网关接入指南
2. 更新版本6.7.8，具体说明见6.7.8版本更新说明
3. 新增接口ReqQryInvestorInfoCommRec、
OnRspQryInvestorInfoCommRec
4. 调整GetFrontInfo字段说明（查询频率->查询流
控、FTD频率->FTD流控）
5. 增加OnRspUserLogin返回字段内容
LoginDRIdentityID（当前登录中心号）、
UserDRIdentityID（用户所属中心号）
2024/10/17


1. 调整ReqOrderInsert中CombOffsetFlag的字段说
明
2024/9/6
1. 更新版本6.7.7，具体说明见6.7.7版本更新说明
2. 接口OnRspOrderInsert、
OnErrRtnOrderInsert、ReqOrderInsert、
OnRspQryOrder、OnRtnOrder、
OnRspOrderAction、ReqOrderAction、
OnErrRtnOrderAction、OnRspQuoteInsert、
OnErrRtnQuoteInsert、ReqQuoteInsert、
OnRspQuoteAction、ReqQuoteAction、
OnRspQryQuote、OnRtnQuote、
OnErrRtnQuoteAction新增OrderMemo（报单回
显字段）、SessionReqSeq（session上请求计数
api自动维护）字段
3. 接口ReqQryDepthMarketData新增ProductClass
（产品类型）字段
4. 接口ReqQryInvestorPortfMarginRatio、
OnRspQryInvestorPortfMarginRatio新增
ProductGroupID（产品群代码）字段
5. 新增接口GetFrontInfo、
ReqQryInvestorPortfSetting、
OnRspQryInvestorPortfSetting
6. 接口OnRspQryInvestorProdRCAMSMargin中
CloseCombFrozenMargin、CloseFrozenMargin


注释调整
2024/9/5
1. 调整各交易所行情区别说明，郑商所七期行情不
再支持4Tick/秒，仅有2Tick/秒
2024/8/16
1. 调整SubscribePrivateTopic说明，不订阅也会按
照restart模式运行。
2024/7/9
1. 条件单规则中增加字段说明和5个使用场景实例。
2. 由于中金所新增期货期权，在期货期权的行权、
自对冲中更新对中金所行权的描述。
2024/6/12
1. ReqQryInstrumentCommissionRate中新增接口
返回结果faq，看结果是交易所的还是投资者手续
费率
2024/6/4
1. ReqQryInvestorPositionCombineDetail添加关于
rule组保开启后查不到持仓的说明。
2. OnRtnTrade增加faq关于成交价来源的说明
2024/5/30


1. 做市商询价和报价中关于中金所是否支持撤销单
边衍生单调整为不支持。
2. 新增页面行情流控
2024/5/20
1. RegisterUserSystemInfo、SubmitUserSystemInfo
中添加-7报错
2024/4/18
1. OnRspQryInvestorPositionDetail调整字段
PositionProfitByTrade关于大商所rule的说明。
2024/4/8
1. ReqParkedOrderInsert中添加faq后台版本672开
始不支持报入预埋条件单、预埋预埋单。
2024/3/28
1. OnRspQryOrder增加OrderSource、
ActiveTraderID、ActiveUserID的说明
2. OnRtnOrder补充OrderSource、
ActiveTraderID、ActiveUserID的说明
2024/3/15
1. OnFrontDisconnected调整断线返回代码注解
2024/3/6


1. CreateFtdcTraderApi添加faq。
2024/2/20
1. OnRspQryDepthMarketData修订成交金额
turnover的说明。
2. OnRspQryInvestorPosition修订持仓成本和持仓
成本差值的说明
3. OnRspQryRiskSettleInvstPosition修订持仓成本
差值的说明
4. 大商所组保修订持仓成本和持仓成本差值的说明
5. OnRspQryInvestorPositionDetail中添加volume字
段说明
6. OnRtnOrder和OnRspQryOrder修订撤销时间
CancelTime的字段说明
2024/1/9
1. 行情和交易的登录接口IPAddress字段说明调整
2. 增加OnRspQryDepthMarketData中Turnover的
说明
2024/1/2
1. 增加6.7.2版本更新说明中关于心跳机制的调整说
明
2023/11/7


1. 更新期货期权的行权、自对冲中撤销自动行权和
自对冲指令的方式，由于6.7.2支持撤销其他席位
发起的“取消到期自动行权”指令或“期权自对
冲”指令
2023/10/28
1. 更新版本6.7.2，具体说明见6.7.2版本更新说明
2. 接口OnRspQrySPBMFutureParameter新增
AddOnLockRateX2（期货合约内部对锁仓附加
费率折扣比例）字段
3. 接口OnRspQrySPBMIntraParameter新增
AddOnIntraRateY2（品种内合约间对锁仓附加
费率折扣比例）字段
4. 新增接口ReqQrySPBMAddOnInterParameter、
ReqQryRCAMSCombProductInfo、
ReqQryRCAMSInstrParameter、
ReqQryRCAMSIntraParameter、
ReqQryRCAMSInterParameter、
ReqQryRCAMSShortOptAdjustParam、
ReqQryRCAMSInvestorCombPosition、
ReqQryInvestorProdRCAMSMargin、
ReqQryRULEInstrParameter、
ReqQryRULEIntraParameter、
ReqQryRULEInterParameter、
ReqQryInvestorProdRULEMargin、
OnRspQrySPBMAddOnInterParameter、
OnRspQryRCAMSCombProductInfo、


OnRspQryRCAMSInstrParameter、
OnRspQryRCAMSIntraParameter、
OnRspQryRCAMSInterParameter、
OnRspQryRCAMSShortOptAdjustParam、
OnRspQryRCAMSInvestorCombPosition、
OnRspQryInvestorProdRCAMSMargin、
OnRspQryRULEInstrParameter、
OnRspQryRULEIntraParameter、
OnRspQryRULEInterParameter、
OnRspQryInvestorProdRULEMargin
2023/10/17
1. 增加[OnRspQryInvestorPositionDetail]和
[OnRspQryInvestorPosition]中SettlementPrice字
段的说明
2023/10/11
1. 修订ReqQryOptionInstrCommRate中的示例
2023/9/26
1. 更新版本6.7.1，具体说明见6.7.1版本更新说明
2. 新增ReqQryInvestorCommoditySPMMMargin、
ReqQryInvestorCommodityGroupSPMMMargin
、ReqQrySPMMInstParam、
ReqQrySPMMProductParam、
OnRspQryInvestorCommoditySPMMMargin、
OnRspQryInvestorCommodityGroupSPMMMar


gin、OnRspQrySPMMInstParam、
OnRspQrySPMMProductParam
3. OnRspQuoteInsert、OnErrRtnQuoteInsert、
ReqQuoteInsert、OnRspQryQuote、
OnRtnQuote接口结构体中增加TimeCondition
（有效期类型）字段
2023/9/8
1. 在合约状态变化说明中修改了INE交易所中的合
约状态
2. 各交易所特殊指令中添加大商所本节有效指令
2023/9/5
1. 在ReqQueryBankAccountMoneyByFuture增加个
别字段必填说明。
2. 在ReqExecOrderInsert中增加faq，各所的行权支
持情况
2023/8/10
1. 增加合约状态变化说明中郑商所合约状态是根据5
期环境整理的说明
2. 在ReqOrderInsert新增关于投机套保标志的faq
3. 在ReqOrderAction增加关于未知单撤单的faq
4. 增加ReqOrderInsert关于请求返回的faq


5. 增加OnRspQryInvestorPosition中关于优惠保证
金收取规则说明
6. 在ReqOrderInsert增加CombHedgeFlag的字段说
明郑商所只能填写投机标志位
7. 在OnRspQryInstrument中添加
LongMarginRatio、ShortMarginRatio字段说明
8. 在OnRtnTradingNotice添加说明
9. 在OnRspQryInstrument和
OnRspQryInvestorPosition添加 PositionDateType
和PositionDate字段说明
10. 在OnRtnOrder添加ZCETotalTradedVolume字段
说明
11. 在OnRspQryExchangeMarginRateAdjust添加faq
12. 在OnRspQryInvestorPosition增加OpenAmount
和OpenCost的字段说明
13. 在SubscribeMarketData添加订阅合约faq
14. 在TAS介绍中增加升贴水介绍
2023/7/17
1. 增加ReqExecOrderInsert中第二条FAQ
2. 更新到目前为止合约状态变化说明
2023/6/2


1. 调整OnRtnOrder中OrderSubmitStatus字段说明
2023/5/22
1. 更新版本6.7.0，具体说明见6.7.0版本更新说明
2. 更新主页说明
2023/5/16
1. 在各交易所行情区别添加TrunOver、
AveragePrice说明，缺少合约乘数需自行添加至
公式中。
2023/3/9
1. 添加ReqOrderInsert中FAQ对于郑商所报单的说
明
2023/3/6
1. 修改所有的TradeApi为TraderApi
2022/12/2
1. 更新深度行情中OpenInterest的说明
OnRtnDepthMarketData
2022/11/8
1. 更新版本6.6.9，具体说明见6.6.9版本更新说明


2. 新增ReqQryInvestorProdSPBMDetail、
ReqQryInvestorPortfMarginRatio、
ReqQrySPBMInvestorPortfDef、
ReqQrySPBMPortfDefinition、
ReqQrySPBMInterParameter、
ReqQrySPBMIntraParameter、
ReqQrySPBMOptionParameter、
ReqQrySPBMFutureParameter
3. 新增对应回调
OnRspQrySPBMFutureParameter、
OnRspQrySPBMOptionParameter、
OnRspQrySPBMIntraParameter、
OnRspQrySPBMInterParameter、
OnRspQrySPBMPortfDefinition、
OnRspQrySPBMInvestorPortfDef、
OnRspQryInvestorPortfMarginRatio、
OnRspQryInvestorProdSPBMDetail
2022/8/25
1. 修订期货期权的行权、自对冲中的说明
2. 删除大商所组保中TimeFirstVolume字段是大商所
专用的说明
3. 修订合约状态变化说明
2022/8/23
1. 更新版本至6.6.8，具体说明见6.6.8版本更新说明


2. OnRspUserLogin接口结构体有新增广期所时间字
段
3. 新增加页面期货期权的行权、自对冲规则
2022/6/20
1. 新增接口中一些重要序号说明
2. 更新版本至6.6.7，具体说明见6.6.7版本更新说明
3. OnRspQryInvestor和OnRspQryProduct接口结构
体有新增字段
4. 看穿式监管数据采集说明中新增CTP-
GetDataCollectApiVersion
5. 更新CTP-GetSystemInfo中返回值的说明
2022/6/8
1. 在页面OnRspQryTradingAccount中添加
ReserveBalance和Reserve的说明
2022/4/18
1. 在OnRtnOrder中新增faq
2022/4/15
1. 在接口OnRspQryOrder、OnRspQryTrade、
OnRtnOrder、OnRtnTrade中新增insertdate和
tradedate的faq


2022/4/14
1. 在OnRspQryTradingAccount和
OnRspQrySecAgentTradingAccount中增加已废
弃的字段描述
2022/3/8
1. 修改OnRspQryInstrument中
MinMarketOrderVolume字段说明
2022/3/2
1. 修改做市商询价和报价中“是否会撤销前一次报
价”行“中金所”为会自动撤销
2022/3/1
1. 在ReqCombActionInsert中增加示例说明。
2022/2/23
1. 更新版本至6.6.5
2. 新增加接口OnRspQryTraderOffer和
ReqQryTraderOffer
3. 修订各交易所特殊指令，删除普通报单说明
4. 修正ReqQryAccountregister接口中描述错误，
原“填写正确查询无返回”，实际只要签约成功后
就能返回。


5. 更新OrderInsert中TimeCondition的字段说明
6. 新增ReqQryOptionInstrCommRate的faq
7. 更新OnRspQryInstrument中最大最小下单量说
明
8. 更新ReqQryBrokerTradingParams中查询方法
9. 新增ReqParkedOrderInsert接口关于组合合约预
埋单说明
10. 在报单流控说明中新增分用户会话数设置的说明
11. 修改ReqOptionSelfCloseInsert的Hedgeflag说明
改成必填
2021/11/3
1. 修订ReqQryInvestorProductGroupMargin中字段
注释
2. 增加ReqAuthenticate中faq报错说明
2021/8/30
1. 在各交易所特殊指令中金所最优价转限价，五档
最优价转限价指令
2. 在常见FAQ中新增28点问答
2021/8/13


1. 新增看穿式监管数据采集说明中穿透式权限要求
说明
2021/7/15
1. 新增行情登录接口ReqUserLogin的行情登录验证
说明
2021/7/13
1. 新增页面“各家交易所特殊指令”
2. 新增tas介绍页面中对持仓的说明
3. 新增做市商询价和报价页面中OrderType和
TradeType字段说明
4. 新增OnRspQryInvestorPosition中PositionCost的
说明
2021/5/21
1. 更新做市商说明上期所期货合约其他交易所使用
接口不同
2. 6.6.

---

## 看穿式监管数据采集说明

看穿式监管数据采集说明
■ 6.7.8_API接口说明
└◆ 看穿式监管数据采集说明


◇ 1.定义
术语
术语说明
采集API链接
库
负责采集终端信息的动态链接库(windows版的
WinDataCollect.dll，linux版的LinuxDataCollect.so，
安卓版的InfoCollection_single_release.aar)，只有连
接中继服务器的终端需要调用，直连模式或中继服
务器无需调用
直连类型终
端
直接连接到CTP交易系统的客户交易终端
中继类型终
端
先连接到中继服务器,中继服务器再调用TraderAPI连
接到CTP交易系统的客户交易终端
多对多类型
中继服务器
为每个客户终端，建立CTP API实例，每个用户独占
一个交易API实例的中继服务器
一对多类型
中继服务器
为多个客户终端，建立一个CTP API实例，使用操作
员为多个客户进行交易的中继服务器
AppID
每个期货终端软件需要向期货公司申请自己的
AppID
RelayAppID
中继服务器软件需要向期货公司申请自己的
RelayAppID
AppID和RelayAppID必须符合监控中心的格式要求。见监控
中心官网技术规范。
直连类型终端无需加载采集链接库，登录时会自动采集并上
报终端信息。
中继类型服务器无需加载采集链接库，仅负责接收客户端的
采集信息并上报CTP。


中继类型终端需要加载采集链接库，并将采集到的信息发送
给中继服务器。
回到顶部


◇ 2.说明
期货公司确认终端软件集成了正确的数据采集模块后，为该
AppID的终端软件分配授权码。终端软件需要保护好自己的AppID
和授权码，防止被其他软件盗用。CTP对终端的认证流程如下：
直连终端认证流程
对于直接连接期货公司交易柜台的终端，期货公司确认终端软
件集成了正确的数据采集模块后。给该终端软件（根据AppID）分
配一个授权码。这个授权码和AppID是绑定的，当终端试图登录期
货公司交易软件的时候，交易后台会验证该终端是否持有合法的
AppID和授权码。
中继和中继下属终端的认证流程
对于使用中继服务器连接期货公司交易柜台的终端，期货公司
确认终端软件集成了正确的数据采集模块和确认中继可以正常报送
终端信息后。期货公司给该终端软件（根据AppID）分配一个授权
码，给中继服务器（根据RelayAppID） 分配一个授权码。当终端登
