    TThostFtdcOldInstrumentIDType   reserve3;
    ///产品类型
    TThostFtdcProductClassType  ProductClass;
    ///交割年份
    TThostFtdcYearType  DeliveryYear;
    ///交割月
    TThostFtdcMonthType DeliveryMonth;
    ///市价单最大下单量
    TThostFtdcVolumeType    MaxMarketOrderVolume;
    ///市价单最小下单量
    TThostFtdcVolumeType    MinMarketOrderVolume;
    ///限价单最大下单量
    TThostFtdcVolumeType    MaxLimitOrderVolume;
    ///限价单最小下单量
    TThostFtdcVolumeType    MinLimitOrderVolume;
    ///合约数量乘数
    TThostFtdcVolumeMultipleType    VolumeMultiple;
    ///最小变动价位


    TThostFtdcPriceType PriceTick;
    ///创建日
    TThostFtdcDateType  CreateDate;
    ///上市日
    TThostFtdcDateType  OpenDate;
    ///到期日
    TThostFtdcDateType  ExpireDate;
    ///开始交割日
    TThostFtdcDateType  StartDelivDate;
    ///结束交割日
    TThostFtdcDateType  EndDelivDate;
    ///合约生命周期状态
    TThostFtdcInstLifePhaseType InstLifePhase;
    ///当前是否交易
    TThostFtdcBoolType  IsTrading;
    ///持仓类型
    TThostFtdcPositionTypeType  PositionType;
    ///持仓日期类型
    TThostFtdcPositionDateTypeType  PositionDateType;
    ///多头保证金率
    TThostFtdcRatioType LongMarginRatio;
    ///空头保证金率
    TThostFtdcRatioType ShortMarginRatio;
    ///是否使用大额单边保证金算法
    TThostFtdcMaxMarginSideAlgorithmType    
MaxMarginSideAlgorithm;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve4;
    ///执行价
    TThostFtdcPriceType StrikePrice;
    ///期权类型
    TThostFtdcOptionsTypeType   OptionsType;
    ///合约基础商品乘数
    TThostFtdcUnderlyingMultipleType    UnderlyingMultiple;


    ///组合类型
    TThostFtdcCombinationTypeType   CombinationType;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///合约在交易所的代码
    TThostFtdcExchangeInstIDType    ExchangeInstID;
    ///产品代码
    TThostFtdcInstrumentIDType  ProductID;
    ///基础商品代码
    TThostFtdcInstrumentIDType  UnderlyingInstrID;
};
VolumeMultiple：合约乘数（同交易所）
PriceTick：最小变动价位（同交易所）
IsTrading：是否活跃（同交易所）
DeliveryYear：交割年份（同交易所）
DeliveryMonth：交割月（同交易所）
MaxMarketOrderVolume:（同交易所，郑商所的由柜台设置）
MinMarketOrderVolume:（同交易所，市价单最小下单量来自柜
台）
MaxLimitOrderVolume:（同交易所）
MinLimitOrderVolume:（同交易所）
柜台版本6.6.5及以上版本支持每日从交易所更新市价单、限
价单最大最小报单量；
CTP不会根据合约信息里的下单量限制去判断下单手数，交
由交易所判断。
OpenDate：上市日（同交易所）
CreateDate：创建日（同交易所）


ExpireDate：到期日（同交易所）
StartDeliveDate：开始交割日（同交易所）
EndDelivDate：结束交割日（同交易所）
同交易所表示这些字段每天更新自交易所，其余字段为柜台
设置值。如果发现有些字段值有误，则以此来判断是否CTP柜台
设置问题。
PositionDateType:持仓日期类型（区分产品是否有昨仓和今仓的
区别，只有上海和能源是使用历史持仓）
LongMarginRatio：多头保证金率（终值），算法如下：
跟随交易所多头保证金率：LongMarginRatioByMoney（由
OnRspQryExchangeMarginRate查到）+LongMarginRatioByMoney
（由OnRspQryExchangeMarginRateAdjust查到）
不跟随交易所多头保证金率：NoLongMarginRatioByMoney（由
OnRspQryExchangeMarginRateAdjust查到）
ShortMarginRatio：空头保证金率（终值），算法如下：
跟随交易所空头保证金率：ShortMarginRatioByMoney（由
OnRspQryExchangeMarginRate查到）+ShortMarginRatioByMoney
（由OnRspQryExchangeMarginRateAdjust查到）
不跟随交易所空头保证金率：NoShortMarginRatioByMoney（由
OnRspQryExchangeMarginRateAdjust查到）
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

## OnRspQryInstrumentCommissionRate

OnRspQryInstrumentCommissionRate
请求查询合约手续费率响应，当执行
ReqQryInstrumentCommissionRate后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQryInstrumentCommissionRate(CThostFtdcInstrumentCommissio
nRateField *pInstrumentCommissionRate, CThostFtdcRspInfoField
*pRspInfo, int nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInstrumentCommissionRate：合约手续费率
struct CThostFtdcInstrumentCommissionRateField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///投资者范围
    TThostFtdcInvestorRangeType InvestorRange;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///开仓手续费率
    TThostFtdcRatioType OpenRatioByMoney;
    ///开仓手续费
    TThostFtdcRatioType OpenRatioByVolume;
    ///平仓手续费率
    TThostFtdcRatioType CloseRatioByMoney;
    ///平仓手续费
    TThostFtdcRatioType CloseRatioByVolume;
    ///平今手续费率
    TThostFtdcRatioType CloseTodayRatioByMoney;
    ///平今手续费
    TThostFtdcRatioType CloseTodayRatioByVolume;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///业务类型
    TThostFtdcBizTypeType   BizType;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///合约代码


    TThostFtdcInstrumentIDType  InstrumentID;
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

## OnRspQryInstrumentMarginRate

OnRspQryInstrumentMarginRate
请求查询合约保证金率响应，当执行ReqQryInstrumentMarginRate
后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQryInstrumentMarginRate(CThostFtdcInstrumentMarginRateFiel
d *pInstrumentMarginRate, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
///pInstrumentMarginRate：合约保证金率
struct CThostFtdcInstrumentMarginRateField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///投资者范围
    TThostFtdcInvestorRangeType InvestorRange;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///多头保证金率
    TThostFtdcRatioType LongMarginRatioByMoney;
    ///多头保证金费
    TThostFtdcMoneyType LongMarginRatioByVolume;
    ///空头保证金率
    TThostFtdcRatioType ShortMarginRatioByMoney;
    ///空头保证金费
    TThostFtdcMoneyType ShortMarginRatioByVolume;
    ///是否相对交易所收取
    TThostFtdcBoolType  IsRelative;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
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

## OnRspQryInstrumentOrderCommRate

OnRspQryInstrumentOrderCommRate
请求查询报单手续费响应，当执行
ReqQryInstrumentOrderCommRate后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQryInstrumentOrderCommRate(CThostFtdcInstrumentOrderCom
mRateField *pInstrumentOrderCommRate, CThostFtdcRspInfoField
*pRspInfo, int nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInstrumentOrderCommRate：当前报单手续费的详细内容
struct CThostFtdcInstrumentOrderCommRateField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///投资者范围
    TThostFtdcInvestorRangeType InvestorRange;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///报单手续费
    TThostFtdcRatioType OrderCommByVolume;
    ///撤单手续费
    TThostFtdcRatioType OrderActionCommByVolume;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///报单手续费
    TThostFtdcRatioType OrderCommByTrade;
    ///撤单手续费
    TThostFtdcRatioType OrderActionCommByTrade;
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

## OnRspQryInvestor

OnRspQryInvestor
请求查询投资者响应，当执行ReqQryInvestor后，该方法被调用。


◇ 1.函数原型
virtual void OnRspQryInvestor(CThostFtdcInvestorField
*pInvestor, CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool
bIsLast) {};
回到顶部


◇ 2.参数
pInvestor：投资者
struct CThostFtdcInvestorField
{
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者分组代码
    TThostFtdcInvestorIDType InvestorGroupID;
    ///投资者名称
    TThostFtdcPartyNameType InvestorName;
    ///证件类型
    TThostFtdcIdCardTypeType IdentifiedCardType;
    ///证件号码
    TThostFtdcIdentifiedCardNoType IdentifiedCardNo;
    ///是否活跃
    TThostFtdcBoolType IsActive;
    ///联系电话
    TThostFtdcTelephoneType Telephone;
    ///通讯地址
    TThostFtdcAddressType Address;
    ///开户日期
    TThostFtdcDateType OpenDate;
    ///手机
    TThostFtdcMobileType Mobile;
    ///手续费率模板代码
    TThostFtdcInvestorIDType CommModelID;
    ///保证金率模板代码
    TThostFtdcInvestorIDType MarginModelID;
    ///是否频率控制


    TThostFtdcEnumBoolType  IsOrderFreq;
    ///是否开仓限制
    TThostFtdcEnumBoolType  IsOpenVolLimit;
};
CommModelID：CTP内部使用
MarginModelID：CTP内部使用
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

## OnRspQryInvestorPosition

OnRspQryInvestorPosition
请求查询投资者持仓响应，当执行ReqQryInvestorPosition后，该
方法被调用。
CTP 系统将持仓明细记录按合约，持仓方向，开仓日期（仅针对
上期所，区分昨仓、今仓）进行汇总。
卖可平数量=多头仓-shortfrozen
买可平数量=空头仓-longfrozen


◇ 1.函数原型
virtual void
OnRspQryInvestorPosition(CThostFtdcInvestorPositionField
*pInvestorPosition, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pInvestorPosition：投资者持仓
struct CThostFtdcInvestorPositionField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///持仓多空方向
    TThostFtdcPosiDirectionType PosiDirection;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///持仓日期
    TThostFtdcPositionDateType  PositionDate;
    ///上日持仓
    TThostFtdcVolumeType    YdPosition;
    ///今日持仓
    TThostFtdcVolumeType    Position;
    ///多头冻结
    TThostFtdcVolumeType    LongFrozen;
    ///空头冻结
    TThostFtdcVolumeType    ShortFrozen;
    ///开仓冻结金额
    TThostFtdcMoneyType LongFrozenAmount;
    ///开仓冻结金额
    TThostFtdcMoneyType ShortFrozenAmount;
    ///开仓量
    TThostFtdcVolumeType    OpenVolume;
    ///平仓量


    TThostFtdcVolumeType    CloseVolume;
    ///开仓金额
    TThostFtdcMoneyType OpenAmount;
    ///平仓金额
    TThostFtdcMoneyType CloseAmount;
    ///持仓成本
    TThostFtdcMoneyType PositionCost;
    ///上次占用的保证金
    TThostFtdcMoneyType PreMargin;
    ///占用的保证金
    TThostFtdcMoneyType UseMargin;
    ///冻结的保证金
    TThostFtdcMoneyType FrozenMargin;
    ///冻结的资金
    TThostFtdcMoneyType FrozenCash;
    ///冻结的手续费
    TThostFtdcMoneyType FrozenCommission;
    ///资金差额
    TThostFtdcMoneyType CashIn;
    ///手续费
    TThostFtdcMoneyType Commission;
    ///平仓盈亏
    TThostFtdcMoneyType CloseProfit;
    ///持仓盈亏
    TThostFtdcMoneyType PositionProfit;
    ///上次结算价
    TThostFtdcPriceType PreSettlementPrice;
    ///本次结算价
    TThostFtdcPriceType SettlementPrice;
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///结算编号
    TThostFtdcSettlementIDType  SettlementID;
    ///开仓成本


    TThostFtdcMoneyType OpenCost;
    ///交易所保证金
    TThostFtdcMoneyType ExchangeMargin;
    ///组合成交形成的持仓
    TThostFtdcVolumeType    CombPosition;
    ///组合多头冻结
    TThostFtdcVolumeType    CombLongFrozen;
    ///组合空头冻结
    TThostFtdcVolumeType    CombShortFrozen;
    ///逐日盯市平仓盈亏
    TThostFtdcMoneyType CloseProfitByDate;
    ///逐笔对冲平仓盈亏
    TThostFtdcMoneyType CloseProfitByTrade;
    ///今日持仓
    TThostFtdcVolumeType    TodayPosition;
    ///保证金率
    TThostFtdcRatioType MarginRateByMoney;
    ///保证金率(按手数)
    TThostFtdcRatioType MarginRateByVolume;
    ///执行冻结
    TThostFtdcVolumeType    StrikeFrozen;
    ///执行冻结金额
    TThostFtdcMoneyType StrikeFrozenAmount;
    ///放弃执行冻结
    TThostFtdcVolumeType    AbandonFrozen;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///执行冻结的昨仓
    TThostFtdcVolumeType    YdStrikeFrozen;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///持仓成本差值（柜台版本6.7.1后已废弃）
    TThostFtdcMoneyType PositionCostOffset;
    ///tas持仓手数


    TThostFtdcVolumeType    TasPosition;
    ///tas持仓成本
    TThostFtdcMoneyType TasPositionCost;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
};
PositionProfit：期权没有持仓盈亏，为0
Position：表示当前持仓数量
TodayPosition：表示今新开仓
YdPosition：表示昨日收盘时持仓数量（≠ 当前的昨仓数量，静
态，日间不随着开平仓而变化）
当前的昨仓数量 = ∑Position -∑TodayPosition
YdStrikeFrozen：该字段是给个股期权用的，期货期权里一直保
持为0
PositionCost：持仓成本=上日持仓 * 昨结算价 * 合约乘数 +
SUM（今日持仓 * 开仓价 * 合约乘数）
PositionDate：持仓日期（用于区分上海和能源的仓是今仓还是
昨仓）
OpenAmount:开仓金额（所有开仓单的金额累加用于计算开仓
手续费）开仓金额是当天一直累加，即使仓位平掉了手续费也要计
算。
OpenCost：持仓成本（当前每笔持仓明细最初的开仓价*合约乘
数）计算当前仓位的实际数量。
SettlementPrice：该字段日初为昨结算价，期权空头持仓和期货
持仓中该字段会实时跟随行情的最新价变动，而期权多头持仓中该
字段为该仓位变动时的最新价，不随行情变动。例如某期权多头持


仓对应的合约最新价为10，持仓手数为2，此时以成交价20平仓了1
手，平仓后该多头持仓的SettlementPrice会变更为10。
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
投资者发现自己账号里面莫名其妙多了一条“PRT
SR711&SR711C6000"的持仓，是什么原因？
这个合约是郑商所跨式，宽跨式期权组合合约。此合
约不需要投资者使用特别的指令构建，只要客户持有可以
构成备兑期权组合的持仓。在每日结算的时候，郑商所会
将符合条件的期权和期货持仓自动确认为备兑期权套利持
仓，包括备兑看涨期权套利和备兑看跌期权套利，并给予
保证金优惠。
对于此期权合约，投资者需要分腿平仓，不需要管这
个是否为备兑组合。
各大交易所保证金的单边收取规则有的是盘中生效，有
的是结算后生效。那查询的持仓中的保证金占用是否有
区别？
查询后的结果都是按照优惠收取。
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryInvestorPositionCombineDetail

OnRspQryInvestorPositionCombineDet
ail
请求查询投资者持仓明细响应，当执行
ReqQryInvestorPositionCombineDetail后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQryInvestorPositionCombineDetail(CThostFtdcInvestorPositionC
ombineDetailField *pInvestorPositionCombineDetail,
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInvestorPositionCombineDetail：投资者组合持仓明细
struct CThostFtdcInvestorPositionCombineDetailField
{
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///开仓日期
    TThostFtdcDateType  OpenDate;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///结算编号
    TThostFtdcSettlementIDType  SettlementID;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///组合编号
    TThostFtdcTradeIDType   ComTradeID;
    ///撮合编号
    TThostFtdcTradeIDType   TradeID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///买卖
    TThostFtdcDirectionType Direction;
    ///持仓量
    TThostFtdcVolumeType    TotalAmt;
    ///投资者保证金
    TThostFtdcMoneyType Margin;
    ///交易所保证金


    TThostFtdcMoneyType ExchMargin;
    ///保证金率
    TThostFtdcRatioType MarginRateByMoney;
    ///保证金率(按手数)
    TThostFtdcRatioType MarginRateByVolume;
    ///单腿编号
    TThostFtdcLegIDType LegID;
    ///单腿乘数
    TThostFtdcLegMultipleType   LegMultiple;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve2;
    ///成交组号
    TThostFtdcTradeGroupIDType  TradeGroupID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///组合持仓合约编码
    TThostFtdcInstrumentIDType  CombInstrumentID;
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
组合持仓明细应该如何去判断谁和谁是一对？
用ComTradeID判断。注意以下几点：
1) ComTradeID是CTP定义的。盘中新开仓的时候Com
TradeID比如是115，在盘后结算完成后为了防止重复会重
新打上编号，编号里带上交易日，如20170720000003；
2) ComTradeID并不一定是成对出现。比如下单五手
组合合约：左腿全部成交，右腿是分笔成交的1，1，3的
方式成交的。所以总的收到的成交回报就会有4条。这四
条的ComTradeID，在盘中是一致的。
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryInvestorPositionDetail

OnRspQryInvestorPositionDetail
请求查询投资者持仓明细响应，当执行
ReqQryInvestorPositionDetail后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQryInvestorPositionDetail(CThostFtdcInvestorPositionDetailField
*pInvestorPositionDetail, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInvestorPositionDetail：投资者持仓明细
struct CThostFtdcInvestorPositionDetailField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///买卖
    TThostFtdcDirectionType Direction;
    ///开仓日期
    TThostFtdcDateType  OpenDate;
    ///成交编号
    TThostFtdcTradeIDType   TradeID;
    ///数量
    TThostFtdcVolumeType    Volume;
    ///开仓价
    TThostFtdcPriceType OpenPrice;
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///结算编号
    TThostFtdcSettlementIDType  SettlementID;
    ///成交类型
    TThostFtdcTradeTypeType TradeType;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve2;
    ///交易所代码


    TThostFtdcExchangeIDType    ExchangeID;
    ///逐日盯市平仓盈亏
    TThostFtdcMoneyType CloseProfitByDate;
    ///逐笔对冲平仓盈亏
    TThostFtdcMoneyType CloseProfitByTrade;
    ///逐日盯市持仓盈亏
    TThostFtdcMoneyType PositionProfitByDate;
    ///逐笔对冲持仓盈亏
    TThostFtdcMoneyType PositionProfitByTrade;
    ///投资者保证金
    TThostFtdcMoneyType Margin;
    ///交易所保证金
    TThostFtdcMoneyType ExchMargin;
    ///保证金率
    TThostFtdcRatioType MarginRateByMoney;
    ///保证金率(按手数)
    TThostFtdcRatioType MarginRateByVolume;
    ///昨结算价
    TThostFtdcPriceType LastSettlementPrice;
    ///结算价
    TThostFtdcPriceType SettlementPrice;
    ///平仓量
    TThostFtdcVolumeType    CloseVolume;
    ///平仓金额
    TThostFtdcMoneyType CloseAmount;
    ///先开先平剩余数量
    TThostFtdcVolumeType    TimeFirstVolume;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///特殊持仓标志
    TThostFtdcSpecPosiTypeType  SpecPosiType;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///组合合约代码


    TThostFtdcInstrumentIDType  CombInstrumentID;
};
SettlementPrice：该字段日初为昨结算价，仓位变动时，会更新
为当时的最新价，不随行情变动。例如某持仓对应的合约最新价为
10，持仓手数为2，此时以成交价20平仓了1手，平仓后该多头持仓
的SettlementPrice会变更为10。
Volume：如果是大商所的持仓则按照先单一后组合的平仓顺序
显示平仓后的剩余手数。
PositionProfitByTrade：大商所开启rule后，大商所的期权会计
算持仓盈亏，其他交易所无影响（不计算持仓盈亏）。
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





