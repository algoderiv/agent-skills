# Changelog - CTP 6.7.8 API

本节包含 16 个主题。

## 目录

  - [文档版本与重要更新](#文档版本与重要更新) (第 30 页)
  - [6.5.1版本更新说明](#6.5.1版本更新说明) (第 53 页)
  - [6.5.1版本更新说明补充说明](#6.5.1版本更新说明补充说明) (第 71 页)
  - [6.6.1P1版本更新说明](#6.6.1P1版本更新说明) (第 75 页)
  - [6.6.5版本更新说明](#6.6.5版本更新说明) (第 83 页)
  - [6.6.7版本更新说明](#6.6.7版本更新说明) (第 89 页)
  - [6.6.8版本更新说明](#6.6.8版本更新说明) (第 97 页)
  - [6.6.9版本更新说明](#6.6.9版本更新说明) (第 100 页)
  - [6.7.0版本更新说明](#6.7.0版本更新说明) (第 119 页)
  - [6.7.1版本更新说明](#6.7.1版本更新说明) (第 122 页)
  - [6.7.2版本更新说明](#6.7.2版本更新说明) (第 129 页)
  - [6.7.7版本更新说明](#6.7.7版本更新说明) (第 143 页)
  - [6.7.8版本更新说明](#6.7.8版本更新说明) (第 151 页)
    - [CTP-GetDataCollectApiVersion](#CTP-GetDataCollectApiVersion) (第 184 页)
      - [GetApiVersion](#GetApiVersion) (第 211 页)
      - [GetApiVersion](#GetApiVersion) (第 1463 页)

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
5. 调整ReqQueryM

---

## 6.5.1版本更新说明

6.5.1版本更新说明
版本号：v6.5.1_20200908_traderapi


◇ 1. 兼容性
V6.3.X的API可以访问6.3.X~6.5.X及其以上的CTP后台系统，但
无法支持大商所长合约期权组合，无法支持IPv6；
V6.5.X的API只能访问6.5.X及其以上的CTP后台系统，支持大
商所长合约期权组合，支持IPv6；
注意：6.5.1API引入的新函数只有在接入6.5.1及以上后台的时
候才会有响应，接入6.5.0时无响应。
回到顶部


◇ 2. 运行环境
Linux操作系统版本为Redhat5.8及以上。
Windows为vista及以上的操作系统，支持32位和64位。
只支持vista及以上的操作系统是由于本次更新支持IPv6，涉
及函数只能在vista及以上版本使用。
回到顶部


◇ 3. API变动


◇ 3.1. InstrumentID字段增长
为了支持大商所的超长组合合约，原先的合约字段30个字节
已经不够长，所以需要对合约字段进行扩长。
新增合约代码类型为80字节。
///TFtdcInstrumentIDType是一个合约代码类型
/////////////////////////////////////////////////////////////////////////
typedef char TThostFtdcInstrumentIDType[81];
原来的合约代码类型改为TThostFtdcOldInstrumentIDType，
并且原来的合约代码类型对应的字段改成保留字段且不用填写
（填写系统会忽略）。
/////////////////////////////////////////////////////////////////////////
///TFtdcOldInstrumentIDType是一个合约代码类型
/////////////////////////////////////////////////////////////////////////
typedef char TThostFtdcOldInstrumentIDType[31];
TThostFtdcOldInstrumentIDType   reserve1；
TThostFtdcOldExchangeInstIDType reserve2;
TThostFtdcOldInstrumentIDType   reserve3；


◇ 3.2. IPAddress字段增长
为了支持以后IPv6的ip地址长度，对ip地址进行扩长，新增IP
地址类型为32字节。
/////////////////////////////////////////////////////////////////////////
///TFtdcIPAddressType是一个IP地址类型
/////////////////////////////////////////////////////////////////////////
typedef char TThostFtdcIPAddressType[33];
原来的ip地址类型改为TThostFtdcOldIPAddressType，原来的
ip地址类型对应的字段改成保留字段且不用填写（填写系统会忽
略）。
/////////////////////////////////////////////////////////////////////////
///TFtdcOldIPAddressType是一个IP地址类型
/////////////////////////////////////////////////////////////////////////
typedef char TThostFtdcOldIPAddressType[16];
TThostFtdcOldIPAddressType  reserve2
凡是接口中涉及IPAdress要手动填写的（如ReqOrderInsert）
按照如下规则：
strcpy(address, "127.0.0.1")
strcpy(address, 
"AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH")
ipv6要转成非零压缩地址，即原始地址，同时要去掉冒
号，例如：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
RegisterFront和RegisterNameServer函数按照如下规则填写：


FrontAddress=tcp://172.24.121.51:35001
FrontAddress=tcp6://fe80::20f8:aa9b:7d59:887d:35001


◇ 3.3. 新增查询大商所组合优惠参数接口
大商所的期权对锁组合（OPL），买入期权垂直价差组合
（BLS），卖出期权垂直价差组合（BES），日历价差组合
（CAS）和买入期权期货组合（BFO）的保证金计算公式中需要
乘以保证金优惠比例。
该比例参数由交易所给出，终端如果需要计算如上组合的保
证金，可以通过本接口查询组合合约的组合优惠参数X值，由于
优惠参数目前不区分投机套保，因此查询返回域投保标志
CombHedgeFlag字段默认为投机类型。
保证金计算公式如下：
期权对锁（OPL），保证金收取为X*卖期权保证金
买入期权垂直价差组合（BLS），保证金收取为X*卖期
权保证金
卖出期权垂直价差组合（BES），保证金收取为min(执
行价格之差*交易单位，空头期权保证金)
日历价差组合（CAS），保证金收取为X*卖出期权保证
金
买入期权期货组合（BFO），保证金收取为X*期货保证
金
新增查询大商所组合优惠参数接口如下，合约代码需要填组
合合约代码：


///请求组合优惠比例
virtual int 
ReqQryCombPromotionParam(CThostFtdcQryCombPromotionPara
mField *pQryCombPromotionParam, int nRequestID);
///查询组合优惠比例
struct CThostFtdcQryCombPromotionParamField
{
     ///交易所代码
    TThostFtdcExchangeIDType ExchangeID;
     ///合约代码
    TThostFtdcInstrumentIDType InstrumentID;
};
///请求组合优惠比例响应
virtual void 
OnRspQryCombPromotionParam(CThostFtdcCombPromotionParam
Field *pCombPromotionParam, CThostFtdcRspInfoField *pRspInfo, 
int nRequestID, bool bIsLast) {};
///组合优惠比例
struct CThostFtdcCombPromotionParamField
{
     ///交易所代码
    TThostFtdcExchangeIDType ExchangeID;
     ///合约代码
    TThostFtdcInstrumentIDType InstrumentID;
     ///投机套保标志
    TThostFtdcCombHedgeFlagType CombHedgeFlag;
     ///期权组合保证金比例
    TThostFtdcDiscountRatioType Xparameter;
};


◇ 3.4. 新增组合枚举值
///TFtdcCombinationTypeType是一个组合类型类型
/////////////////////////////////////////////////////////////////////////
    ///期货组合
    #define THOST_FTDC_COMBT_Future '0'
    ///垂直价差BUL
    #define THOST_FTDC_COMBT_BUL '1'
    ///垂直价差BER
    #define THOST_FTDC_COMBT_BER '2'
    ///跨式组合
    #define THOST_FTDC_COMBT_STD '3'
    ///宽跨式组合
    #define THOST_FTDC_COMBT_STG '4'
    ///备兑组合
    #define THOST_FTDC_COMBT_PRT '5'
    ///时间价差组合
    #define THOST_FTDC_COMBT_CAS '6'
    ///期权对锁组合
    #define THOST_FTDC_COMBT_OPL '7'
    ///买备兑组合
    #define THOST_FTDC_COMBT_BFO '8'
    ///买入期权垂直价差组合
    #define THOST_FTDC_COMBT_BLS '9'
    ///卖出期权垂直价差组合
    #define THOST_FTDC_COMBT_BES 'a'
对大商所组合进行申请和拆分时，合约填写规则如下：
期权跨式组合合约申请和拆分（STD m1905-c-2700&m1905-
p-2700）


期权宽跨式组合合约申请和拆分（STG m1905-p-
2400&m1905-c-2700）
备兑组合合约申请和拆分（PRT m1905-c-2400&m1905）
期货跨期组合合约申请和拆分（SP a1903&a1905）
期货跨品种组合合约申请和拆分（SPC c1903&cs1909）
期货对锁组合合约申请和拆分（SP a1903& a1903）
期权对锁组合合约申请和拆分（OPL m1809-P-3150&m1809-
P-3150）
期权买入垂直价差组合申请和拆分（BLS m1809-P-
3150&m1809-P-3100）
期权卖出垂直价差组合申请和拆分（BES m1809-P-
3350&m1809-P-3150）
期权日历价差组合申请和拆分（CAS m1807-P-3150&m1809-
P-3150）
买入期权期货组合申请和拆分（BFO m1809-P-
3150&m1809）


◇ 3.5. 新增公有流可订阅和取消的功能
增加取消订阅公共流的枚举THOST_TERT_NONE，不调用
SubscribePublicTopic时默认从本交易日开始订阅，调用
SubscribePublicTopic则以传参为准，同订阅私有流，在登录之前
调用。
///订阅公共流。
virtual void SubscribePublicTopic(THOST_TE_RESUME_TYPE 
nResumeType) = 0;
///@param nResumeType 公共流重传方式  
///        THOST_TERT_RESTART:从本交易日开始重传
///        THOST_TERT_RESUME:从上次收到的续传
///        THOST_TERT_QUICK:只传送登录后公共流的内容
///        THOST_TERT_NONE:取消订阅公共流
///@remark 该方法要在Init方法前调用。若不调用则不会收到公
共流的数据。


◇ 3.6. 新增查询分类合约的接口
CTP合约信息可分为可交易合约和非交易合约，非交易合约
数据量占比较大。
大商所只可申请组合的合约为非交易合约
新增查询分类合约接口可依据查询请求域交易类型
TradingType字段查询指定合约信息。
///请求查询分类合约：
virtual int 
ReqQryClassifiedInstrument(CThostFtdcQryClassifiedInstrumentFie
ld *pQryClassifiedInstrument, int nRequestID) = 0;
///请求查询分类合约响应：
virtual void 
OnRspQryClassifiedInstrument(CThostFtdcInstrumentField 
*pInstrument, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast)
///查询分类合约
struct CThostFtdcQryClassifiedInstrumentField
{
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///合约在交易所的代码
    TThostFtdcExchangeInstIDType    ExchangeInstID;
    ///产品代码
    TThostFtdcInstrumentIDType  ProductID;
    ///合约交易状态
    TThostFtdcTradingTypeType   TradingType;
    ///合约分类类型


    TThostFtdcClassTypeType ClassType;
};
其中合约类型TradingType的枚举值为：
///所有状态
#define TD_ALL '0'
///交易
#define TD_TRADE '1'
///非交易
#define TD_UNTRADE '2'
分类类型ClassType的枚举值为：
///所有合约
#define INS_ALL '0'
///期货、即期、期转现、Tas、金属指数合约
#define INS_FUTURE '1'
///期货期权、现货期权合约
#define INS_OPTION '2'
///组合合约
#define INS_COMB '3' //对应产品类型字段Productclass 为组合类
型


◇ 3.7. 查询最大报单数量的接口名称修改
查询最大报单数量请求'的接口名字
由'ReqQueryMaxOrderVolume'修改
为'ReqQryMaxOrderVolume'，‘OnRspQueryMaxOrderVolume’修改
为‘OnRspQryMaxOrderVolume’。
该名称修改不影响旧版本（V6.5.1以下版本）API使用，旧版
本API接入6.5.1后台，仍可使用原有名称；CTPV6.5.1后台系统兼
容线上旧版本API接入。
查询最大报单数量请求
virtual int 
ReqQueryMaxOrderVolume(CThostFtdcQueryMaxOrderVolumeFiel
d *pQueryMaxOrderVolume, int nRequestID) = 0;
修改为：
virtual int ReqQryMaxOrderVolume 
(CThostFtdcQryMaxOrderVolumeField *pQryMaxOrderVolume, int 
nRequestID) = 0;
查询最大报单数量响应
virtual void 
OnRspQueryMaxOrderVolume(CThostFtdcQueryMaxOrderVolumeF
ield *pQueryMaxOrderVolume, CThostFtdcRspInfoField *pRspInfo, 
int nRequestID, bool bIsLast) {};
修改为：


virtual void 
OnRspQryMaxOrderVolume(CThostFtdcQryMaxOrderVolumeField 
*pQryMaxOrderVolume, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};


◇ 3.8. 请求查询报价接口逻辑优化
如果查询请求有指定QuoteSysID(填空则返回全部)，后台会
根据QuoteSysID返回相应的报价信息，旧版本后台不支持根据
QuoteSysID进行过滤返回。
///请求查询报价
virtual int ReqQryQuote(CThostFtdcQryQuoteField *pQryQuote, int 
nRequestID) = 0;
回到顶部


◇ 4. 看穿式采集库说明
本次更新没有变动采集库，采集库暂时只采集IPv4地址。
回到顶部
< 前页 回目录 后页 >




---

## 6.5.1版本更新说明补充说明

6.5.1版本更新说明补充说明
版本号：v6.5.1_20200908_traderapi
后台版本：V6.5.2_P3


◇ 1. 兼容性
V6.5.1API连接CTP后台需为V6.5.0及以上版本（如果用
V6.5.1API接入V6.5.0后台，V6.5.1新加接口不支持使用，其他接口
正常使用）；
CTPV6.5.1后台系统兼容线上旧版本API接入（V6.5.1以下版
本）
回到顶部


◇ 2.补充变更说明：（请各终端厂商根据自身
情况进行开发）
1.当终端调用ReqQryInstrument接口查询合约时，返回的
合约信息中不再包含大商所不可交易只能申请组合的合约
信息，减少投资者客户端登录系统时查询合约的数据量。
2.当终端调用ReqQryClassifiedInstrument接口查询合约
时，为了减少查询合约的流量压力，针对大商所可申请组
合的合约信息的返回进行变更如下：
1）大商所可申请组合的合约信息以产品级别返回，且终端在请
求查询时必须填写，ExchangeID=‘DCE’（大商所），
TradingType=‘2’（非交易），ClassType=‘3’（组合合约）才会返回
大商所可申请组合的组合产品信息。
举例：例如STD m2103-C-2400&m2103-P-2400，STD m2103-
C-2500&m2103-P-2500...此系列合约整合成一条组合产品 STD
m_o&m_o 的记录返回。
2）当终端需要查询所有合约，请求时必须填写
TradingType=‘0’（所有状态），ClassType=‘0’（所有合约），返回
的合约信息中，大商所期货组合以合约级别返回（包括能下套利单
的期货组合和只可申请组合的期货组合），不包括大商所可申请组
合的期权组合。
3.当终端调用ReqQryCombPromotionParam接口，
InstrumentID需要填写组合合约产品级别代码，且返回信
息中保证金优惠比例不再以合约级别返回，修改为以产品
级别返回。


举例：当终端需要查询 BLS m2103-P-3150&m2103-P-3100 的
保证金优惠比例，请求查询时InstrumentID需要填写BLS
m_o&m_o。
当终端不指定入参查询（即入参为空），返回的保证金优惠比
例均以产品级别返回。
回到顶部
< 前页 回目录 后页 >




---

## 6.6.1P1版本更新说明

6.6.1P1版本更新说明
版本号：V6.6.1_P1_20210407_traderapi


◇ 1. 兼容性
V6.3.X的API可以访问6.3.X~6.6.X及其以上的CTP后台系统，但
无法支持大商所长合约期权组合，无法支持IPv6；
V6.5.X、V6.6.X的API只能访问6.5.X及其以上的CTP后台系
统，支持大商所长合约期权组合，支持IPv6；
注意：6.6.1API引入的新函数只有在接入6.6.1及以上后台的时
候才会有响应，接入6.5.0时无响应。
回到顶部


◇ 2. 运行环境
Linux操作系统版本为Redhat5.8及以上。
Windows为vista及以上的操作系统，支持32位和64位。
只支持vista及以上的操作系统是由于本次更新支持IPv6，涉
及函数只能在vista及以上版本使用。
回到顶部


◇ 3. API变动


◇ 3.1. 新增‘ClientLoginRemark’字段
因原API接口中ReqUserLogin中的登录备注字段LoginRemark
长度为36个字节，期货公司及终端厂商反馈该字段长度不够，故
在RegisterUserSystemInfo和SubmitUserSystemInfo
的‘CThostFtdcUserSystemInfoField’新增‘ClientLoginRemark’字
段，解决原登录备注长度不足的问题。
如需使用本功能，CTP后台系统需要升级至V6.6.1及以上版
本。
调用的接口如下：
///注册用户终端信息，用于中继服务器多连接模式
///需要在终端认证成功后，用户登录前调用该接口
virtual int RegisterUserSystemInfo(CThostFtdcUserSystemInfoField 
*pUserSystemInfo) = 0;
///上报用户终端信息，用于中继服务器操作员登录模式
///操作员登录后，可以多次调用该接口上报客户信息
virtual int SubmitUserSystemInfo(CThostFtdcUserSystemInfoField 
*pUserSystemInfo) = 0;
接口中新增字段如下：（在CThostFtdcUserSystemInfoField如
下字段，支持中继客户填写登录备注（例如客户端的IP，MAC等
信息），该字段长度为150字节）
///客户登录备注2
TThostFtdcClientLoginRemarkType ClientLoginRemark;


◇ 3.2. 支持中金所顶单功能
支持中金所顶单业务，支持中金所报价填写被顶单的单子
QuoteSysID，顶单即用一笔新报价，来更新相同合约的老报价，
一次指令中包含撤销老报价和录入新报价两个操作，且会生成新
的报价单编号。
如需使用本功能，CTP后台系统需要升级至V6.6.1及以上版
本。
调用的接口如下：
///报价录入请求
virtual int ReqQuoteInsert(CThostFtdcInputQuoteField 
*pInputQuote, int nRequestID) = 0;
接口中CThostFtdcInputQuoteField增加字段:
///被顶单编号
TThostFtdcOrderSysIDType    ReplaceSysID;
注：
1）ReplaceSysID为空：普通报价，即不顶单；
2）ReplaceSysID填"last"：对最近一笔报价顶单；
3）ReplaceSysID填指定报价编号：对指定报价编号顶单。


◇ 3.3. 支持交易所结算风险隔离功能
针对已完成风险计算但尚未完成正式结算的品种，支持查询
该风险产品与风险持仓。
如需使用本功能，CTP后台系统需要升级至V6.6.0及以上版
本。
新增接口如下：
///投资者风险结算持仓查询
virtual int 
ReqQryRiskSettleInvstPosition(CThostFtdcQryRiskSettleInvstPositi
onField *pQryRiskSettleInvstPosition, int nRequestID) = 0;
///风险结算产品查询
virtual int 
ReqQryRiskSettleProductStatus(CThostFtdcQryRiskSettleProductSt
atusField *pQryRiskSettleProductStatus, int nRequestID) = 0;
///投资者风险结算持仓查询响应
virtual void 
OnRspQryRiskSettleInvstPosition(CThostFtdcRiskSettleInvstPositio
nField *pRiskSettleInvstPosition, CThostFtdcRspInfoField 
*pRspInfo, int nRequestID, bool bIsLast) {};
///风险结算产品查询响应
virtual void 
OnRspQryRiskSettleProductStatus(CThostFtdcRiskSettleProductStat
usField *pRiskSettleProductStatus, CThostFtdcRspInfoField 
