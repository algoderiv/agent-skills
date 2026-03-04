---

## ReqQryDepthMarketData

ReqQryDepthMarketData
请求查询行情，只能查询当前快照，不能查询历史行情。
响应: OnRspQryDepthMarketData


◇ 1.函数原型
virtual int
ReqQryDepthMarketData(CThostFtdcQryDepthMarketDataField
*pQryDepthMarketData, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryDepthMarketData：查询行情
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
TThostFtdcProductClassType
ProductClass 产品类
型
是
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryEWarrantOffset
请求查询仓单折抵信息
响应: OnRspQryEWarrantOffset


◇ 1.函数原型
virtual int
ReqQryEWarrantOffset(CThostFtdcQryEWarrantOffsetField
*pQryEWarrantOffset, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryEWarrantOffset：查询仓单折抵信息
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcInvestUnitIDType
InvestUnitID 投资单
元代码
否
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryExchange
请求查询交易所
响应: OnRspQryExchange


◇ 1.函数原型
virtual int ReqQryExchange(CThostFtdcQryExchangeField
*pQryExchange, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExchange：查询交易所
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcExchangeIDType ExchangeID 交易所
代码
是
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryExchangeMarginRate
请求查询交易所保证金率
响应: OnRspQryExchangeMarginRate


◇ 1.函数原型
virtual int
ReqQryExchangeMarginRate(CThostFtdcQryExchangeMarginRateField
*pQryExchangeMarginRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExchangeMarginRate：查询交易所保证金率
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
是
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcHedgeFlagType
HedgeFlag
投机套
保标志
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryExchangeMarginRateAdjust
请求查询交易所调整保证金率
响应: OnRspQryExchangeMarginRateAdjust


◇ 1.函数原型
virtual int
ReqQryExchangeMarginRateAdjust(CThostFtdcQryExchangeMarginRat
eAdjustField *pQryExchangeMarginRateAdjust, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExchangeMarginRateAdjust：查询交易所调整保证金率
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
是
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcHedgeFlagType
HedgeFlag
投机套
保标志
是
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryForQuote

ReqQryForQuote
请求查询询价
响应：OnRspQryForQuote


◇ 1.函数原型
virtual int ReqQryForQuote(CThostFtdcQryForQuoteField
*pQryForQuote, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryForQuote：询价查询
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcInstrumentIDType
InstrumentID
合约代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcTimeType
InsertTimeStart 开始时
间
是
TThostFtdcTimeType
InsertTimeEnd
结束时
间
是
TThostFtdcInvestUnitIDType
InvestUnitID
投资单
元代码
否
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryInstrument
请求查询合约，填空可以查询到所有合约。
响应:OnRspQryInstrument


◇ 1.函数原型
virtual int ReqQryInstrument(CThostFtdcQryInstrumentField
*pQryInstrument, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInstrument：查询合约
字段类型
字段名称
含义
是否可
作为过
滤条件
TThostFtdcInstrumentIDType
InstrumentID
合约代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcExchangeInstIDType
ExchangeInstID
合约在
交易所
的代码
是
TThostFtdcInstrumentIDType
ProductID
产品代
码
是
TThostFtdcOldInstrumentIDType
reserve1
保留的
无效字
段
否
TThostFtdcOldExchangeInstIDType reserve2
保留的
无效字
段
否
TThostFtdcOldInstrumentIDType
reserve3
保留的
无效字
段
否
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。


回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcQryInstrumentField a = { 0 };
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryInstrument(&a, nRequestID++);
回到顶部


◇ 5.FAQ
为何查询不到郑商所跨式和宽跨式套利合约？
郑商所没有推跨式和宽跨式套利合约，所以查询不到
此类合约。
为何查询中金所的EFP（期转现）合约，
InstrumentName为空？
交易所就没有推送instrumentname。
回到顶部
< 前页 回目录 后页 >


ReqQryInstrumentCommissionRate
请求查询合约手续费率，对应响应
OnRspQryInstrumentCommissionRate。如果InstrumentID填空，则返回
持仓对应的合约手续费率。
目前无法通过一次查询得到所有合约手续费率，如果要查询所
有，则需要通过多次查询得到。


◇ 1.函数原型
virtual int
ReqQryInstrumentCommissionRate(CThostFtdcQryInstrumentCommissi
onRateField *pQryInstrumentCommissionRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInstrumentCommissionRate：查询手续费率
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcInvestUnitIDType
InvestUnitID 投资单
元代码
否
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
InstrumentID：返回手续费率对应的合约。
但是如果在柜台没有设置具体合约的手续费率，则默认会返
回产品的手续费率，InstrumentID就为对应产品ID。
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcQryInstrumentCommissionRateField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
m_pUserApi->ReqQryInstrumentCommissionRate(&a, 
nRequestID++);
回到顶部


◇ 5.FAQ
查询返回结果是交易所手续费率还是投资者手续费率？
返回的是投资者手续费率。
回到顶部
< 前页 回目录 后页 >


ReqQryInstrumentMarginRate
请求查询合约保证金率，对应响应
OnRspQryInstrumentMarginRate。如果InstrumentID填空，则返回持仓
对应的合约保证金率，否则返回相应InstrumentID的保证金率。
目前无法通过一次查询得到所有合约保证金率，如果要查询所
有，则需要通过多次查询得到。


◇ 1.函数原型
virtual int
ReqQryInstrumentMarginRate(CThostFtdcQryInstrumentMarginRateFiel
d *pQryInstrumentMarginRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInstrumentMarginRate：查询合约保证金率
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcInvestUnitIDType
InvestUnitID 投资单
元代码
否
TThostFtdcHedgeFlagType
HedgeFlag
投机套
保标志
是
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcQryInstrumentMarginRateField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
a.HedgeFlag = THOST_FTDC_HF_Speculation;
m_pUserApi->ReqQryInstrumentMarginRate(&a, 1);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryInstrumentOrderCommRate


◇ 1.说明
请求查询报单手续费
响应: OnRspQryInstrumentOrderCommRate
回到顶部


◇ 2.函数原型
virtual int
ReqQryInstrumentOrderCommRate(CThostFtdcQryInstrumentOrderCo
mmRateField *pQryInstrumentOrderCommRate, int nRequestID) = 0;
回到顶部


◇ 3.参数
pQryInstrumentOrderCommRate：报单手续费率查询
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 4.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部




---

## ReqQryQuote

ReqQryQuote
请求查询报价
响应: OnRspQryQuote


◇ 1.函数原型
virtual int ReqQryQuote(CThostFtdcQryQuoteField *pQryQuote,
int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryQuote：报价查询
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcInstrumentIDType
InstrumentID
合约代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcOrderSysIDType
QuoteSysID
报价编
号
是
TThostFtdcTimeType
InsertTimeStart 开始时
间
是
TThostFtdcTimeType
InsertTimeEnd
结束时
间
是
TThostFtdcInvestUnitIDType
InvestUnitID
投资单
元代码
否
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否


nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQrySecAgentACIDMap
请求查询二级代理操作员银期权限
响应: OnRspQrySecAgentACIDMap


◇ 1.函数原型
virtual int
ReqQrySecAgentACIDMap(CThostFtdcQrySecAgentACIDMapField
*pQrySecAgentACIDMap, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySecAgentACIDMap：二级代理操作员银期权限查询
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
是
TThostFtdcUserIDType
UserID
用户代码
是
TThostFtdcAccountIDType
AccountID
资金账户
是
TThostFtdcCurrencyIDType CurrencyID 币种
是
使用二级代理商操作员管理的投资者进行查询，字段值填写正
确时，均无查询结果。
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQrySecAgentCheckMode
请求查询二级代理商资金校验模式
响应: OnRspQrySecAgentCheckMode


◇ 1.函数原型
virtual int
ReqQrySecAgentCheckMode(CThostFtdcQrySecAgentCheckModeField
*pQrySecAgentCheckMode, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySecAgentCheckMode：查询二级代理商资金校验模式
字段类型
字段名称
含义
是否可作为过滤
条件
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
是
TThostFtdcInvestorIDType InvestorID 投资者代
码
是
使用二级代理商操作员管理的投资者进行查询，字段值填写正
确时，有查询结果。
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQrySecAgentTradeInfo
请求查询二级代理商信息
响应: OnRspQrySecAgentTradeInfo


◇ 1.函数原型
virtual int
ReqQrySecAgentTradeInfo(CThostFtdcQrySecAgentTradeInfoField
*pQrySecAgentTradeInfo, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySecAgentTradeInfo：查询二级代理商信息
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
是
TThostFtdcAccountIDType BrokerSecAgentID
境外中介
机构资金
帐号
是
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQrySecAgentTradingAccount
请求查询资金账户
响应: OnRspQrySecAgentTradingAccount


◇ 1.函数原型
virtual int
ReqQrySecAgentTradingAccount(CThostFtdcQryTradingAccountField
*pQryTradingAccount, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryTradingAccount：请求查询资金账户
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
否
TThostFtdcInvestorIDType
InvestorID
投资者代
码
否
TThostFtdcCurrencyIDType CurrencyID 币种代码
否
TThostFtdcAccountIDType
AccountID
业务类型
否
TThostFtdcBizTypeType
BizType
投资者帐
号
否
使用二级代理商操作员管理的投资者进行查询，字段值填写正
确时，均无查询结果。
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
无
回到顶部




---

## ReqQuoteInsert

ReqQuoteInsert
报价录入请求，如果出错，则返回响应OnRspQuoteInsert和
OnErrRtnQuoteInsert；正确则推送OnRtnQuote、OnRtnOrder和
OnRtnTrade。
单边报价和双边报价，都是用一个接口 ReqQuoteInsert。
在单边报价的时候，只需要另一边的数量填0，交易核心就能
区分开。另外，无论是单边还是双边，Ask/BidOrderRef都是要填
的。
除上期所的期货合约使用ReqOrderInsert接口报价，其他交易所
均使用本接口报价。


◇ 1.函数原型
virtual int ReqQuoteInsert(CThostFtdcInputQuoteField
*pInputQuote, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputQuote：输入的报价
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代码必
填
TThostFtdcInvestorIDType
InvestorID
投资者代码
必
填
TThostFtdcInstrumentIDType
InstrumentID
合约代码
必
填
TThostFtdcOrderRefType
QuoteRef
报价引用
无
TThostFtdcUserIDType
UserID
用户代码
无
TThostFtdcBusinessUnitType
BusinessUnit
业务单元
无
TThostFtdcOrderRefType
AskOrderRef
衍生卖报单引
用
选
填
TThostFtdcOrderRefType
BidOrderRef
衍生买报单引
用
选
填
TThostFtdcOrderSysIDType
ForQuoteSysID 应价编号
无
TThostFtdcExchangeIDType
ExchangeID
交易所代码
无
TThostFtdcInvestUnitIDType
InvestUnitID
投资单元代码无
TThostFtdcClientIDType
ClientID
客户代码
无
TThostFtdcIPAddressType
IPAddress
IP地址
无
TThostFtdcMacAddressType
MacAddress
Mac地址
无
TThostFtdcVolumeType
AskVolume
卖数量
必
填
TThostFtdcVolumeType
BidVolume
买数量
必
填


TThostFtdcRequestIDType
RequestID
请求编号
无
TThostFtdcOffsetFlagType
AskOffsetFlag
卖开平标志
必
填
TThostFtdcOffsetFlagType
BidOffsetFlag
买开平标志
必
填
TThostFtdcHedgeFlagType
AskHedgeFlag
卖投机套保标
志
投
机
TThostFtdcHedgeFlagType
BidHedgeFlag
买投机套保标
志
投
机
TThostFtdcPriceType
AskPrice
卖价格
必
填
TThostFtdcPriceType
BidPrice
买价格
必
填
TThostFtdcOldInstrumentIDType reserve1
保留的无效字
段
否
TThostFtdcOldIPAddressType
reserve2
保留的无效字
段
否
TThostFtdcOrderSysIDType
ReplaceSysID
被顶单编号
否
TThostFtdcTimeConditionType
TimeCondition
有效期类型
否
TThostFtdcOrderMemoType
OrderMemo
报单回显字段无
TThostFtdcSequenceNo12Type
SessionReqSeq
session上请求
计数 api自动
维护
无
ForQuoteSysID：询价编号，用于唯一定位一笔询价，需要纯数
字递增
AskOrderRef：需要纯数字递增
BidOrderRef：要比AskOrderRef大，需要纯数字递增


IPAddress：手工填写本机IP地址，不自动获取。填写规则如
下：ipv4原样填写，ipv6要转成非零压缩地址，即原始地址，同时
要去掉冒号，eg：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
OrderMemo:报单回显字段，OrderMemo字段可供终端厂商标记
订单使用，CTP不做处理，即终端填写什么CTP就返回什么
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.示例调用
CThostFtdcInputQuoteField t = { 0 };
strcpy_s(t.BrokerID, "9999");
strcpy_s(t.InvestorID, "1000001");
strcpy_s(t.InstrumentID, "rb1809");
strcpy_s(t.UserID, "1000001"); 
strcpy_s(t.ExchangeID, "SHFE"); 
t.AskPrice = 200;
t.BidPrice = 150;
t.AskVolume = 1;
t.BidVolume = 1;
t.AskOffsetFlag = THOST_FTDC_OF_Open;///卖开平标志
t.BidOffsetFlag = THOST_FTDC_OF_Open;///买开平标志
t.AskHedgeFlag = THOST_FTDC_HF_Hedge;///卖投机套保标志
t.BidHedgeFlag = THOST_FTDC_HF_Hedge;///买投机套保标志
_itoa_s(OrderRef, t.AskOrderRef, 10);///衍生卖报单引用
OrderRef++;
_itoa_s(OrderRef, t.BidOrderRef, 10);///衍生买报单引用
OrderRef++; 
m_pUserApi->ReqQuoteInsert(&t, nRequestID++);
回到顶部


◇ 5.FAQ
询价时报：“没有该合约的做市商”？
这是因为询价合约不对，目前期权合约可以参加询
价。
询价时报：“CTP：当前时间禁止询价”？
这是因为期货公司一般把询价限制时间设置为60秒询
价一次，周期内不能多次询价。
询价时报：“CTP：当前价差禁止询价”？
交易所有询价价差的限制，期货公司可以在柜台上进
行设置，一般如下：
经纪公司代码
合约代码
交易所代码
最新价
价差
1008
SRC
CZCE
0
8
1008
SRC
CZCE
50
10
1008
SRC
CZCE
100
20
1008
SRC
CZCE
200
30
1008
SRC
CZCE
300
50
1008
SRC
CZCE
500
75
询价价差的判断过程
1）看最新价对应于上面的哪一档次，确定价差的最
小值
2）计算买价和卖价的价差，看是否大于设置的价差
（等于也不行）
如果2）通过，那么询价单报入交易所，否则会被CTP
直接拒绝。


“非法的做市商响应”是什么原因？
这可能是所用交易编码非做市商专用所致。
各家交易所第二次报价是否会撤销第一次报价？
中金所：不会撤销
大商所：不会撤销
郑商所：会撤销
上期所：会撤销
各家交易所使用的报价接口是否一样？
中金所：只有期权做市商,使用ReqQuoteInsert
大商所：使用ReqQuoteInsert
郑商所：使用ReqQuoteInsert
上期所：期货合约使用ReqOrderInsert，期权合约使用
ReqQuoteInsert
回到顶部
< 前页 回目录 后页 >


ReqRemoveParkedOrder
请求删除预埋单，对应响应OnRspRemoveParkedOrder。该函数用
于删除已经报入但未触发的某笔预埋报单，注意跟
ReqRemoveParkedOrderAction的区别。


◇ 1.函数原型
virtual int
ReqRemoveParkedOrder(CThostFtdcRemoveParkedOrderField
*pRemoveParkedOrder, int nRequestID) = 0;
回到顶部


◇ 2.参数
pRemoveParkedOrder：删除预埋单
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代码必填
TThostFtdcInvestorIDType
InvestorID
投资者代码
必填
TThostFtdcParkedOrderIDType ParkedOrderID 预埋报单编号必填
TThostFtdcInvestUnitIDType
InvestUnitID
投资单元代码无
ParkedOrderID：对应的预埋报单编号，指定要删除的预埋报
单。
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.示例调用
CThostFtdcRemoveParkedOrderField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.ParkedOrderID, "           5");
m_pUserApi->ReqRemoveParkedOrder(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqRemoveParkedOrderAction
请求删除预埋撤单，对应响应OnRspRemoveParkedOrderAction。
该函数用于删除已经报入但未触发的某笔预埋撤单，注意是预埋撤
单，而不是预埋报单。删除预埋报单使用ReqRemoveParkedOrder。


◇ 1.函数原型
virtual int
ReqRemoveParkedOrderAction(CThostFtdcRemoveParkedOrderAction
Field *pRemoveParkedOrderAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pRemoveParkedOrderAction：删除预埋撤单
字段类型
字段名称
含
义
值
TThostFtdcBrokerIDType
BrokerID
经
纪
公
司
代
码
必
填
TThostFtdcInvestorIDType
InvestorID
投
资
者
代
码
必
填
TThostFtdcParkedOrderActionIDType ParkedOrderActionID
预
埋
撤
单
单
编
号
必
填
TThostFtdcInvestUnitIDType
InvestUnitID
投
资
单
元
无


代
码
ParkedOrderActionID：对应的要删除的预埋撤单编号
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcRemoveParkedOrderActionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.ParkedOrderActionID, "1");  
m_pUserApi->ReqRemoveParkedOrderAction(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqSettlementInfoConfirm
投资者结算结果确认，在开始每日交易前都需要先确认上一日结
算单，只需要确认一次。对应响应OnRspSettlementInfoConfirm。


◇ 1.函数原型
virtual int
ReqSettlementInfoConfirm(CThostFtdcSettlementInfoConfirmField
*pSettlementInfoConfirm, int nRequestID) = 0;
回到顶部


◇ 2.参数
pSettlementInfoConfirm：投资者结算结果确认信息
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
是
TThostFtdcInvestorIDType
InvestorID
投资者代
码
是
TThostFtdcDateType
ConfirmDate 确认日期否
TThostFtdcTimeType
ConfirmTime 确认时间否
TThostFtdcAccountIDType
AccountID
投资者帐
号
否
TThostFtdcCurrencyIDType
CurrencyID
币种代码否
TThostFtdcSettlementIDType SettlementID 结算编号否
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcSettlementInfoConfirmField Confirm = { 0 };
strcpy_s(Confirm.BrokerID, "9999"); 
strcpy_s(Confirm.InvestorID, "1000001");
m_pUserApi->ReqSettlementInfoConfirm(&Confirm, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqTradingAccountPasswordUpdate
资金账户口令更新请求，对应响应
OnRspTradingAccountPasswordUpdate。


◇ 1.函数原型
virtual int
ReqTradingAccountPasswordUpdate(CThostFtdcTradingAccountPasswo
rdUpdateField *pTradingAccountPasswordUpdate, int nRequestID) = 0;
回到顶部




---


