这有可能是CTP系统里没有生成你的月结算单。CTP
的日结算单是需要每天结算的时候业务人员去点击生成
的，月结算单也是需要定期生成的。
回到顶部
< 前页 回目录 后页 >


ReqQrySettlementInfoConfirm
请求查询结算信息确认
响应: OnRspQrySettlementInfoConfirm


◇ 1.函数原型
virtual int
ReqQrySettlementInfoConfirm(CThostFtdcQrySettlementInfoConfirmFi
eld *pQrySettlementInfoConfirm, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySettlementInfoConfirm：查询结算信息确认域
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
TThostFtdcAccountIDType
AccountID
投资者帐
号
否
TThostFtdcCurrencyIDType CurrencyID 币种代码
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





