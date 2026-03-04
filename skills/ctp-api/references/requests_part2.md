# Requests - CTP 6.7.8 API

本节包含 26 个主题。


## ReqQryMaxOrderVolume

ReqQryMaxOrderVolume
查询最大报单数量请求，对应响应OnRspQryMaxOrderVolume。
虽然ReqQryMaxOrderVolume可以查询可开，但是交易核心在计算
的时候是没有算手续费的，所以不完全准，计算逻辑是按照昨结算价
计算的可开；如果需要精确结果的，建议自行计算。另外，可平的查
询是已经排除了冻结持仓的。


◇ 1.函数原型
virtual int
ReqQryMaxOrderVolume(CThostFtdcQryMaxOrderVolumeField
*pQryMaxOrderVolume, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryMaxOrderVolume：查询最大报单数量
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
TThostFtdcVolumeType
MaxVolume
最大允
许报单
数量
否
TThostFtdcDirectionType
Direction
买卖方
向
是
TThostFtdcOffsetFlagType
OffsetFlag
开平标
志
是
TThostFtdcHedgeFlagType
HedgeFlag
投机套
保标志
是
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
否


段
MaxVolume：取值结果为，min（可开/可平，限价单最大下单
量）；可开根据昨结算价计算得出。
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
CThostFtdcQryMaxOrderVolumeField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
a.Direction = THOST_FTDC_D_Buy;
a.OffsetFlag = THOST_FTDC_OF_Open;
a.HedgeFlag = THOST_FTDC_HF_Speculation;
a.MaxVolume = 1;
m_pUserApi->ReqQryMaxOrderVolume(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQuoteAction

ReqQuoteAction
报价操作请求，用于撤销报价
错误响应:OnRspQuoteAction OnErrRtnQuoteAction
正确响应:OnRtnQuote OnRtnOrder


◇ 1.函数原型
virtual int ReqQuoteAction(CThostFtdcInputQuoteActionField
*pInputQuoteAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputQuoteAction：输入报价操作
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代
码
必
填
*2
TThostFtdcInvestorIDType
InvestorID
投资者代码
必
填
*2
TThostFtdcOrderRefType
QuoteRef
报价引用
必
填
*1
TThostFtdcExchangeIDType
ExchangeID
交易所代码
必
填
*2
TThostFtdcOrderSysIDType
QuoteSysID
报价编号
必
填
*2
TThostFtdcUserIDType
UserID
用户代码
无
TThostFtdcInstrumentIDType
InstrumentID
合约代码
必
填
*1
TThostFtdcInvestUnitIDType
InvestUnitID
投资单元代
码
无
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
TThostFtdcOrderActionRefType
QuoteActionRef 报价操作引
用
无
TThostFtdcRequestIDType
RequestID
请求编号
无
TThostFtdcFrontIDType
FrontID
前置编号
必
填
*1
TThostFtdcSessionIDType
SessionID
会话编号
必
填
*1
TThostFtdcActionFlagType
ActionFlag
操作标志
必
填
TThostFtdcOldInstrumentIDType reserve1
保留的无效
字段
否
TThostFtdcOldIPAddressType
reserve2
保留的无效
字段
否
TThostFtdcOrderMemoType
OrderMemo
报单回显字
段
无
TThostFtdcSequenceNo12Type
SessionReqSeq
session上请
求计数 api自
动维护
无
必填*1、必填*2：两组选一组必填，能对应要撤的报价单。
FrontID：对应要撤销的报价的前置编号
SessionID：对应要撤销的报价的会话编号
QuoteSysID: 对应要撤销的报价的编号
ActionFlag：操作标志，支持删除，目前不支持改单。想实现改
单操作可以先撤销之后再重新报单。


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


◇ 4.调用示例
CThostFtdcInputQuoteActionField t = { 0 };
strcpy_s(t.BrokerID, "9999");
strcpy_s(t.InvestorID, "1000001");
strcpy_s(t.UserID, "1000001");
strcpy_s(t.ExchangeID, "SHFE");
strcpy_s(t.QuoteRef, "           1");
t.FrontID = 1;
t.SessionID = 6442531;
t.ActionFlag = THOST_FTDC_AF_Delete;
strcpy_s(t.InstrumentID, "rb1809");
m_pUserApi->ReqQuoteAction(&t, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


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




---

## ReqRemoveParkedOrder

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




---

## ReqRemoveParkedOrderAction

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


◇ 2.参数
pTradingAccountPasswordUpdate：资金账户口令变更域
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代码
必填
TThostFtdcAccountIDType
AccountID
投资者帐号
必填
TThostFtdcPasswordType
OldPassword
原来的口令
必填
TThostFtdcPasswordType
NewPassword
新的口令
必填
TThostFtdcCurrencyIDType
CurrencyID
币种代码
必填
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
CThostFtdcTradingAccountPasswordUpdateField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.AccountID, "1000001");
strcpy_s(a.OldPassword, "123456");
strcpy_s(a.NewPassword, "666666");
strcpy_s(a.CurrencyID, "CNY");
m_pUserApi->ReqTradingAccountPasswordUpdate(&a, 
nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqUserAuthMethod
查询用户当前支持的认证模式，暂不支持
响应: OnRspUserAuthMethod


◇ 1.函数原型
virtual int
ReqUserAuthMethod(CThostFtdcReqUserAuthMethodField
*pReqUserAuthMethod, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqUserAuthMethod：用户发出获取安全安全登陆方法请求
字段类型
字段名称
含义
值
TThostFtdcDateType
TradingDay
交易日
无
TThostFtdcBrokerIDType
BrokerID
经纪公司代码
无
TThostFtdcUserIDType
UserID
用户代码
无
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

## ReqUserLogin

ReqUserLogin
用户登录请求，对应响应OnRspUserLogin。
接口包含采集函数不是线程安全的，不同线程同步调用建议加
锁。


◇ 1.函数原型
virtual int ReqUserLogin(CThostFtdcReqUserLoginField
*pReqUserLoginField, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqUserLoginField：用户登录请求
字段类型
字段名称
含义
值
TThostFtdcDateType
TradingDay
交易日
无
TThostFtdcBrokerIDType
BrokerID
经纪公司代
码
必
填
TThostFtdcUserIDType
UserID
用户代码
必
填
TThostFtdcPasswordType
Password
密码
必
填
TThostFtdcProductInfoType
UserProductInfo
用户端产品
信息
无
TThostFtdcProductInfoType
InterfaceProductInfo 接口端产品
信息
无
TThostFtdcProtocolInfoType
ProtocolInfo
协议信息
无
TThostFtdcMacAddressType
MacAddress
Mac地址
无
TThostFtdcPasswordType
OneTimePassword
动态密码
无
TThostFtdcOldIPAddressType reserve1
保留的无效
字段
无
TThostFtdcIPAddressType
ClientIPAddress
终端IP地址无
TThostFtdcLoginRemarkType LoginRemark
登录备注
无
TThostFtdcIPPortType
ClientIPPort
终端IP端口无
UserID：操作员代码，之后的investorid需要属于该操作员管理
下。


UserProductInfo：客户端的产品信息，如软件开发商、版本号
等。
用户事件中的UserProductInfo取决于认证填写的值，而非登
录。
例如：SFITTraderV100。
LoginRemark：可以写登录备注，能够被交易系统的日志查询
到。
OneTimePassword：如果期货公司启用了动态口令，客户会有
一个电子口令牌，把生成的值，输入到登录报文里的
OneTimePassword(动态密码)这个字段中去即可
ClientIPAddress：CTP前置主动获取，填写无效。
MacAddress：API采集，填写无效。
LoginRemark：在注备中：逗号，引号，“\r”，“\n”。四种符号
在风控用户事件中会被过滤。
IPAddress：系统自动获取，填写无效。
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
CThostFtdcReqUserLoginField reqUserLogin = { 0 };
strcpy_s(reqUserLogin.BrokerID, “0000”);
strcpy_s(reqUserLogin.UserID, “00001”);
strcpy_s(reqUserLogin.Password, “123456”); 
m_pUserApi->ReqUserLogin(&reqUserLogin, nRequestID++);
回到顶部


◇ 5.FAQ
现在投资者登录的时候，有时候会被锁定，请问会有什
么原因？
CTP有单独锁IP的功能。被锁定的情况下继续登录会
报“CTP：登录失败次数超限，IP被禁止”。目前这个阈值
一般设置为5000，单个交易日内累积计算。
CTP有锁IP+账号的功能。被锁定的情况下继续登录会
报“CTP：连续登录失败次数超限，登录被禁止”。目前这
个阈值一般设置为6-10，注意这里的登录错误次数CTP是
连续计算的。
具体的阈值以期货公司设置的为准。
首次登陆如果提示修改密码，如何修改？
首次登陆如果提示修改密码，则必须在当前会话调用
ReqUserPasswordUpdate来修改密码。
“CTP:用户在线会话超出上限”是什么意思？
表示该UserID的同时在线会话数超出了期货公司设置
的阈值，具体阈值咨询期货公司柜台设置。
登录报“CTP：重复的登录”，是什么原因？
正常情况下，收到登录成功的响应后再发起登录，是
没有响应的，这是因为如果前置收到登陆成功的响应，则
会过滤后续的相同Session的登陆请求。
极端情况下，当前置还没收到核心返回的登录成功响
应，而此时又再次收到来自客户端的登录请求，就不会过


滤，核心收到重复的登录请求后就会返回“CTP：重复的登
录”的错误响应。
登录时候报错：“CTP：连续登录失败数超限，登录被禁
止”，为什么？
此报错说明用户连续输错密码被禁止登录了，ip被锁
定。此时用户换ip或者联系期货公司解锁即可。
回到顶部
< 前页 回目录 后页 >




---

## ReqUserLoginWithCaptcha

ReqUserLoginWithCaptcha
用户发出带有图片验证码的登陆请求，暂不支持
响应: OnRspUserLogin


◇ 1.函数原型
virtual int
ReqUserLoginWithCaptcha(CThostFtdcReqUserLoginWithCaptchaField
*pReqUserLoginWithCaptcha, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqUserLoginWithCaptcha：用户发出带图形验证码的登录请
求请求
struct CThostFtdcReqUserLoginWithCaptchaField
{
///交易日
TThostFtdcDateType  TradingDay;
///经纪公司代码
TThostFtdcBrokerIDType  BrokerID;
///用户代码
TThostFtdcUserIDType    UserID;
///密码
TThostFtdcPasswordType  Password;
///用户端产品信息
TThostFtdcProductInfoType   UserProductInfo;
///接口端产品信息
TThostFtdcProductInfoType   InterfaceProductInfo;
///协议信息
TThostFtdcProtocolInfoType  ProtocolInfo;
///Mac地址
TThostFtdcMacAddressType    MacAddress;
///保留的无效字段
TThostFtdcOldIPAddressType  reserve1;
///登录备注
TThostFtdcLoginRemarkType   LoginRemark;
///图形验证码的文字内容
TThostFtdcPasswordType  Captcha;
///终端IP端口
TThostFtdcIPPortType    ClientIPPort;
///终端IP地址


TThostFtdcIPAddressType ClientIPAddress;
};
ClientIPAddress：填写规则如下：ipv4原样填写，ipv6要转成非
零压缩地址，即原始地址，同时要去掉冒号，eg：
AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
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

## ReqUserLoginWithOTP

ReqUserLoginWithOTP
用户发出带有动态口令的登陆请求，暂不支持
响应: OnRspUserLogin


◇ 1.函数原型
virtual int
ReqUserLoginWithOTP(CThostFtdcReqUserLoginWithOTPField
*pReqUserLoginWithOTP, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqUserLoginWithOTP：用户发出带动态验证码的登录请求请
求
struct CThostFtdcReqUserLoginWithOTPField
{
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///密码
    TThostFtdcPasswordType  Password;
    ///用户端产品信息
    TThostFtdcProductInfoType   UserProductInfo;
    ///接口端产品信息
    TThostFtdcProductInfoType   InterfaceProductInfo;
    ///协议信息
    TThostFtdcProtocolInfoType  ProtocolInfo;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve1;
    ///登录备注
    TThostFtdcLoginRemarkType   LoginRemark;
    ///OTP密码
    TThostFtdcPasswordType  OTPPassword;
    ///终端IP端口
    TThostFtdcIPPortType    ClientIPPort;
    ///终端IP地址


    TThostFtdcIPAddressType ClientIPAddress;
};
ClientIPAddress：填写规则如下：ipv4原样填写，ipv6要转成非
零压缩地址，即原始地址，同时要去掉冒号，eg：
AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
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

## ReqUserLoginWithText

ReqUserLoginWithText
用户发出带有短信验证码的登陆请求，暂不支持
响应: OnRspUserLogin


◇ 1.函数原型
virtual int
ReqUserLoginWithText(CThostFtdcReqUserLoginWithTextField
*pReqUserLoginWithText, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqUserLoginWithText：用户发出带短信验证码的登录请求请
求
struct CThostFtdcReqUserLoginWithTextField
{
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///密码
    TThostFtdcPasswordType  Password;
    ///用户端产品信息
    TThostFtdcProductInfoType   UserProductInfo;
    ///接口端产品信息
    TThostFtdcProductInfoType   InterfaceProductInfo;
    ///协议信息
    TThostFtdcProtocolInfoType  ProtocolInfo;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve1;
    ///登录备注
    TThostFtdcLoginRemarkType   LoginRemark;
    ///短信验证码文字内容
    TThostFtdcPasswordType  Text;
    ///终端IP端口
    TThostFtdcIPPortType    ClientIPPort;
    ///终端IP地址


    TThostFtdcIPAddressType ClientIPAddress;
};
ClientIPAddress：填写规则如下：ipv4原样填写，ipv6要转成非
零压缩地址，即原始地址，同时要去掉冒号，eg：
AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
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


ReqUserLogout
登出请求，对应响应OnRspUserLogout。


◇ 1.函数原型
virtual int ReqUserLogout(CThostFtdcUserLogoutField
*pUserLogout, int nRequestID) = 0;
回到顶部


◇ 2.参数
pUserLogout：用户登出请求
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代码
必填
TThostFtdcUserIDType
UserID
用户代码
必填
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
CThostFtdcUserLogoutField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.UserID, "1000001");
m_pUserApi->ReqUserLogout(&a, nRequestID++);
回到顶部


◇ 5.FAQ
用户调用ReqUserLogout后是否会自动重连？
会，Logout后，触发OnFrontDisconnected，能自动On
FrontConnected。
回到顶部
< 前页 回目录 后页 >


ReqUserPasswordUpdate
用户口令更新请求，对应响应OnRspUserPasswordUpdate。


◇ 1.函数原型
virtual int
ReqUserPasswordUpdate(CThostFtdcUserPasswordUpdateField
*pUserPasswordUpdate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pUserPasswordUpdate：用户口令变更
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代码
必填
TThostFtdcUserIDType
UserID
用户代码
必填
TThostFtdcPasswordType
OldPassword
原来的口令
必填
TThostFtdcPasswordType
NewPassword
新的口令
必填
NewPassword：新密码需要符合复杂度要求，不然修改失败。
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
CThostFtdcUserPasswordUpdateField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.UserID, "1000001");
strcpy_s(a.OldPassword, "123456");
strcpy_s(a.NewPassword, "666666");
m_pUserApi->ReqUserPasswordUpdate(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


SubmitUserSystemInfo
上报用户终端信息，用于中继服务器操作员登录模式，操作员登
录后，可以多次调用该接口上报不同客户信息。




---


