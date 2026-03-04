# Callbacks - CTP 6.7.8 API

本节包含 147 个主题。


## OnRspQryParkedOrder

OnRspQryParkedOrder
请求查询预埋单响应，当执行ReqQryParkedOrder后，该方法被调
用。


◇ 1.函数原型
virtual void OnRspQryParkedOrder(CThostFtdcParkedOrderField
*pParkedOrder, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pParkedOrder：预埋单
struct CThostFtdcParkedOrderField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///报单引用
    TThostFtdcOrderRefType  OrderRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///报单价格条件
    TThostFtdcOrderPriceTypeType    OrderPriceType;
    ///买卖方向
    TThostFtdcDirectionType Direction;
    ///组合开平标志
    TThostFtdcCombOffsetFlagType    CombOffsetFlag;
    ///组合投机套保标志
    TThostFtdcCombHedgeFlagType CombHedgeFlag;
    ///价格
    TThostFtdcPriceType LimitPrice;
    ///数量
    TThostFtdcVolumeType    VolumeTotalOriginal;
    ///有效期类型
    TThostFtdcTimeConditionType TimeCondition;
    ///GTD日期
    TThostFtdcDateType  GTDDate;
    ///成交量类型


    TThostFtdcVolumeConditionType   VolumeCondition;
    ///最小成交量
    TThostFtdcVolumeType    MinVolume;
    ///触发条件
    TThostFtdcContingentConditionType   ContingentCondition;
    ///止损价
    TThostFtdcPriceType StopPrice;
    ///强平原因
    TThostFtdcForceCloseReasonType  ForceCloseReason;
    ///自动挂起标志
    TThostFtdcBoolType  IsAutoSuspend;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///用户强平标志
    TThostFtdcBoolType  UserForceClose;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///预埋报单编号
    TThostFtdcParkedOrderIDType ParkedOrderID;
    ///用户类型
    TThostFtdcUserTypeType  UserType;
    ///预埋单状态
    TThostFtdcParkedOrderStatusType Status;
    ///错误代码
    TThostFtdcErrorIDType   ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType  ErrorMsg;
    ///互换单标志
    TThostFtdcBoolType  IsSwapOrder;
    ///资金账号
    TThostFtdcAccountIDType AccountID;
    ///币种代码


    TThostFtdcCurrencyIDType    CurrencyID;
    ///交易编码
    TThostFtdcClientIDType  ClientID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve2;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///IP地址
    TThostFtdcIPAddressType IPAddress;
};
Status：预埋单的报单状态
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryParkedOrderAction

OnRspQryParkedOrderAction
请求查询预埋撤单响应，当执行ReqQryParkedOrderAction后，该
方法被调用。


◇ 1.函数原型
virtual void
OnRspQryParkedOrderAction(CThostFtdcParkedOrderActionField
*pParkedOrderAction, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pParkedOrderAction：输入预埋单操作
struct CThostFtdcParkedOrderActionField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///报单操作引用
    TThostFtdcOrderActionRefType    OrderActionRef;
    ///报单引用
    TThostFtdcOrderRefType  OrderRef;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///报单编号
    TThostFtdcOrderSysIDType    OrderSysID;
    ///操作标志
    TThostFtdcActionFlagType    ActionFlag;
    ///价格
    TThostFtdcPriceType LimitPrice;
    ///数量变化
    TThostFtdcVolumeType    VolumeChange;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///保留的无效字段


    TThostFtdcOldInstrumentIDType   reserve1;
    ///预埋撤单单编号
    TThostFtdcParkedOrderActionIDType   ParkedOrderActionID;
    ///用户类型
    TThostFtdcUserTypeType  UserType;
    ///预埋撤单状态
    TThostFtdcParkedOrderStatusType Status;
    ///错误代码
    TThostFtdcErrorIDType   ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType  ErrorMsg;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve2;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///IP地址
    TThostFtdcIPAddressType IPAddress;
};
Status：报入预埋撤单的报单状态
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};


nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryProduct

OnRspQryProduct
请求查询产品响应，当执行ReqQryProduct后，该方法被调用。


◇ 1.函数原型
virtual void OnRspQryProduct(CThostFtdcProductField *pProduct,
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pProduct：产品
struct CThostFtdcProductField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///产品名称
    TThostFtdcProductNameType   ProductName;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///产品类型
    TThostFtdcProductClassType  ProductClass;
    ///合约数量乘数
    TThostFtdcVolumeMultipleType    VolumeMultiple;
    ///最小变动价位
    TThostFtdcPriceType PriceTick;
    ///市价单最大下单量
    TThostFtdcVolumeType    MaxMarketOrderVolume;
    ///市价单最小下单量
    TThostFtdcVolumeType    MinMarketOrderVolume;
    ///限价单最大下单量
    TThostFtdcVolumeType    MaxLimitOrderVolume;
    ///限价单最小下单量
    TThostFtdcVolumeType    MinLimitOrderVolume;
    ///持仓类型
    TThostFtdcPositionTypeType  PositionType;
    ///持仓日期类型
    TThostFtdcPositionDateTypeType  PositionDateType;
    ///平仓处理类型
    TThostFtdcCloseDealTypeType CloseDealType;
    ///交易币种类型


    TThostFtdcCurrencyIDType    TradeCurrencyID;
    ///质押资金可用范围
    TThostFtdcMortgageFundUseRangeType  MortgageFundUseRange;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve2;
    ///合约基础商品乘数
    TThostFtdcUnderlyingMultipleType    UnderlyingMultiple;
    ///产品代码
    TThostFtdcInstrumentIDType  ProductID;
    ///交易所产品代码
    TThostFtdcInstrumentIDType  ExchangeProductID;
    ///开仓量限制粒度
    TThostFtdcOpenLimitControlLevelType OpenLimitControlLevel;
    ///报单频率控制粒度
    TThostFtdcOrderFreqControlLevelType OrderFreqControlLevel;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryProductExchRate

OnRspQryProductExchRate
请求查询产品报价汇率，当执行ReqQryProductExchRate后，该方
法被调用。


◇ 1.函数原型
virtual void
OnRspQryProductExchRate(CThostFtdcProductExchRateField
*pProductExchRate, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pProductExchRate：产品报价汇率
struct CThostFtdcProductExchRateField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///报价币种类型
    TThostFtdcCurrencyIDType    QuoteCurrencyID;
    ///汇率
    TThostFtdcExchangeRateType  ExchangeRate;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///产品代码
    TThostFtdcInstrumentIDType  ProductID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。


回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryProductGroup

OnRspQryProductGroup
请求查询产品组，当执行ReqQryProductGroup后，该方法被调
用。


◇ 1.函数原型
virtual void
OnRspQryProductGroup(CThostFtdcProductGroupField
*pProductGroup, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pProductGroup：投资者品种/跨品种保证金产品组
struct CThostFtdcProductGroupField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve2;
    ///产品代码
    TThostFtdcInstrumentIDType  ProductID;
    ///产品组代码
    TThostFtdcInstrumentIDType  ProductGroupID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。


回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryQuote

OnRspQryQuote
请求查询报价响应，当执行ReqQryQuote后，该方法被调用。


◇ 1.函数原型
virtual void OnRspQryQuote(CThostFtdcQuoteField *pQuote,
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pQuote：报价
struct CThostFtdcQuoteField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///报价引用
    TThostFtdcOrderRefType  QuoteRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///卖价格
    TThostFtdcPriceType AskPrice;
    ///买价格
    TThostFtdcPriceType BidPrice;
    ///卖数量
    TThostFtdcVolumeType    AskVolume;
    ///买数量
    TThostFtdcVolumeType    BidVolume;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///卖开平标志
    TThostFtdcOffsetFlagType    AskOffsetFlag;
    ///买开平标志
    TThostFtdcOffsetFlagType    BidOffsetFlag;
    ///卖投机套保标志


    TThostFtdcHedgeFlagType AskHedgeFlag;
    ///买投机套保标志
    TThostFtdcHedgeFlagType BidHedgeFlag;
    ///本地报价编号
    TThostFtdcOrderLocalIDType  QuoteLocalID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///会员代码
    TThostFtdcParticipantIDType ParticipantID;
    ///客户代码
    TThostFtdcClientIDType  ClientID;
    ///保留的无效字段
    TThostFtdcOldExchangeInstIDType reserve2;
    ///交易所交易员代码
    TThostFtdcTraderIDType  TraderID;
    ///安装编号
    TThostFtdcInstallIDType InstallID;
    ///报价提示序号
    TThostFtdcSequenceNoType    NotifySequence;
    ///报价提交状态
    TThostFtdcOrderSubmitStatusType OrderSubmitStatus;
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///结算编号
    TThostFtdcSettlementIDType  SettlementID;
    ///报价编号
    TThostFtdcOrderSysIDType    QuoteSysID;
    ///报单日期
    TThostFtdcDateType  InsertDate;
    ///插入时间
    TThostFtdcTimeType  InsertTime;
    ///撤销时间
    TThostFtdcTimeType  CancelTime;
    ///报价状态


    TThostFtdcOrderStatusType   QuoteStatus;
    ///结算会员编号
    TThostFtdcParticipantIDType ClearingPartID;
    ///序号
    TThostFtdcSequenceNoType    SequenceNo;
    ///卖方报单编号
    TThostFtdcOrderSysIDType    AskOrderSysID;
    ///买方报单编号
    TThostFtdcOrderSysIDType    BidOrderSysID;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///用户端产品信息
    TThostFtdcProductInfoType   UserProductInfo;
    ///状态信息
    TThostFtdcErrorMsgType  StatusMsg;
    ///操作用户代码
    TThostFtdcUserIDType    ActiveUserID;
    ///经纪公司报价编号
    TThostFtdcSequenceNoType    BrokerQuoteSeq;
    ///衍生卖报单引用
    TThostFtdcOrderRefType  AskOrderRef;
    ///衍生买报单引用
    TThostFtdcOrderRefType  BidOrderRef;
    ///应价编号
    TThostFtdcOrderSysIDType    ForQuoteSysID;
    ///营业部编号
    TThostFtdcBranchIDType  BranchID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///资金账号
    TThostFtdcAccountIDType AccountID;
    ///币种代码


    TThostFtdcCurrencyIDType    CurrencyID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve3;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///合约在交易所的代码
    TThostFtdcExchangeInstIDType    ExchangeInstID;
    ///IP地址
    TThostFtdcIPAddressType IPAddress;
    ///被顶单编号
    TThostFtdcOrderSysIDType    ReplaceSysID;
    ///有效期类型
    TThostFtdcTimeConditionType TimeCondition;
    ///报单回显字段
    TThostFtdcOrderMemoType OrderMemo;
    ///session上请求计数 api自动维护
    TThostFtdcSequenceNo12Type  SessionReqSeq;
};
InstallID：CTP内部使用
OrderSubmitStatus：报价的最后状态
QuoteStatus：报价是否有报出，或者收到交易所的回报
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息


    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQrySecAgentACIDMap

OnRspQrySecAgentACIDMap
请求查询二级代理操作员银期权限响应，当执行
ReqQrySecAgentACIDMap后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQrySecAgentACIDMap(CThostFtdcSecAgentACIDMapField
*pSecAgentACIDMap, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pSecAgentACIDMap：二级代理操作员银期权限
struct CThostFtdcSecAgentACIDMapField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///用户代码
    TThostFtdcUserIDType UserID;
    ///资金账户
    TThostFtdcAccountIDType AccountID;
    ///币种
    TThostFtdcCurrencyIDType CurrencyID;
    ///境外中介机构资金帐号
    TThostFtdcAccountIDType BrokerSecAgentID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。


回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQrySecAgentCheckMode

OnRspQrySecAgentCheckMode
请求查询二级代理商资金校验模式响应


◇ 1.函数原型
virtual void
OnRspQrySecAgentCheckMode(CThostFtdcSecAgentCheckModeField
*pSecAgentCheckMode, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pSecAgentCheckMode：二级代理商资金校验模式
struct CThostFtdcSecAgentCheckModeField
{
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///币种
    TThostFtdcCurrencyIDType CurrencyID;
    ///境外中介机构资金帐号
    TThostFtdcAccountIDType BrokerSecAgentID;
    ///是否需要校验自己的资金账户
    TThostFtdcBoolType CheckSelfAccount;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。


回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQrySecAgentTradeInfo

OnRspQrySecAgentTradeInfo
请求查询二级代理商信息响应


◇ 1.函数原型
virtual void
OnRspQrySecAgentTradeInfo(CThostFtdcSecAgentTradeInfoField
*pSecAgentTradeInfo, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pSecAgentTradeInfo：二级代理商信息
struct CThostFtdcSecAgentTradeInfoField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///境外中介机构资金帐号
    TThostFtdcAccountIDType BrokerSecAgentID;
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///二级代理商姓名
    TThostFtdcLongIndividualNameType LongCustomerName;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQrySecAgentTradingAccount

OnRspQrySecAgentTradingAccount
请求查询资金账户响应


◇ 1.函数原型
virtual void
OnRspQrySecAgentTradingAccount(CThostFtdcTradingAccountField
*pTradingAccount, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pTradingAccount：资金账户
struct CThostFtdcTradingAccountField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者帐号
    TThostFtdcAccountIDType AccountID;
    ///上次质押金额
    TThostFtdcMoneyType PreMortgage;
    ///上次信用额度
    TThostFtdcMoneyType PreCredit;
    ///上次存款额
    TThostFtdcMoneyType PreDeposit;
    ///上次结算准备金
    TThostFtdcMoneyType PreBalance;
    ///上次占用的保证金
    TThostFtdcMoneyType PreMargin;
    ///利息基数
    TThostFtdcMoneyType InterestBase;
    ///利息收入
    TThostFtdcMoneyType Interest;
    ///入金金额
    TThostFtdcMoneyType Deposit;
    ///出金金额
    TThostFtdcMoneyType Withdraw;
    ///冻结的保证金
    TThostFtdcMoneyType FrozenMargin;
    ///冻结的资金
    TThostFtdcMoneyType FrozenCash;
    ///冻结的手续费


    TThostFtdcMoneyType FrozenCommission;
    ///当前保证金总额
    TThostFtdcMoneyType CurrMargin;
    ///资金差额
    TThostFtdcMoneyType CashIn;
    ///手续费
    TThostFtdcMoneyType Commission;
    ///平仓盈亏
    TThostFtdcMoneyType CloseProfit;
    ///持仓盈亏
    TThostFtdcMoneyType PositionProfit;
    ///期货结算准备金
    TThostFtdcMoneyType Balance;
    ///可用资金
    TThostFtdcMoneyType Available;
    ///可取资金
    TThostFtdcMoneyType WithdrawQuota;
    ///基本准备金
    TThostFtdcMoneyType Reserve;
    ///交易日
    TThostFtdcDateType TradingDay;
    ///结算编号
    TThostFtdcSettlementIDType SettlementID;
    ///信用额度
    TThostFtdcMoneyType Credit;
    ///质押金额
    TThostFtdcMoneyType Mortgage;
    ///交易所保证金
    TThostFtdcMoneyType ExchangeMargin;
    ///投资者交割保证金
    TThostFtdcMoneyType DeliveryMargin;
    ///交易所交割保证金
    TThostFtdcMoneyType ExchangeDeliveryMargin;
    ///保底期货结算准备金


    TThostFtdcMoneyType ReserveBalance;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
    ///上次货币质入金额
    TThostFtdcMoneyType PreFundMortgageIn;
    ///上次货币质出金额
    TThostFtdcMoneyType PreFundMortgageOut;
    ///货币质入金额
    TThostFtdcMoneyType FundMortgageIn;
    ///货币质出金额
    TThostFtdcMoneyType FundMortgageOut;
    ///货币质押余额
    TThostFtdcMoneyType FundMortgageAvailable;
    ///可质押货币金额--已废弃
    TThostFtdcMoneyType MortgageableFund;
    ///特殊产品占用保证金--已废弃
    TThostFtdcMoneyType SpecProductMargin;
    ///特殊产品冻结保证金--已废弃 
    TThostFtdcMoneyType SpecProductFrozenMargin;
    ///特殊产品手续费--已废弃
    TThostFtdcMoneyType SpecProductCommission;
    ///特殊产品冻结手续费--已废弃
    TThostFtdcMoneyType SpecProductFrozenCommission;
    ///特殊产品持仓盈亏--已废弃
    TThostFtdcMoneyType SpecProductPositionProfit;
    ///特殊产品平仓盈亏--已废弃
    TThostFtdcMoneyType SpecProductCloseProfit;
    ///根据持仓盈亏算法计算的特殊产品持仓盈亏--已废弃
    TThostFtdcMoneyType SpecProductPositionProfitByAlg;
    ///特殊产品交易所保证金--已废弃
    TThostFtdcMoneyType SpecProductExchangeMargin;
    ///业务类型
    TThostFtdcBizTypeType BizType;
    ///延时换汇冻结金额


    TThostFtdcMoneyType FrozenSwap;
    ///剩余换汇额度
    TThostFtdcMoneyType RemainSwap;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQrySettlementInfo

OnRspQrySettlementInfo
请求查询投资者结算结果响应，当执行ReqQrySettlementInfo后，
该方法被调用。


◇ 1.函数原型
virtual void
OnRspQrySettlementInfo(CThostFtdcSettlementInfoField
*pSettlementInfo, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pSettlementInfo：投资者结算结果
struct CThostFtdcSettlementInfoField
{
    ///交易日
    TThostFtdcDateType TradingDay;
    ///结算编号
    TThostFtdcSettlementIDType SettlementID;
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///序号
    TThostFtdcSequenceNoType SequenceNo;
    ///消息正文
    TThostFtdcContentType Content;
    ///投资者帐号
    TThostFtdcAccountIDType AccountID;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};


nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQrySettlementInfoConfirm

OnRspQrySettlementInfoConfirm
请求查询结算信息确认响应，当执行ReqQrySettlementInfoConfirm
后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQrySettlementInfoConfirm(CThostFtdcSettlementInfoConfirmFie
ld *pSettlementInfoConfirm, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pSettlementInfoConfirm：投资者结算结果确认信息
struct CThostFtdcSettlementInfoConfirmField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///确认日期
    TThostFtdcDateType ConfirmDate;
    ///确认时间
    TThostFtdcTimeType ConfirmTime;
    ///结算编号
    TThostFtdcSettlementIDType SettlementID;
    ///投资者帐号
    TThostFtdcAccountIDType AccountID;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};


nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryTrade

OnRspQryTrade
请求查询成交响应，当执行ReqQryTrade后，该方法被调用。


◇ 1.函数原型
virtual void OnRspQryTrade(CThostFtdcTradeField *pTrade,
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pTrade：成交
struct CThostFtdcTradeField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///报单引用
    TThostFtdcOrderRefType  OrderRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///成交编号
    TThostFtdcTradeIDType   TradeID;
    ///买卖方向
    TThostFtdcDirectionType Direction;
    ///报单编号
    TThostFtdcOrderSysIDType    OrderSysID;
    ///会员代码
    TThostFtdcParticipantIDType ParticipantID;
    ///客户代码
    TThostFtdcClientIDType  ClientID;
    ///交易角色
    TThostFtdcTradingRoleType   TradingRole;
    ///保留的无效字段
    TThostFtdcOldExchangeInstIDType reserve2;
    ///开平标志


    TThostFtdcOffsetFlagType    OffsetFlag;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///价格
    TThostFtdcPriceType Price;
    ///数量
    TThostFtdcVolumeType    Volume;
    ///成交时期
    TThostFtdcDateType  TradeDate;
    ///成交时间
    TThostFtdcTimeType  TradeTime;
    ///成交类型
    TThostFtdcTradeTypeType TradeType;
    ///成交价来源
    TThostFtdcPriceSourceType   PriceSource;
    ///交易所交易员代码
    TThostFtdcTraderIDType  TraderID;
    ///本地报单编号
    TThostFtdcOrderLocalIDType  OrderLocalID;
    ///结算会员编号
    TThostFtdcParticipantIDType ClearingPartID;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///序号
    TThostFtdcSequenceNoType    SequenceNo;
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///结算编号
    TThostFtdcSettlementIDType  SettlementID;
    ///经纪公司报单编号
    TThostFtdcSequenceNoType    BrokerOrderSeq;
    ///成交来源
    TThostFtdcTradeSourceType   TradeSource;
    ///投资单元代码


    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///合约在交易所的代码
    TThostFtdcExchangeInstIDType    ExchangeInstID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
不同交易所，为什么TradeDate有的是自然日有的是交易
日？
TradeDate字段，大商所、郑商所回报中该字段为交易
日；上期所、能源回报为自然日。
建议确认一笔成交的时间用Tradingday+TradeTime这
一组字段。
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryTradingAccount

OnRspQryTradingAccount
请求查询资金账户响应，当执行ReqQryTradingAccount后，该方
法被调用。


◇ 1.函数原型
virtual void
OnRspQryTradingAccount(CThostFtdcTradingAccountField
*pTradingAccount, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pTradingAccount：资金账户
struct CThostFtdcTradingAccountField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者帐号
    TThostFtdcAccountIDType AccountID;
    ///上次质押金额
    TThostFtdcMoneyType PreMortgage;
    ///上次信用额度
    TThostFtdcMoneyType PreCredit;
    ///上次存款额
    TThostFtdcMoneyType PreDeposit;
    ///上次结算准备金
    TThostFtdcMoneyType PreBalance;
    ///上次占用的保证金
    TThostFtdcMoneyType PreMargin;
    ///利息基数
    TThostFtdcMoneyType InterestBase;
    ///利息收入
