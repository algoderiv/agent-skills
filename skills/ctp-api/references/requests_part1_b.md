---

## ReqQryExecOrder

ReqQryExecOrder
请求查询执行宣告
响应: OnRspQryExecOrder


◇ 1.函数原型
virtual int ReqQryExecOrder(CThostFtdcQryExecOrderField
*pQryExecOrder, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExecOrder：执行宣告查询
字段类型
字段名称
含义
是否可
作为过
滤条件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码是
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
TThostFtdcExecOrderSysIDType ExecOrderSysID 报单编
号
是
TThostFtdcTimeType
InsertTimeStart
开始时
间
是
TThostFtdcTimeType
InsertTimeEnd
结束时
间
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
CThostFtdcQryExecOrderField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryExecOrder(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


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




---

## ReqQryInstrumentOrderCommRate

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


◇ 5.调用示例
CThostFtdcQryInstrumentOrderCommRateField a = { 0 };
strcpy(a.BrokerID, "9999");
strcpy(a.InvestorID, "1000001");
strcpy(a.InstrumentID, "rb1809");
m_pUserApi->ReqQryInstrumentOrderCommRate(&a, 1);
回到顶部


◇ 6.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryInvestor
请求查询投资者
响应：OnRspQryInvestor


◇ 1.函数原型
virtual int ReqQryInvestor(CThostFtdcQryInvestorField
*pQryInvestor, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestor：查询投资者
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
CThostFtdcQryInvestorField a = { 0 };
strcpy(a.BrokerID, "9999");
strcpy(a.InvestorID, "1000001");
m_pUserApi->ReqQryInvestor(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryInvestorPosition
请求查询投资者持仓，对应响应OnRspQryInvestorPosition。CTP
系统将持仓明细记录按合约，持仓方向，开仓日期（仅针对上期所和
能源所，区分昨仓、今仓）进行汇总。


◇ 1.函数原型
virtual int
ReqQryInvestorPosition(CThostFtdcQryInvestorPositionField
*pQryInvestorPosition, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPosition：查询投资者持仓
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
CThostFtdcQryInvestorPositionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");//不填写合约则返回所有持仓
m_pUserApi->ReqQryInvestorPosition(&a, 1);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryInvestorPositionCombineDetail
请求查询投资者持仓明细
响应: OnRspQryInvestorPositionCombineDetail


◇ 1.函数原型
virtual int
ReqQryInvestorPositionCombineDetail(CThostFtdcQryInvestorPosition
CombineDetailField *pQryInvestorPositionCombineDetail, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPositionCombineDetail：查询组合持仓明细
字段类型
字段名称
含义
是否可
作为过
滤条件
TThostFtdcBrokerIDType
BrokerID
经纪
公司
代码
是
TThostFtdcInvestorIDType
InvestorID
投资
者代
码
是
TThostFtdcInstrumentIDType
CombInstrumentID
组合
持仓
合约
编码
是
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
否
TThostFtdcInvestUnitIDType
InvestUnitID
投资
单元
代码
否
TThostFtdcOldInstrumentIDType reserve1
保留
的无
效字
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
大商所下套利单成交后查询接口没有找到持仓，是什么
原因呢？
因为柜台开启了大商所rule新型组合保证金算法，就
没有套利持仓了。
回到顶部
< 前页 回目录 后页 >


ReqQryInvestorPositionDetail
请求查询投资者持仓明细，对应响应
OnRspQryInvestorPositionDetail。
CTP系统根据来自交易所的成交记录生成持仓明细记录，一笔成
交记录对应一条持仓明细记录。


◇ 1.函数原型
virtual int
ReqQryInvestorPositionDetail(CThostFtdcQryInvestorPositionDetailFiel
d *pQryInvestorPositionDetail, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPositionDetail：查询投资者持仓明细
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
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部




---

## ReqQryOrder

ReqQryOrder
请求查询报单
响应: OnRspQryOrder


◇ 1.函数原型
virtual int ReqQryOrder(CThostFtdcQryOrderField *pQryOrder, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryOrder：查询报单
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
OrderSysID
报单编
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
CThostFtdcQryOrderField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryOrder(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryParkedOrder

ReqQryParkedOrder
请求查询预埋单
响应: OnRspQryParkedOrder


◇ 1.函数原型
virtual int ReqQryParkedOrder(CThostFtdcQryParkedOrderField
*pQryParkedOrder, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryParkedOrder：查询预埋单
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




---

## ReqQryParkedOrderAction

ReqQryParkedOrderAction
请求查询预埋撤单
响应: OnRspQryParkedOrderAction


◇ 1.函数原型
virtual int
ReqQryParkedOrderAction(CThostFtdcQryParkedOrderActionField
*pQryParkedOrderAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryParkedOrderAction：查询预埋撤单
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


ReqQryProduct
请求查询产品
响应: OnRspQryProduct


◇ 1.函数原型
virtual int ReqQryProduct(CThostFtdcQryProductField
*pQryProduct, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryProduct：查询产品
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcInstrumentIDType
ProductID
产品代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcProductClassType
ProductClass 产品类
型
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
CThostFtdcQryProductField a = { 0 };
strcpy_s(a.ProductID, "sc");
a.ProductClass = THOST_FTDC_PC_Futures;
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryProduct(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryProductExchRate
请求查询产品报价汇率
响应: OnRspQryProductExchRate


◇ 1.函数原型
virtual int
ReqQryProductExchRate(CThostFtdcQryProductExchRateField
*pQryProductExchRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryProductExchRate：产品报价汇率查询
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcInstrumentIDType
ProductID
产品代
码
是
TThostFtdcExchangeIDType
ExchangeID 交易所
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


ReqQryProductGroup
请求查询产品组
响应: OnRspQryProductGroup


◇ 1.函数原型
virtual int ReqQryProductGroup(CThostFtdcQryProductGroupField
*pQryProductGroup, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryProductGroup：查询产品组
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcInstrumentIDType
ProductID
产品代
码
是
TThostFtdcExchangeIDType
ExchangeID 交易所
代码
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




---

## ReqQueryBankAccountMoneyByFuture

ReqQueryBankAccountMoneyByFuture
期货发起查询银行余额请求
错误响应: OnRspQueryBankAccountMoneyByFuture
正确响应: OnRtnQueryBankBalanceByFuture


◇ 1.函数原型
virtual int
ReqQueryBankAccountMoneyByFuture(CThostFtdcReqQueryAccountF
ield *pReqQueryAccount, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqQueryAccount：查询账户信息请求
struct CThostFtdcReqQueryAccountField
{
    ///业务功能码
    TThostFtdcTradeCodeType TradeCode;
    ///银行代码
    TThostFtdcBankIDType BankID;
    ///银行分支机构代码
    TThostFtdcBankBrchIDType BankBranchID;
    ///期商代码
    TThostFtdcBrokerIDType BrokerID;
    ///期商分支机构代码
    TThostFtdcFutureBranchIDType BrokerBranchID;
    ///交易日期
    TThostFtdcTradeDateType TradeDate;
    ///交易时间
    TThostFtdcTradeTimeType TradeTime;
    ///银行流水号
    TThostFtdcBankSerialType BankSerial;
    ///交易系统日期 
    TThostFtdcTradeDateType TradingDay;
    ///银期平台消息流水号
    TThostFtdcSerialType PlateSerial;
    ///最后分片标志
    TThostFtdcLastFragmentType LastFragment;
    ///会话号
    TThostFtdcSessionIDType SessionID;
    ///客户姓名
    TThostFtdcIndividualNameType CustomerName;
    ///证件类型


    TThostFtdcIdCardTypeType IdCardType;
    ///证件号码
    TThostFtdcIdentifiedCardNoType IdentifiedCardNo;
    ///客户类型
    TThostFtdcCustTypeType CustType;
    ///银行帐号
    TThostFtdcBankAccountType BankAccount;
    ///银行密码
    TThostFtdcPasswordType BankPassWord;
    ///投资者帐号
    TThostFtdcAccountIDType AccountID;
    ///期货密码
    TThostFtdcPasswordType Password;
    ///期货公司流水号
    TThostFtdcFutureSerialType FutureSerial;
    ///安装编号
    TThostFtdcInstallIDType InstallID;
    ///用户标识
    TThostFtdcUserIDType UserID;
    ///验证客户证件号码标志
    TThostFtdcYesNoIndicatorType VerifyCertNoFlag;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
    ///摘要
    TThostFtdcDigestType Digest;
    ///银行帐号类型
    TThostFtdcBankAccTypeType BankAccType;
    ///渠道标志
    TThostFtdcDeviceIDType DeviceID;
    ///期货单位帐号类型
    TThostFtdcBankAccTypeType BankSecuAccType;
    ///期货公司银行编码
    TThostFtdcBankCodingForFutureType BrokerIDByBank;
    ///期货单位帐号


    TThostFtdcBankAccountType BankSecuAcc;
    ///银行密码标志
    TThostFtdcPwdFlagType BankPwdFlag;
    ///期货资金密码核对标志
    TThostFtdcPwdFlagType SecuPwdFlag;
    ///交易柜员
    TThostFtdcOperNoType OperNo;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///交易ID
    TThostFtdcTIDType TID;
    ///长客户姓名
    TThostFtdcLongIndividualNameType LongCustomerName;
};
TradeCode：204002 期货发起查询银行余额
BankBranchID：填0000
InstallID：ctp内部使用，不用填
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
Brokerid:必填项
InvestorID:必填项
UserID:必填项
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

## ReqQueryCFMMCTradingAccountToken

ReqQueryCFMMCTradingAccountToke
n
请求查询监控中心用户令牌，对应响应
OnRspQueryCFMMCTradingAccountToken、
OnRtnCFMMCTradingAccountToken。


◇ 1.函数原型
virtual int
ReqQueryCFMMCTradingAccountToken(CThostFtdcQueryCFMMCTra
dingAccountTokenField *pQueryCFMMCTradingAccountToken, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQueryCFMMCTradingAccountToken：查询监控中心用户令牌
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
TThostFtdcInvestUnitIDType InvestUnitID 投资单元
代码
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


