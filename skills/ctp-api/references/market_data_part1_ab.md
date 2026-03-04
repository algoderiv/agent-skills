TThostFtdcClientIDType  ClientID;
///业务单元
TThostFtdcBusinessUnitType  BusinessUnit;
///报单操作状态
TThostFtdcOrderActionStatusType OrderActionStatus;
///用户代码
TThostFtdcUserIDType    UserID;
///状态信息
TThostFtdcErrorMsgType  StatusMsg;
///保留的无效字段
TThostFtdcOldInstrumentIDType   reserve1;
///营业部编号
TThostFtdcBranchIDType  BranchID;
///投资单元代码
TThostFtdcInvestUnitIDType  InvestUnitID;
///保留的无效字段
TThostFtdcOldIPAddressType  reserve2;
///Mac地址
TThostFtdcMacAddressType    MacAddress;
///合约代码
TThostFtdcInstrumentIDType  InstrumentID;
///IP地址
TThostFtdcIPAddressType IPAddress;
};
ActionFlag：只有删除标志，修改标志位不支持


InstallID：CTP内部使用
StatusMsg：报错信息
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >


OnErrRtnOptionSelfCloseInsert
期权自对冲录入错误回报，当执行ReqOptionSelfCloseInsert后有
字段填写不对之类的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void
OnErrRtnOptionSelfCloseInsert(CThostFtdcInputOptionSelfCloseField
*pInputOptionSelfClose, CThostFtdcRspInfoField *pRspInfo) {};
回到顶部


◇ 2.参数
pInputOptionSelfClose：输入的期权自对冲
struct CThostFtdcInputOptionSelfCloseField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///期权自对冲引用
    TThostFtdcOrderRefType  OptionSelfCloseRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///数量
    TThostFtdcVolumeType    Volume;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///期权行权的头寸是否自对冲
    TThostFtdcOptSelfCloseFlagType  OptSelfCloseFlag;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///资金账号
    TThostFtdcAccountIDType AccountID;
    ///币种代码


    TThostFtdcCurrencyIDType    CurrencyID;
    ///交易编码
    TThostFtdcClientIDType  ClientID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve2;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///IP地址
    TThostFtdcIPAddressType IPAddress;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >


OnErrRtnOrderAction
报单操作错误回报，当执行ReqOrderAction后有字段填写不对之
类的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void OnErrRtnOrderAction(CThostFtdcOrderActionField
*pOrderActio

---

## OnErrRtnQuoteAction

OnErrRtnQuoteAction
报价操作错误回报, 当执行ReqQuoteAction后有字段填写不对之类
的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void OnErrRtnQuoteAction(CThostFtdcQuoteActionField
*pQuoteAction, CThostFtdcRspInfoField *pRspInfo) {};
回到顶部


◇ 2.参数
pQuoteAction：报价操作
struct CThostFtdcQuoteActionField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///报价操作引用
    TThostFtdcOrderActionRefType    QuoteActionRef;
    ///报价引用
    TThostFtdcOrderRefType  QuoteRef;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///报价操作编号
    TThostFtdcOrderSysIDType    QuoteSysID;
    ///操作标志
    TThostFtdcActionFlagType    ActionFlag;
    ///操作日期
    TThostFtdcDateType  ActionDate;
    ///操作时间
    TThostFtdcTimeType  ActionTime;
    ///交易所交易员代码
    TThostFtdcTraderIDType  TraderID;
    ///安装编号


    TThostFtdcInstallIDType InstallID;
    ///本地报价编号
    TThostFtdcOrderLocalIDType  QuoteLocalID;
    ///操作本地编号
    TThostFtdcOrderLocalIDType  ActionLocalID;
    ///会员代码
    TThostFtdcParticipantIDType ParticipantID;
    ///客户代码
    TThostFtdcClientIDType  ClientID;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///报单操作状态
    TThostFtdcOrderActionStatusType OrderActionStatus;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///状态信息
    TThostFtdcErrorMsgType  StatusMsg;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///营业部编号
    TThostFtdcBranchIDType  BranchID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve2;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///IP地址
    TThostFtdcIPAddressType IPAddress;
    ///报单回显字段
    TThostFtdcOrderMemoType OrderMemo;
    ///session上请求计数 api自动维护


    TThostFtdcSequenceNo12Type  SessionReqSeq;
};
OrderActionStatus：报价撤销的状态信息，此处返回的是错误信
息
StatusMsg：报价单最后的状态
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnErrRtnQuoteInsert

OnErrRtnQuoteInsert
报价录入错误回报，当执行ReqQuoteInsert后有字段填写不对之类
的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void OnErrRtnQuoteInsert(CThostFtdcInputQuoteField
*pInputQuote, CThostFtdcRspInfoField *pRspInfo) {};
回到顶部


◇ 2.参数
pInputQuote：输入的报价
struct CThostFtdcInputQuoteField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///报价引用
    TThostFtdcOrderRefType  QuoteRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///卖价格
    TThostFtdcPriceType AskPrice;
    ///买价格
    TThostFtdcPriceType BidPrice;
    ///卖数量
    TThostFtdcVolumeType    AskVolume;
    ///买数量
    TThostFtdcVolumeType    BidVolume;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///卖开平标志
    TThostFtdcOffsetFlagType    AskOffsetFlag;
    ///买开平标志
    TThostFtdcOffsetFlagType    BidOffsetFlag;
    ///卖投机套保标志


    TThostFtdcHedgeFlagType AskHedgeFlag;
    ///买投机套保标志
    TThostFtdcHedgeFlagType BidHedgeFlag;
    ///衍生卖报单引用
    TThostFtdcOrderRefType  AskOrderRef;
    ///衍生买报单引用
    TThostFtdcOrderRefType  BidOrderRef;
    ///应价编号
    TThostFtdcOrderSysIDType    ForQuoteSysID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///交易编码
    TThostFtdcClientIDType  ClientID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve2;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///IP地址
    TThostFtdcIPAddressType IPAddress;
    ///被顶单编号
    TThostFtdcOrderSysIDType    ReplaceSysID;
    ///有效期类型
    TThostFtdcTimeConditionType TimeCondition;
    ///报单回显字段
    TThostFtdcOrderMemoType OrderMemo;
    ///session上请求计数 api自动维护
    TThostFtdcSequenceNo12Type  SessionReqSeq;
};
ForQuoteSysID：应价编号对应市场上询价的编号


pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >


OnErrRtnRepealBankToFutureByFutur
eManual
系统运行时期货端手工发起冲正银行转期货错误回报。发起冲正
后系统的报错回报，由于发起冲正属于ctp内部操作，没有req接口


◇ 1.函数原型
virtual void
OnErrRtnRepealBankToFutureByFutureManual(CThostFtdcReqRepealF
ield *pReqRepeal, CThostFtdcRspInfoField *pRspInfo) {};
回到顶部


◇ 2.参数
pReqRepeal：冲正请求
struct CThostFtdcReqRepealField
{
    ///冲正时间间隔
    TThostFtdcRepealTimeIntervalType RepealTimeInterval;
    ///已经冲正次数
    TThostFtdcRepealedTimesType RepealedTimes;
    ///银行冲正标志
    TThostFtdcBankRepealFlagType BankRepealFlag;
    ///期商冲正标志
    TThostFtdcBrokerRepealFlagType BrokerRepealFlag;
    ///被冲正平台流水号
    TThostFtdcPlateSerialType PlateRepealSerial;
    ///被冲正银行流水号
    TThostFtdcBankSerialType BankRepealSerial;
    ///被冲正期货流水号
    TThostFtdcFutureSerialType FutureRepealSerial;
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
TradeCode：203001 期货发起冲正银行转期货
BankID：银行在ctp对应的代码
BankBranchID：填0000
BrokerBranchID：一般为空
BankPassWord：返回银行的密码，都是“*”密码不显示
Password：返回期货账户的密码，都是“*”密码不显示
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >


OnErrRtnRepealFutureToBankByFutur
eManual
系统运行时期货端手工发起冲正期货转银行错误回报, 发起冲正后
系统的报错回报，由于发起冲正属于ctp内部操作，没有req接口


◇ 1.函数原型
virtual void
OnErrRtnRepealFutureToBankByFutureManual(CThostFtdcReqRepealF
ield *pReqRepeal, CThostFtdcRspInfoField *pRspInfo) {};
回到顶部


◇ 2.参数
pReqRepeal：冲正请求
struct CThostFtdcReqRepealField
{
    ///冲正时间间隔
    TThostFtdcRepealTimeIntervalType RepealTimeInterval;
    ///已经冲正次数
    TThostFtdcRepealedTimesType RepealedTimes;
    ///银行冲正标志
    TThostFtdcBankRepealFlagType BankRepealFlag;
    ///期商冲正标志
    TThostFtdcBrokerRepealFlagType BrokerRepealFlag;
    ///被冲正平台流水号
    TThostFtdcPlateSerialType PlateRepealSerial;
    ///被冲正银行流水号
    TThostFtdcBankSerialType BankRepealSerial;
    ///被冲正期货流水号
    TThostFtdcFutureSerialType FutureRepealSerial;
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


---

## ReqForQuoteInsert

ReqForQuoteInsert
询价录入请求
错误响应: OnErrRtnForQuoteInsert，OnRspForQuoteInsert
正确响应: OnRtnForQuoteRsp


◇ 1.函数原型
virtual int ReqForQuoteInsert(CThostFtdcInputForQuoteField
*pInputForQuote, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputForQuote：输入的询价
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
必填
TThostFtdcInvestorIDType
InvestorID
投资者代
码
必填
TThostFtdcInstrumentIDType
InstrumentID 合约代码
期权合
约名称
TThostFtdcOrderRefType
ForQuoteRef 询价引用
选填
TThostFtdcUserIDType
UserID
用户代码
无
TThostFtdcExchangeIDType
ExchangeID
交易所代
码
无
TThostFtdcInvestUnitIDType
InvestUnitID 投资单元
代码
无
TThostFtdcIPAddressType
IPAddress
IP地址
无
TThostFtdcMacAddressType
MacAddress
Mac地址
无
TThostFtdcOldInstrumentIDType reserve1
保留的无
效字段
否
ForQuoteRef：需要纯数字递增，不填则ctp自动填写
IPAddress：手工填写本机IP地址，不自动获取。填写规则如
下：ipv4原样填写，ipv6要转成非零压缩地址，即原始地址，同时
要去掉冒号，eg：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
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
CThostFtdcInputForQuoteField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.UserID, "1000001");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqForQuoteInsert(&a, nRequestID++);
回到顶部


◇ 5.FAQ
询价时报：“没有该合约的做市商”？
询价合约不对。
回到顶部
< 前页 回目录 后页 >


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


◇ 3.返


