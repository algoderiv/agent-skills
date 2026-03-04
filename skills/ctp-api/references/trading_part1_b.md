---

## ReqQrySecAgentTradeInfo

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




---

## ReqQrySecAgentTradingAccount

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




---

## ReqQrySettlementInfo

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
这有可能是CTP系统里没有生成你的月结算单。CTP
的日结算单是需要每天结算的时候业务人员去点击生成
的，月结算单也是需要定期生成的。
回到顶部
< 前页 回目录 后页 >




---

## ReqQrySettlementInfoConfirm

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




---

## ReqQryTrade

ReqQryTrade
请求查询成交
响应: OnRspQryTrade


◇ 1.函数原型
virtual int ReqQryTrade(CThostFtdcQryTradeField *pQryTrade, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryTrade：查询成交
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
TThostFtdcTradeIDType
TradeID
成交编
号
是
TThostFtdcTimeType
TradeTimeStart 开始时
间
是
TThostFtdcTimeType
TradeTimeEnd
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

## ReqQryTradingAccount

ReqQryTradingAccount
请求查询资金账户，
响应: OnRspQryTradingAccount


◇ 1.函数原型
virtual int
ReqQryTradingAccount(CThostFtdcQryTradingAccountField
*pQryTradingAccount, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryTradingAccount：查询资金账户
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
TThostFtdcCurrencyIDType CurrencyID 币种代码
是
TThostFtdcAccountIDType
AccountID
投资者帐
号
否
TThostFtdcBizTypeType
BizType
业务类型
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
CThostFtdcQryTradingAccountField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.CurrencyID, "CNY");
m_pUserApi->ReqQryTradingAccount(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryTradingCode
请求查询交易编码
响应: OnRspQryTradingCode


◇ 1.函数原型
virtual int ReqQryTradingCode(CThostFtdcQryTradingCodeField
*pQryTradingCode, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryTradingCode：查询交易编码
字段类型
字段名称
含义
是否可作为
过滤条件
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
TThostFtdcClientIDType
ClientID
客户代
码
是
TThostFtdcInvestUnitIDType InvestUnitID 投资单
元代码
否
TThostFtdcClientIDTypeType ClientIDType 交易编
码类型
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


ReqQryTradingNotice
请求查询交易通知
响应: OnRspQryTradingNotice


◇ 1.函数原型
virtual int
ReqQryTradingNotice(CThostFtdcQryTradingNoticeField
*pQryTradingNotice, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryTradingNotice：查询交易事件通知
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


ReqQryTransferBank
请求查询转帐银行
响应: OnRspQryTransferBank


◇ 1.函数原型
virtual int ReqQryTransferBank(CThostFtdcQryTransferBankField
*pQryTransferBank, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryTransferBank：查询转帐银行
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcBankIDType
BankID
银行代码
是
TThostFtdcBankBrchIDType BankBrchID 银行分中
心代码
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


ReqQryTransferSerial
请求查询转帐流水。
响应：OnRspQryTransferSerial


◇ 1.函数原型
virtual int ReqQryTransferSerial(CThostFtdcQryTransferSerialField
*pQryTransferSerial, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryTransferSerial：请求查询转帐流水
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
TThostFtdcAccountIDType
AccountID
投资者帐
号
是
TThostFtdcBankIDType
BankID
银行代码
是
TThostFtdcCurrencyIDType CurrencyID 币种代码
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
CThostFtdcQryTransferSerialField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.AccountID, "1000001"); 
strcpy_s(a.BankID, “1”);
strcpy_s(a.CurrencyID, "CNY");
m_pUserApi->ReqQryTransferSerial(&a, nRequestID++);
回到顶部


◇ 5.FAQ
我在柜台上做了手工出入金，通过这个接口查不到？
ReqQryTransferSerial不能查柜台手工出入金，只能查
询到期货发起期货转银行或者银行转期货的内容。
回到顶部
< 前页 回目录 后页 >




---

## ReqSettlementInfoConfirm

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




---

## ReqTradingAccountPasswordUpdate

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




---

## ReqQryRiskSettleInvstPosition

ReqQryRiskSettleInvstPosition
投资者风险结算持仓查询，对应响应请求
OnRspQryRiskSettleInvstPosition
详见 6.6.1P1版本更新说明


◇ 1.函数原型
virtual int
ReqQryRiskSettleInvstPosition(CThostFtdcQryRiskSettleInvstPositionFi
eld *pQryRiskSettleInvstPosition, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRiskSettleInvstPosition：投资者风险结算持仓查询
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
TThostFtdcInstrumentIDType InstrumentID 合约代码否
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


ReqQryRiskSettleProductStatus
风险结算产品查询，对应响应请求
OnRspQryRiskSettleProductStatus
详见 6.6.1P1版本更新说明


◇ 1.函数原型
virtual int
ReqQryRiskSettleProductStatus(CThostFtdcQryRiskSettleProductStatus
Field *pQryRiskSettleProductStatus, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRiskSettleProductStatus：风险结算产品查询
字段类型
字段名称
含义
是否可作为过滤
条件
TThostFtdcInstrumentIDType ProductID 产品代
码
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


