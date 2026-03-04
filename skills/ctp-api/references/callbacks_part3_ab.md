    TThostFtdcMoneyType Interest;
    ///入金金额
    TThostFtdcMoneyType Deposit;
    ///出金金额
    TThostFtdcMoneyType Withdraw;
    ///冻结的保证金
    TThostFtdcMoneyType FrozenMargin;
    ///冻结的资金
    TThostFtdcMoneyType FrozenCash;
    ///冻结的手续费


    TThostFtdcMoneyType FrozenCommission;
    ///当前保证金总额
    TThostFtdcMoneyType CurrMargin;
    ///资金差额
    TThostFtdcMoneyType CashIn;
    ///手续费
    TThostFtdcMoneyType Commission;
    ///平仓盈亏
    TThostFtdcMoneyType CloseProfit;
    ///持仓盈亏
    TThostFtdcMoneyType PositionProfit;
    ///期货结算准备金
    TThostFtdcMoneyType Balance;
    ///可用资金
    TThostFtdcMoneyType Available;
    ///可取资金
    TThostFtdcMoneyType WithdrawQuota;
    ///基本准备金
    TThostFtdcMoneyType Reserve;
    ///交易日
    TThostFtdcDateType TradingDay;
    ///结算编号
    TThostFtdcSettlementIDType SettlementID;
    ///信用额度
    TThostFtdcMoneyType Credit;
    ///质押金额
    TThostFtdcMoneyType Mortgage;
    ///交易所保证金
    TThostFtdcMoneyType ExchangeMargin;
    ///投资者交割保证金
    TThostFtdcMoneyType DeliveryMargin;
    ///交易所交割保证金
    TThostFtdcMoneyType ExchangeDeliveryMargin;
    ///保底期货结算准备金


    TThostFtdcMoneyType ReserveBalance;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
    ///上次货币质入金额
    TThostFtdcMoneyType PreFundMortgageIn;
    ///上次货币质出金额
    TThostFtdcMoneyType PreFundMortgageOut;
    ///货币质入金额
    TThostFtdcMoneyType FundMortgageIn;
    ///货币质出金额
    TThostFtdcMoneyType FundMortgageOut;
    ///货币质押余额
    TThostFtdcMoneyType FundMortgageAvailable;
    ///可质押货币金额--已废弃
    TThostFtdcMoneyType MortgageableFund;
    ///特殊产品占用保证金--已废弃
    TThostFtdcMoneyType SpecProductMargin;
    ///特殊产品冻结保证金--已废弃 
    TThostFtdcMoneyType SpecProductFrozenMargin;
    ///特殊产品手续费--已废弃
    TThostFtdcMoneyType SpecProductCommission;
    ///特殊产品冻结手续费--已废弃
    TThostFtdcMoneyType SpecProductFrozenCommission;
    ///特殊产品持仓盈亏--已废弃
    TThostFtdcMoneyType SpecProductPositionProfit;
    ///特殊产品平仓盈亏--已废弃
    TThostFtdcMoneyType SpecProductCloseProfit;
    ///根据持仓盈亏算法计算的特殊产品持仓盈亏--已废弃
    TThostFtdcMoneyType SpecProductPositionProfitByAlg;
    ///特殊产品交易所保证金--已废弃
    TThostFtdcMoneyType SpecProductExchangeMargin;
    ///业务类型
    TThostFtdcBizTypeType BizType;
    ///延时换汇冻结金额


    TThostFtdcMoneyType FrozenSwap;
    ///剩余换汇额度
    TThostFtdcMoneyType RemainSwap;
};
ReserveBalance：最低权益标准，对应柜台菜单“投资者最低权
益设置”
Reserve：基础保证金，对应柜台菜单“投资者基础保证金设置”
PreCredit：上日资金冻结，为负值
Credit：当日资金冻结，为负值。交易系统初始化后会将
PreCredit赋值给Credit，因此Credit包含了上日资金冻结和当日资金
冻结。
PreBalance：昨权益
Balance：权益
FrozenCash：冻结权利金
CashIn：权利金
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。


bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
期货品种的套保仓、投机仓、套利仓的单向大边是单独
计算，还是一起计算的？对于能源中心INE目前的交易细
则是不是也有单向大边的规则？
单向大边目前只有上期所和中金所有此项业务。对于
上期所，同品种的买卖双边分别计算，取买卖双边中保证
金金额较大的一边单向实时收取。
对于中金所，套利、套保和投机的交易编码不同，计
算的时候首先是区分交易编码的，每个交易编码下同品种
的买卖双边再分别计算取大边。
对于能源中心，sc目前是单向大边的规则。
关于冻结手续费计算的问题：比如我有大商所的持仓
i1709 10手昨仓，这个时候我平仓2手挂单了，请问此时
的平仓手续费率是采用平昨手续费还是平今手续费，还
是这两个费率取较大的？
短线开平仓合约的平仓（不管平仓还是平今）在成交
的时候是按平今手续费收取的，在冻结的时候是按平仓手
续费收取。
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryTradingCode

OnRspQryTradingCode
请求查询交易编码响应，当执行ReqQryTradingCode后，该方法被
调用。


◇ 1.函数原型
virtual void OnRspQryTradingCode(CThostFtdcTradingCodeField
*pTradingCode, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pTradingCode：交易编码
struct CThostFtdcTradingCodeField
{
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///交易所代码
    TThostFtdcExchangeIDType ExchangeID;
    ///客户代码
    TThostFtdcClientIDType ClientID;
    ///是否活跃
    TThostFtdcBoolType IsActive;
    ///交易编码类型
    TThostFtdcClientIDTypeType ClientIDType;
    ///营业部编号
    TThostFtdcBranchIDType BranchID;
    ///业务类型
    TThostFtdcBizTypeType BizType;
    ///投资单元代码
    TThostFtdcInvestUnitIDType InvestUnitID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息


    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryTradingNotice

OnRspQryTradingNotice
请求查询交易通知响应，当执行ReqQryTradingNotice后，该方法
被调用。


◇ 1.函数原型
virtual void
OnRspQryTradingNotice(CThostFtdcTradingNoticeField
*pTradingNotice, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pTradingNotice：用户事件通知
struct CThostFtdcTradingNoticeField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者范围
    TThostFtdcInvestorRangeType InvestorRange;
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///序列系列号
    TThostFtdcSequenceSeriesType SequenceSeries;
    ///用户代码
    TThostFtdcUserIDType UserID;
    ///发送时间
    TThostFtdcTimeType SendTime;
    ///序列号
    TThostFtdcSequenceNoType SequenceNo;
    ///消息正文
    TThostFtdcContentType FieldContent;
    ///投资单元代码
    TThostFtdcInvestUnitIDType InvestUnitID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息


    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryTransferBank

OnRspQryTransferBank
请求查询转帐银行响应，当执行ReqQryTransferBank后，该方法
被调用。


◇ 1.函数原型
virtual void OnRspQryTransferBank(CThostFtdcTransferBankField
*pTransferBank, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pTransferBank：转帐银行
struct CThostFtdcTransferBankField
{
    ///银行代码
    TThostFtdcBankIDType BankID;
    ///银行分中心代码
    TThostFtdcBankBrchIDType BankBrchID;
    ///银行名称
    TThostFtdcBankNameType BankName;
    ///是否活跃
    TThostFtdcBoolType IsActive;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryTransferSerial

OnRspQryTransferSerial
请求查询转帐流水响应，当执行ReqQryTransferSerial后，该方法
被调用。


◇ 1.函数原型
virtual void
OnRspQryTransferSerial(CThostFtdcTransferSerialField
*pTransferSerial, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pTransferSerial：银期转账交易流水表
struct CThostFtdcTransferSerialField
{
    ///平台流水号
    TThostFtdcPlateSerialType PlateSerial;
    ///交易发起方日期
    TThostFtdcTradeDateType TradeDate;
    ///交易日期
    TThostFtdcDateType TradingDay;
    ///交易时间
    TThostFtdcTradeTimeType TradeTime;
    ///交易代码
    TThostFtdcTradeCodeType TradeCode;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///银行编码
    TThostFtdcBankIDType BankID;
    ///银行分支机构编码
    TThostFtdcBankBrchIDType BankBranchID;
    ///银行帐号类型
    TThostFtdcBankAccTypeType BankAccType;
    ///银行帐号
    TThostFtdcBankAccountType BankAccount;
    ///银行流水号
    TThostFtdcBankSerialType BankSerial;
    ///期货公司编码
    TThostFtdcBrokerIDType BrokerID;
    ///期商分支机构代码
    TThostFtdcFutureBranchIDType BrokerBranchID;
    ///期货公司帐号类型


    TThostFtdcFutureAccTypeType FutureAccType;
    ///投资者帐号
    TThostFtdcAccountIDType AccountID;
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///期货公司流水号
    TThostFtdcFutureSerialType FutureSerial;
    ///证件类型
    TThostFtdcIdCardTypeType IdCardType;
    ///证件号码
    TThostFtdcIdentifiedCardNoType IdentifiedCardNo;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
    ///交易金额
    TThostFtdcTradeAmountType TradeAmount;
    ///应收客户费用
    TThostFtdcCustFeeType CustFee;
    ///应收期货公司费用
    TThostFtdcFutureFeeType BrokerFee;
    ///有效标志
    TThostFtdcAvailabilityFlagType AvailabilityFlag;
    ///操作员
    TThostFtdcOperatorCodeType OperatorCode;
    ///新银行帐号
    TThostFtdcBankAccountType BankNewAccount;
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
BankBranchID：填0000
BrokerBranchID：填0000


AvailabilityFlag：是否转账成功，不成功则CTP会自动发起冲正
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQueryBankAccountMoneyByFuture

OnRspQueryBankAccountMoneyByFut
ure
期货发起查询银行余额应答，当执行
ReqQueryBankAccountMoneyByFuture后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQueryBankAccountMoneyByFuture(CThostFtdcReqQueryAccou
ntField *pReqQueryAccount, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
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
TradeCode: 204002 期货发起查询银行余额
BankBranchID：填0000
BrokerBranchID：一般为空
BankPassWord：为密文，显示“*”
Password：为密文，显示“*”
InstallID：CTP内部使用
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};


nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQueryCFMMCTradingAccountToken

OnRspQueryCFMMCTradingAccountT
oken
请求查询监控中心用户令牌，当执行
ReqQueryCFMMCTradingAccountToken后，该方法被调用。


◇ 1.函数原型
virtual void
OnRspQueryCFMMCTradingAccountToken(CThostFtdcQueryCFMMC
TradingAccountTokenField *pQueryCFMMCTradingAccountToken,
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pQueryCFMMCTradingAccountToken：查询监控中心用户令牌
struct CThostFtdcQueryCFMMCTradingAccountTokenField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType InvestUnitID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQuoteAction

OnRspQuoteAction
报价操作请求响应，当执行ReqQuoteAction后有字段填写不对之
类的CTP报错则通过此接口返回。


◇ 1.函数原型
virtual void OnRspQuoteAction(CThostFtdcInputQuoteActionField
*pInputQuoteAction, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInputQuoteAction：输入报价操作
struct CThostFtdcInputQuoteActionField
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
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
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
    ///报单回显字段
    TThostFtdcOrderMemoType OrderMemo;
    ///session上请求计数 api自动维护
    TThostFtdcSequenceNo12Type  SessionReqSeq;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQuoteInsert

OnRspQuoteInsert
报价录入请求响应，当执行ReqQuoteInsert后有字段填写不对之类
的CTP报错则通过此接口返回。


◇ 1.函数原型
virtual void OnRspQuoteInsert(CThostFtdcInputQuoteField
*pInputQuote, CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool
bIsLast) {};
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
ForQuoteSysID：指定响应的是市场上哪一笔询价


pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspRemoveParkedOrder

OnRspRemoveParkedOrder
删除预埋单响应，当执行ReqRemoveParkedOrder后，该方法被调
用。


◇ 1.函数原型
virtual void
OnRspRemoveParkedOrder(CThostFtdcRemoveParkedOrderField
*pRemoveParkedOrder, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pRemoveParkedOrder：删除预埋单
struct CThostFtdcRemoveParkedOrderField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///预埋报单编号
    TThostFtdcParkedOrderIDType ParkedOrderID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType InvestUnitID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID 由用户在操作请求
时指定。
bIsLast：指示该次返回是否为针对nRequestID的最后一次返
回。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >





