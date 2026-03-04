---

## ReqQryTradingCode

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




---

## ReqQryTradingNotice

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




---

## ReqQryTransferBank

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




---

## ReqQryTransferSerial

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




---

## ReqUserAuthMethod

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


ReqUserLoginWithText
用户发出带有短信验证码的登陆请求，暂不支持
响应: OnRspUserLogin


◇ 1.函数原型
virtual int
ReqUserLoginWithText(CThostFtdcReqUserLoginWithTextField
*pReqUserLoginWithText, int nRequestID) = 0;
回到顶部




---

## ReqUserLogout

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




---

## ReqUserPasswordUpdate

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




---

## SubmitUserSystemInfo

SubmitUserSystemInfo
上报用户终端信息，用于中继服务器操作员登录模式，操作员登
录后，可以多次调用该接口上报不同客户信息。


◇ 1.函数原型
virtual int SubmitUserSystemInfo(CThostFtdcUserSystemInfoField
*pUserSystemInfo) = 0;
回到顶部


◇ 2.参数
pUserSystemInfo：用户系统信息
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪
公司
代码
必
填
TThostFtdcUserIDType
UserID
用户
代码
必
填
TThostFtdcClientSystemInfoType
ClientSystemInfo
用户
端系
统内
部信
息
必
填
TThostFtdcIPAddressType
ClientPublicIP
终端IP
地址
必
填
TThostFtdcTimeType
ClientLoginTime
登录
成功
时间
必
填
TThostFtdcAppIDType
ClientAppID
App代
码
必
填
TThostFtdcSystemInfoLenType
ClientSystemInfoLen
用户
端系
统内
部信
息长
度
必
填


TThostFtdcIPPortType
ClientIPPort
终端IP
端口
必
填
TThostFtdcOldIPAddressType
reserve1
保留
的无
效字
段
无
TThostFtdcClientLoginRemarkType ClientLoginRemark
客户
登录
备注2
无
UserID：此处应该填终端的投资者账号，即InvestorID，而非操
作员账号。
ClientSystemInfoLen：加密后的用户终端系统内部信息的长度
ClientSystemInfo：加密后的用户终端系统内部信息。
ClientPublicIP：用户终端IP，由中继服务器采集和填写
ClientLoginTime：用户登录中继时间，由中继服务器采集和填
写
ClientAppid：用户终端的appid，由中继服务器采集和填写
回到顶部


◇ 3.返回
0 正确
-1 字段长度不对
-2 非CTP采集的终端信息
-4 当前用户非一对多操作员
-5 字段中存在非法字符或者长度超限
-6 采集结果字段错误
-7 采集库的版本类型和生产库的不一致
回到顶部


◇ 4.调用示例
//一对多中继终端使用方法
Step 1  中继在启动后，在API连接后发起认证
Authenticate();
Step 2  认证成功后发起登录
UserLogin();
Step 3  终端登录中继时，中继上报用户终端信息
char pSystemInfo[344];
  int len;
  CTP_GetSystemInfo(pSystemInfo, len);
  CThostFtdcUserSystemInfoField field1;
  memset(&field1, 0, sizeof(field));
  strcpy(field1.BrokerID, "9999");
  strcpy(field1.UserID, "00001");
  memcpy(field1.ClientSystemInfo, pSystemInfo, len);
  field1.ClientSystemInfoLen = len;
  strcpy(field1.ClientPublicIP, "127.0.0.1");
  field1.ClientIPPort = 65535;
  strcpy(field1.ClientLoginTime, "11:28:28");
  strcpy(field1.ClientAppID, "Q7");
  m_pUserApi->SubmitUserSystemInfo(&field1);
回到顶部


◇ 5.FAQ
采集信息上报时候总是提示operation not permitted，这
是为什么？
如果提示operation not permitted，可能是AppID类型错
误。例如，直连模式的AppID，却错误调用了SubmitUserS
ystemInfo。
此错误不会通过特定接口返回，只在标准输出中提
示，例如直接在屏幕上打印出来。
不上报采集信息，会影响登录吗？
不影响登录，CTP不做控制，但这样不符合监管要
求。
一对多模式下，如果认证成功后，密码输错导致登录失
败了，是否要退出登录，重新认证和登录？
要退出登录，重新认证和登录。否则继续登录的话会
导致submitusersysinfo调用失败！
回到顶部
< 前页 回目录 后页 >




---

## SubscribePrivateTopic

SubscribePrivateTopic
订阅私有流。该方法要在Init 方法前调用。若不调用则默认按照
restart模式订阅。推荐使用THOST_TERT_RESTART方式订阅私有流。


◇ 1.函数原型
virtual void SubscribePrivateTopic(THOST_TE_RESUME_TYPE
nResumeType) = 0;
回到顶部


◇ 2.参数
nResumeType： 私有流重传方式
THOST_TERT_RESTART:从本交易日开始重传（推荐）
THOST_TERT_RESUME:从上次收到的续传
序号保存在本地流水文件里，若遇到CTP交易系统清流重
启，如果此时不删除原流水文件并继续用RESUME模式接入，
则可能收不到私有流回报
THOST_TERT_QUICK:只传送登录后私有流的内容
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
pUserApi->SubscribePrivateTopic(THOST_TERT_RESTART);
pUserApi->SubscribePublicTopic(THOST_TERT_RESTART);
pUserApi->RegisterFront(“tcp://127.0.0.1:51205”);
pUserApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## SubscribePublicTopic

SubscribePublicTopic
订阅公共流。该方法要在Init 方法前调用。若不调用，默认
RESTART模式订阅。


◇ 1.函数原型
virtual void SubscribePublicTopic(THOST_TE_RESUME_TYPE
nResumeType) = 0;
回到顶部


◇ 2.参数
nResumeType：私有流重传方式。
THOST_TERT_RESTART：从本交易日开始重传
THOST_TERT_RESUME：从上次收到的续传
THOST_TERT_QUICK：只传送登录后私有流的内容
THOST_TERT_NONE：取消订阅公有流
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
pUserApi->SubscribePrivateTopic(THOST_TERT_RESUME);
pUserApi->SubscribePublicTopic(THOST_TERT_RESUME);
pUserApi->RegisterFront("tcp://127.0.0.1:51205");
pUserApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryClassifiedInstrument

ReqQryClassifiedInstrument
请求查询分类合约，对应响应请求OnRspQryClassifiedInstrument
详见 6.5.1版本更新说明补充说明


◇ 1.函数原型
virtual int
ReqQryClassifiedInstrument(CThostFtdcQryClassifiedInstrumentField
*pQryClassifiedInstrument, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryClassifiedInstrument：查询分类合约
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
否
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcExchangeInstIDType ExchangeInstID
合约在
交易所
的代码
否
TThostFtdcInstrumentIDType
ProductID
产品代
码
否
TThostFtdcTradingTypeType
TradingType
合约交
易状态
是
TThostFtdcClassTypeType
ClassType
合约分
类类型
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


