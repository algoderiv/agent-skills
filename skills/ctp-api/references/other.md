# Other

This section contains 325 topics.

## Table of Contents

- 6.7.8_API接口说明 (page 17)
  - 文档版本与重要更新 (page 30)
    - CTP-GetSystemInfo (page 178)
    - CTP-GetDataCollectApiVersion (page 184)
      - GetApiVersion (page 211)
      - GetTradingDay (page 216)
      - Init (page 221)
      - Join (page 227)
      - RegisterFensUserInfo (page 232)
      - RegisterFront (page 238)
      - RegisterNameServer (page 244)
      - Release (page 255)
      - ReqQryMulticastInstrument (page 260)
      - ReqUserLogin (page 266)
      - ReqUserLogout (page 274)
      - SubscribeForQuoteRsp (page 280)
      - SubscribeMarketData (page 286)
      - UnSubscribeForQuoteRsp (page 292)
      - UnSubscribeMarketData (page 298)
      - OnFrontConnected (page 309)
      - OnFrontDisconnected (page 314)
      - OnHeartBeatWarning (page 319)
      - OnRspQryMulticastInstrument (page 329)
      - OnRspSubForQuoteRsp (page 334)
      - OnRspSubMarketData (page 339)
      - OnRspUnSubForQuoteRsp (page 344)
      - OnRspUnSubMarketData (page 349)
      - OnRspUserLogin (page 354)
      - OnRspUserLogout (page 360)
      - OnRtnDepthMarketData (page 365)
      - OnRtnForQuoteRsp (page 373)
      - OnErrRtnBankToFutureByFuture (page 427)
      - OnErrRtnBatchOrderAction (page 435)
      - OnErrRtnCombActionInsert (page 441)
      - OnErrRtnExecOrderAction (page 447)
      - OnErrRtnExecOrderInsert (page 454)
      - OnErrRtnForQuoteInsert (page 460)
      - OnErrRtnFutureToBankByFuture (page 466)
      - OnErrRtnOptionSelfCloseAction (page 474)
      - OnErrRtnOptionSelfCloseInsert (page 481)
      - OnErrRtnOrderAction (page 487)
      - OnErrRtnOrderInsert (page 494)
      - OnErrRtnQueryBankBalanceByFuture (page 501)
      - OnErrRtnQuoteAction (page 509)
      - OnErrRtnQuoteInsert (page 516)
      - OnErrRtnRepealBankToFutureByFutureManual (page 523)
      - OnErrRtnRepealFutureToBankByFutureManual (page 531)
      - OnFrontConnected (page 539)
      - OnFrontDisconnected (page 544)
      - OnHeartBeatWarning (page 549)

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

## 文档版本与重要更新

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
2. 6.6.1P1说明中支持订单应为QuoteSysID
2021/5/10
1. 更新6.6.1P1版本说明
2. 修正ReqQryOptionInstrTradeCost接口
InvestorID字段可作为过滤条件


3. 修正ReqQryOptionInstrTradeCost接口
InputPrice和UnderlyingPrice中文解释错误问题
4. 修正流控说明页在途流控描述不准确问题，原在
未收到响应前，不能发起下一笔查询请求 改为 在
未收完所有的查询响应前，不能发起下一笔查询
请求
2021/3/1
1. 行情CreateFtdcMdApi添加通过tcp、udp、组播
模式连接mdfront的说明
2021/1/25
1. 新增页面6.5.1版本更新说明补充说明
2. 对ReqQryClassifiedInstrument、
ReqQryCombPromotionParam添加说明
2021/1/6
1. 修正6.5.1版本更新说明中运行环境说明，
windows支持32位和64位。
2020/12/24
1. 添加brokerid在认证中是必填字段
2. 6.5.1中ipv6说明RegisterFens-
>RegisterNmaeServer修订
3. 主页添加线程数说明


2020/12/10
1. 调整Tas手续费和保证金说明
2. 修改OnRtnBulletin说明属于公有流
3. 修改OnRtnInstrumentStatus问答
4. 调整ReqQryInstrument中FAQ说明
5. 调整ReqQueryMaxOrderVolume名称为
ReqQryMaxOrderVolume
6. 增加OnFrontDisconnected说明
7. “做市商询价和报价”页面对于是否会撤销前一次
报价说明增加七期说明
8. 大商所组保合约名称修改BUL->BLS BER->BES
9. 主页增加字符集说明
10. 增加Ref字段需要递增说明
11. 对于银期转账中TradeCode说明修改。
12. SubscribePublicTopic增加一种私有流方式
13. “大商所组保”增加FAQ说明
2020/11/6
1. 增加各个接口对ipv6的说明
2. 修订RegisterUserSystemInfo、
SubmitUserSystemInfo接口字段说明


2020/10/21
1. 更新6.5.1apichm说明
2. 新增6.5.1api版本说明
3. 新增两个接口ReqQryClassifiedInstrument、
ReqQryCombPromotionParam对应的回调
OnRspQryClassifiedInstrument、
OnRspQryCombPromotionParam
4. ReqQueryMaxOrderVolume接口修改为
ReqQryMaxOrderVolume，对应的回调
OnRspQueryMaxOrderVolume修改为
OnRspQryMaxOrderVolume
2020/04/29
1. 更新到6.3.19 api说明，更新对应新增字段。
2. 新增tas介绍
3. 新增fens介绍
4. 更正代码示例错误，Release()前使用
RegisterSpi(NULL)，这个代码是错误的，会导致
api概率崩溃。
5. 新增ReqUserLogout、ReqAuthenticate、增加流
控参数部分说明、郑商所行情说明。
2020/03/13


1. 新增init/release线程安全说明
2020/03/06
1. 修正logout之后是否会触发重连机制
2020/02/21
1. 新添加通讯模式中基本接口所属的数据流
2. 完善做市商接受询价的说明
3. 修改对于撤单接口就上期所新增挂单激活操作在
本系统是否支持说明
4. 新增条件单说明
5. 新增各个交易所盘中行情合约状态推送的具体状
态
2019/11/28
1. 新增做市商询价和报价指令介绍
2. 修正SubscribeForQuoteRsp中FAQ回答错误。普
通投资者只能订阅上期所和中金所询价通知 改为
普通投资者能订阅四所的询价通知。
2019/07/12
1. 新增报单回调规则、报价回调规则
2019/07/03


1. 新增相关穿透式的一些问题回答
2019/06/27
1. 新增大商所组保业务说明
2. 修正OnRspQryInvestorPosition和
OnRspQryInvestorPositionDetail里缺失
TimeFirstVolume和PositionCostOffset字段的问题
2019/06/05
1. 新增CTP流控说明
2019/05/27
1. 新增交易接口和行情接口代码示例说明。
2. 新增阅读指引，方便用户快速上手chm文档。
2019/04/29
1. 新增看穿式监管数据采集的常见FAQ。
2019/4/11
1. 新增二代行情API接口说明，该API新增接口
QryMulticastInstrument。
2019/3/14
1. 增加看穿式监管相关API接口的返回值。
2. 主页新增版本兼容性说明，务必了解！


2019/3/14
1. 增加有关看穿式监管相关问题接单。
回到顶部
< 前页 回目录 后页 >




---

## 其他业务规则

其他业务规则
■ 6.7.8_API接口说明
└◆ 其他业务规则
请参阅：
    做市商询价和报价
    报单回调规则
    大商所组保
    报价回调规则
    协议方式接收二代组播行情
    fens连接说明
    各交易所行情区别
    合约状态变化说明
    报单流控、查询流控和会话数控制
    通讯模式
    TAS介绍
    条件单规则
    各交易所特殊指令
    接口中一些重要序号说明
    期货期权的行权、自对冲
    行情流控


    TGate网关接入指南
回到顶部
< 前页 回目录 后页 >




---

