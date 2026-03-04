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





