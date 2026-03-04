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




---

## ReqQryInstrumentMarginRate

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




---

## ReqQryInvestUnit

ReqQryInvestUnit
请求查询投资单元，暂不支持此功能
响应: OnRspQryInvestUnit


◇ 1.函数原型
virtual int ReqQryInvestUnit(CThostFtdcQryInvestUnitField
*pQryInvestUnit, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestUnit：查询投资单元
字段类型
字段名称
含义
是否可作为
过滤条件
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

## ReqQryMMInstrumentCommissionRate

ReqQryMMInstrumentCommissionRate
请求查询做市商合约手续费率
响应: OnRspQryMMInstrumentCommissionRate


◇ 1.函数原型
virtual int
ReqQryMMInstrumentCommissionRate(CThostFtdcQryMMInstrument
CommissionRateField *pQryMMInstrumentCommissionRate, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryMMInstrumentCommissionRate：查询做市商合约手续费率
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

## ReqQryMMOptionInstrCommRate

ReqQryMMOptionInstrCommRate
请求查询做市商期权合约手续费
响应: OnRspQryMMOptionInstrCommRate


◇ 1.函数原型
virtual int
ReqQryMMOptionInstrCommRate(CThostFtdcQryMMOptionInstrCom
mRateField *pQryMMOptionInstrCommRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryMMOptionInstrCommRate：做市商期权手续费率查询
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

## ReqQryNotice

ReqQryNotice
请求查询客户通知
响应: OnRspQryNotice


◇ 1.函数原型
virtual int ReqQryNotice(CThostFtdcQryNoticeField *pQryNotice,
int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryNotice：查询客户通知
字段类型
字段名
称
含义
是否可作为过滤
条件
TThostFtdcBrokerIDType BrokerID 经纪公司代
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
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryOptionInstrCommRate

ReqQryOptionInstrCommRate
请求查询期权合约手续费
响应: OnRspQryOptionInstrCommRate


◇ 1.函数原型
virtual int
ReqQryOptionInstrCommRate(CThostFtdcQryOptionInstrCommRateFie
ld *pQryOptionInstrCommRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryOptionInstrCommRate：期权手续费率查询
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
CThostFtdcQryOptionInstrCommRateField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "ag2311C1200");
m_pUserApi->ReqQryOptionInstrCommRate(&a, nRequestID++);
回到顶部


◇ 5.FAQ
合约填空，是否能查询所有期权合约手续费率？
不能，填空只能返回持仓的期权合约手续费率。查询
所有期权合约需要一个个请求查询。
回到顶部
< 前页 回目录 后页 >


ReqQryOptionInstrTradeCost
请求查询期权交易成本，该函数用于查期权保证金，对应响应
OnRspQryOptionInstrTradeCost。
保证金=max(权利金+FixedMargin,MiniMargin)，用户可根据此
公式计算实时保证金。


◇ 1.函数原型
virtual int
ReqQryOptionInstrTradeCost(CThostFtdcQryOptionInstrTradeCostField
*pQryOptionInstrTradeCost, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryOptionInstrTradeCost：期权交易成本查询
字段类型
字段名称
含义
是否可
作为过
滤条件
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
TThostFtdcInstrumentIDType
InstrumentID
合约代码是
TThostFtdcExchangeIDType
ExchangeID
交易所代
码
否
TThostFtdcInvestUnitIDType
InvestUnitID
投资单元
代码
否
TThostFtdcHedgeFlagType
HedgeFlag
投机套保
标志
是
TThostFtdcPriceType
InputPrice
期权合约
报价
是
TThostFtdcPriceType
UnderlyingPrice
标的价
格,填0则
用昨结算
价
是
TThostFtdcOldInstrumentIDType reserve1
保留的无
效字段
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
CThostFtdcQryOptionInstrTradeCostField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
a.HedgeFlag = THOST_FTDC_HF_Speculation;
a.InputPrice = 300; 
m_pUserApi->ReqQryOptionInstrTradeCost(&a, nRequestID++);
回到顶部


◇ 5.FAQ
为什么我用这个接口算出来的保证金跟资金查询得到的
保证金占用不一致？
ReqQryOptionInstrTradeCost计算出的期权保证金跟资
金查询里的期权保证金的计算方式不一样。
ReqQryOptionInstrTradeCost只是估计计算，因为其使
用的公式（保证金=max(权利金+FixedMargin,MiniMargi
n)）中的权利金部分在计算时使用的期权价格是InputPric
e。
而资金查询里的期权保证金计算公式中的期权价格是
使用max算法（max(昨结算，最新价)）得到的。
所以两者存在差别。
回到顶部
< 前页 回目录 后页 >




---

## ReqQryOptionSelfClose

ReqQryOptionSelfClose
请求查询期权自对冲
响应: OnRspQryOptionSelfClose


◇ 1.函数原型
virtual int
ReqQryOptionSelfClose(CThostFtdcQryOptionSelfCloseField
*pQryOptionSelfClose, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryOptionSelfClose：期权自对冲查询
字段类型
字段名称
含义
是否
可作
为过
滤条
件
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
InstrumentID
合约
代码是
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
是
TThostFtdcOrderSysIDType
OptionSelfCloseSysID
期权
自对
冲编
号
是
TThostFtdcTimeType
InsertTimeStart
开始
时间是
TThostFtdcTimeType
InsertTimeEnd
结束
时间是


TThostFtdcOldInstrumentIDType reserve1
保留
的无
效字
段
否
OnRspQryOptionSelfClose：由此能定位一笔期权自对冲的报单
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
CThostFtdcQryOptionSelfCloseField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryOptionSelfClose(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


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




---

## ReqQryProduct

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




---

## ReqQryProductExchRate

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




---

## ReqQryProductGroup

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


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQrySecAgentACIDMap

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




---

## ReqQrySecAgentCheckMode

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


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQrySettlementInfo
请求查询投资者结算结果，对应响应OnRspQrySettlementInfo。可
以查询当天或历史结算单，也可以查询月结算单，但是前提是CTP柜
台生成了相应的日或月结算单。


◇ 1.函数原型
virtual int
ReqQrySettlementInfo(CThostFtdcQrySettlementInfoField
*pQrySettlementInfo, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySettlementInfo：查询投资者结算结果
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
TThostFtdcInvestorIDType
InvestorID
投资者代
码
是
TThostFtdcDateType
TradingDay 交易日
是
TThostFtdcAccountIDType
AccountID
投资者帐
号
否
TThostFtdcCurrencyIDType CurrencyID 币种代码
否
TradingDay：查询某一天的结算单，填写格式为“yyyymmdd”；
查询某一月的结算单，填写格式为“yyyymm”
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
CThostFtdcQrySettlementInfoField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.TradingDay,“20180101”);
m_pUserApi->ReqQrySettlementInfo(&a, nRequestID++);
回到顶部


◇ 5.FAQ
为什么我查不到月结单？
