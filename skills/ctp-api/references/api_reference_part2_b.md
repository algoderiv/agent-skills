---

## ReqFromBankToFutureByFuture

ReqFromBankToFutureByFuture
期货发起银行资金转期货请求
错误响应: OnRspFromBankToFutureByFuture
正确响应: OnRtnFromBankToFutureByFuture


◇ 1.函数原型
virtual int
ReqFromBankToFutureByFuture(CThostFtdcReqTransferField
*pReqTransfer, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqTransfer：转账请求
struct CThostFtdcReqTransferField
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
    ///安装编号
    TThostFtdcInstallIDType InstallID;
    ///期货公司流水号
    TThostFtdcFutureSerialType FutureSerial;
    ///用户标识
    TThostFtdcUserIDType UserID;
    ///验证客户证件号码标志
    TThostFtdcYesNoIndicatorType VerifyCertNoFlag;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
    ///转帐金额
    TThostFtdcTradeAmountType TradeAmount;
    ///期货可取金额
    TThostFtdcTradeAmountType FutureFetchAmount;
    ///费用支付标志
    TThostFtdcFeePayFlagType FeePayFlag;
    ///应收客户费用
    TThostFtdcCustFeeType CustFee;
    ///应收期货公司费用
    TThostFtdcFutureFeeType BrokerFee;
    ///发送方给接收方的消息


    TThostFtdcAddInfoType Message;
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
    ///转账交易状态
    TThostFtdcTransferStatusType TransferStatus;
    ///长客户姓名
    TThostFtdcLongIndividualNameType LongCustomerName;
};
TradeCode：202001 期货发起银行资金转期货。
BankBranchID：为0000
BrokerBranchID：为空


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
CThostFtdcReqTransferField a = { 0 };
strcpy_s(a.TradeCode, "202001");///业务功能码
strcpy_s(a.BankID, "1");
strcpy_s(a.BankBranchID, "0000");///期商代码
strcpy_s(a.BrokerID, "9999"); 
a.LastFragment = THOST_FTDC_LF_Yes;///最后分片标志 '0'=是最
后分片
a.IdCardType = THOST_FTDC_ICT_IDCard;///证件类型
a.CustType = THOST_FTDC_CUSTT_Person;///客户类型
strcpy_s(a.BankAccount, "621485212110187");
strcpy_s(a.AccountID, "1000001");///投资者帐号
strcpy_s(a.Password, "123456");///期货密码--资金密码
a.InstallID = 1;///安装编号
a.FutureSerial = 0;///期货公司流水号
a.VerifyCertNoFlag = THOST_FTDC_YNI_No;///验证客户证件号码
标志
strcpy_s(a.CurrencyID, "CNY");///币种代码
a.TradeAmount = output_num;///转帐金额
a.FutureFetchAmount = 0;///期货可取金额
a.CustFee = 0;///应收客户费用
a.BrokerFee = 0;///应收期货公司费用
a.SecuPwdFlag = THOST_FTDC_BPWDF_BlankCheck;///期货资金
密码核对标志
a.RequestID = 0;///请求编号
a.TID = 0;///交易ID
m_pUserApi->ReqFromBankToFutureByFuture(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqFromFutureToBankByFuture

ReqFromFutureToBankByFuture
期货发起期货资金转银行请求
错误响应: OnRspFromFutureToBankByFuture
正确响应: OnRtnFromFutureToBankByFuture


◇ 1.函数原型
virtual int
ReqFromFutureToBankByFuture(CThostFtdcReqTransferField
*pReqTransfer, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqTransfer：转账请求
struct CThostFtdcReqTransferField
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
    ///安装编号
    TThostFtdcInstallIDType InstallID;
    ///期货公司流水号
    TThostFtdcFutureSerialType FutureSerial;
    ///用户标识
    TThostFtdcUserIDType UserID;
    ///验证客户证件号码标志
    TThostFtdcYesNoIndicatorType VerifyCertNoFlag;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
    ///转帐金额
    TThostFtdcTradeAmountType TradeAmount;
    ///期货可取金额
    TThostFtdcTradeAmountType FutureFetchAmount;
    ///费用支付标志
    TThostFtdcFeePayFlagType FeePayFlag;
    ///应收客户费用
    TThostFtdcCustFeeType CustFee;
    ///应收期货公司费用
    TThostFtdcFutureFeeType BrokerFee;
    ///发送方给接收方的消息


    TThostFtdcAddInfoType Message;
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
    ///转账交易状态
    TThostFtdcTransferStatusType TransferStatus;
    ///长客户姓名
    TThostFtdcLongIndividualNameType LongCustomerName;
};
TradeCode：202002 期货发起期货资金转银行。
BankID：返回银行在期货公司的代码
BankBranchID：填0000
BrokerBranchID：为空


TradeAmount：转账金额
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
CThostFtdcReqTransferField a = { 0 };
strcpy_s(a.TradeCode, "202002");///业务功能码
strcpy_s(a.BankID, "1");
strcpy_s(a.BankBranchID, "0000");///期商代码
strcpy_s(a.BrokerID, "9999"); 
a.LastFragment = THOST_FTDC_LF_Yes;///最后分片标志 '0'=是最
后分片
a.SessionID = SessionID;
a.IdCardType = THOST_FTDC_ICT_IDCard;///证件类型
strcpy_s(a.IdentifiedCardNo, "310110198701011914");///证件号码
strcpy_s(a.BankAccount, "123456789");///银行帐号
strcpy_s(a.AccountID, "1000001");///投资者帐号
strcpy_s(a.Password, "123456");///期货密码
a.InstallID = 1;///安装编号
a.CustType = THOST_FTDC_CUSTT_Person;
a.VerifyCertNoFlag = THOST_FTDC_YNI_No;///验证客户证件号码
标志
strcpy_s(a.CurrencyID, "CNY");///币种代码
a.TradeAmount = output_num;///转帐金额
a.FutureFetchAmount = 0;///期货可取金额
a.CustFee = 0;///应收客户费用
a.BrokerFee = 0;///应收期货公司费用
a.RequestID = 0;///请求编号
a.TID = 0;///交易ID
m_pUserApi->ReqFromFutureToBankByFuture(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqGenUserCaptcha

ReqGenUserCaptcha
用户发出获取图形验证码请求，暂不支持
响应: OnRspGenUserCaptcha


◇ 1.函数原型
virtual int
ReqGenUserCaptcha(CThostFtdcReqGenUserCaptchaField
*pReqGenUserCaptcha, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqGenUserCaptcha：用户发出获取安全安全登陆方法请求
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

## ReqGenUserText

ReqGenUserText
用户发出获取短信验证码请求，暂不支持
响应: OnRspGenUserText


◇ 1.函数原型
virtual int ReqGenUserText(CThostFtdcReqGenUserTextField
*pReqGenUserText, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqGenUserText：用户发出获取安全安全登陆方法请求
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


ReqOptionSelfCloseAction
期权自对冲操作请求、详见期货期权的行权、自对冲
错误响应: OnErrRtnOptionSelfCloseAction，
OnRspOptionSelfCloseAction
正确响应: OnRtnOptionSelfClose


◇ 1.函数原型
virtual int
ReqOptionSelfCloseAction(CThostFtdcInputOptionSelfCloseActionField
*pInputOptionSelfCloseAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputOptionSelfCloseAction：输入期权自对冲操作
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
必填
TThostFtdcInvestorIDType
InvestorID
投
资
者
代
码
必填
TThostFtdcOrderRefType
OptionSelfCloseRef
期
权
自
对
冲
引
用
无
TThostFtdcExchangeIDType
ExchangeID
交
易
所
必填


代
码
TThostFtdcOrderSysIDType
OptionSelfCloseSysID
期
权
自
对
冲
编
号
必填
*1，
需要
对应
要撤
的报
单
TThostFtdcUserIDType
UserID
用
户
代
码
无
TThostFtdcInstrumentIDType
InstrumentID
合
约
代
码
无
TThostFtdcInvestUnitIDType
InvestUnitID
投
资
单
元
代
码
无
TThostFtdcIPAddressType
IPAddress
IP
地
址
无
TThostFtdcMacAddressType
MacAddress
Mac
地
址
无


TThostFtdcOrderActionRefType
OptionSelfCloseActionRef
期
权
自
对
冲
操
作
引
用
无
TThostFtdcRequestIDType
RequestID
请
求
编
号
无
TThostFtdcFrontIDType
FrontID
前
置
编
号
无
TThostFtdcSessionIDType
SessionID
会
话
编
号
无
TThostFtdcActionFlagType
ActionFlag
操
作
标
志
必填
TThostFtdcOldInstrumentIDType reserve1
保
留
的
无
否


效
字
段
TThostFtdcOldIPAddressType
reserve2
保
留
的
无
效
字
段
否
OptionSelfCloseRef：对应要撤销的期权自对冲的引用
FrontID：对应要撤销的期权自对冲的前置编号
SessionID：对应要撤销的期权自对冲的会话编号
ExchangeID：对应要撤销的期权自对冲的交易所编号
ActionFlag：支持删除，不支持修改
InstrumentID：对应要撤销的期权自对冲的合约代码
IPAddress：手工填写本机IP地址，不自动获取。填写规则如下：
ipv4原样填写，ipv6要转成非零压缩地址，即原始地址，同时要去掉
冒号，eg：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
nRequestID：请求ID，对应响应里的nRequestID，无递增规则，
由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcInputOptionSelfCloseActionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001"); 
strcpy_s(a.OptionSelfCloseRef, "000000258");//期权自对冲引用
a.FrontID = 1;
a.SessionID = 6442531;
strcpy_s(a.ExchangeID, "SHFE");
a.ActionFlag = THOST_FTDC_AF_Delete;
strcpy_s(a.UserID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
m_pUserApi->ReqOptionSelfCloseAction(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---


