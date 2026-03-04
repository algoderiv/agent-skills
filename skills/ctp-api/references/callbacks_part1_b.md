---

## OnRspFromFutureToBankByFuture

OnRspFromFutureToBankByFuture
期货发起期货资金转银行应答，当执行
ReqFromFutureToBankByFuture后有字段填写不对之类的CTP报错则通
过此接口返回。


◇ 1.函数原型
virtual void
OnRspFromFutureToBankByFuture(CThostFtdcReqTransferField
*pReqTransfer, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
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
TradeCode：202002 期货发起期货资金转银行
BankID：返回银行在期货公司的代码
BankBranchID：填0000
BrokerBranchID：一般为空


CustomerName：客户姓名
BankPassWord：返回银行的密码，都是“*”密码不显示
Password：返回期货账户的密码，都是“*”密码不显示
TransferStatus：转账交易的最后状态
TradeAmount：转账金额
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

## OnRspGenUserCaptcha

OnRspGenUserCaptcha
获取图形验证码请求的回复


◇ 1.函数原型
virtual void
OnRspGenUserCaptcha(CThostFtdcRspGenUserCaptchaField
*pRspGenUserCaptcha, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pRspGenUserCaptcha：生成的图片验证码信息
struct CThostFtdcRspGenUserCaptchaField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///用户代码
    TThostFtdcUserIDType UserID;
    ///图片信息长度
    TThostFtdcCaptchaInfoLenType CaptchaInfoLen;
    ///图片信息
    TThostFtdcCaptchaInfoType CaptchaInfo;
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

## OnRspGenUserText

OnRspGenUserText
获取短信验证码请求的回复


◇ 1.函数原型
virtual void OnRspGenUserText(CThostFtdcRspGenUserTextField
*pRspGenUserText, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pRspGenUserText：短信验证码生成的回复息
struct CThostFtdcRspGenUserTextField
{
    ///短信验证码序号
    TThostFtdcUserTextSeqType UserTextSeq;
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

## OnRspOptionSelfCloseAction

OnRspOptionSelfCloseAction
期权自对冲操作请求响应，当执行ReqOptionSelfCloseAction后，
该方法被调用。


◇ 1.函数原型
virtual void
OnRspOptionSelfCloseAction(CThostFtdcInputOptionSelfCloseActionF
ield *pInputOptionSelfCloseAction, CThostFtdcRspInfoField *pRspInfo,
int nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInputOptionSelfCloseAction：输入期权自对冲操作
struct CThostFtdcInputOptionSelfCloseActionField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///期权自对冲操作引用
    TThostFtdcOrderActionRefType    OptionSelfCloseActionRef;
    ///期权自对冲引用
    TThostFtdcOrderRefType  OptionSelfCloseRef;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///期权自对冲操作编号
    TThostFtdcOrderSysIDType    OptionSelfCloseSysID;
    ///操作标志
    TThostFtdcActionFlagType    ActionFlag;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
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

## OnRspOptionSelfCloseInsert

OnRspOptionSelfCloseInsert
期权自对冲录入请求响应，当执行ReqOptionSelfCloseInsert后，
该方法被调用


◇ 1.函数原型
virtual void
OnRspOptionSelfCloseInsert(CThostFtdcInputOptionSelfCloseField
*pInputOptionSelfClose, CThostFtdcRspInfoField *pRspInfo, , bool
bIsLast) {};
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

## OnRspOrderAction

OnRspOrderAction
报单操作请求响应，当执行ReqOrderAction后有字段填写不对之
类的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void OnRspOrderAction(CThostFtdcInputOrderActionField
*pInputOrderAction, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInputOrderAction：输入报单操作
struct CThostFtdcInputOrderActionField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///报单操作引用
    TThostFtdcOrderActionRefType    OrderActionRef;
    ///报单引用
    TThostFtdcOrderRefType  OrderRef;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///报单编号
    TThostFtdcOrderSysIDType    OrderSysID;
    ///操作标志
    TThostFtdcActionFlagType    ActionFlag;
    ///价格
    TThostFtdcPriceType LimitPrice;
    ///数量变化
    TThostFtdcVolumeType    VolumeChange;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///保留的无效字段


    TThostFtdcOldInstrumentIDType   reserve1;
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
ActionFlag：目前只有删除（撤单）的操作，修改（改单）的操
作还没有，可以通过撤单之后重新报单实现。
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

## OnRspOrderInsert

OnRspOrderInsert
报单录入请求响应，当执行ReqOrderInsert后有字段填写不对之类
的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void OnRspOrderInsert(CThostFtdcInputOrderField
*pInputOrder, CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool
bIsLast) {};
回到顶部


◇ 2.参数
pInputOrder：输入报单
struct CThostFtdcInputOrderField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///报单引用
    TThostFtdcOrderRefType  OrderRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///报单价格条件
    TThostFtdcOrderPriceTypeType    OrderPriceType;
    ///买卖方向
    TThostFtdcDirectionType Direction;
    ///组合开平标志
    TThostFtdcCombOffsetFlagType    CombOffsetFlag;
    ///组合投机套保标志
    TThostFtdcCombHedgeFlagType CombHedgeFlag;
    ///价格
    TThostFtdcPriceType LimitPrice;
    ///数量
    TThostFtdcVolumeType    VolumeTotalOriginal;
    ///有效期类型
    TThostFtdcTimeConditionType TimeCondition;
    ///GTD日期
    TThostFtdcDateType  GTDDate;
    ///成交量类型


    TThostFtdcVolumeConditionType   VolumeCondition;
    ///最小成交量
    TThostFtdcVolumeType    MinVolume;
    ///触发条件
    TThostFtdcContingentConditionType   ContingentCondition;
    ///止损价
    TThostFtdcPriceType StopPrice;
    ///强平原因
    TThostFtdcForceCloseReasonType  ForceCloseReason;
    ///自动挂起标志
    TThostFtdcBoolType  IsAutoSuspend;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///用户强平标志
    TThostFtdcBoolType  UserForceClose;
    ///互换单标志
    TThostFtdcBoolType  IsSwapOrder;
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

## OnRspParkedOrderAction

OnRspParkedOrderAction
预埋撤单录入请求响应，当执行ReqParkedOrderAction后，该方法
被调用


◇ 1.函数原型
virtual void
OnRspParkedOrderAction(CThostFtdcParkedOrderActionField
*pParkedOrderAction, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pParkedOrderAction：输入预埋单操作
struct CThostFtdcParkedOrderActionField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///报单操作引用
    TThostFtdcOrderActionRefType    OrderActionRef;
    ///报单引用
    TThostFtdcOrderRefType  OrderRef;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///报单编号
    TThostFtdcOrderSysIDType    OrderSysID;
    ///操作标志
    TThostFtdcActionFlagType    ActionFlag;
    ///价格
    TThostFtdcPriceType LimitPrice;
    ///数量变化
    TThostFtdcVolumeType    VolumeChange;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///保留的无效字段


    TThostFtdcOldInstrumentIDType   reserve1;
    ///预埋撤单单编号
    TThostFtdcParkedOrderActionIDType   ParkedOrderActionID;
    ///用户类型
    TThostFtdcUserTypeType  UserType;
    ///预埋撤单状态
    TThostFtdcParkedOrderStatusType Status;
    ///错误代码
    TThostFtdcErrorIDType   ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType  ErrorMsg;
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
Status：预埋单的最后情况
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

## OnRspParkedOrderInsert

OnRspParkedOrderInsert
预埋单录入请求响应，当执行ReqParkedOrderInsert后，该方法被
调用


◇ 1.函数原型
virtual void OnRspParkedOrderInsert(CThostFtdcParkedOrderField
*pParkedOrder, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pParkedOrder：预埋单
struct CThostFtdcParkedOrderField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///报单引用
    TThostFtdcOrderRefType  OrderRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///报单价格条件
    TThostFtdcOrderPriceTypeType    OrderPriceType;
    ///买卖方向
    TThostFtdcDirectionType Direction;
    ///组合开平标志
    TThostFtdcCombOffsetFlagType    CombOffsetFlag;
    ///组合投机套保标志
    TThostFtdcCombHedgeFlagType CombHedgeFlag;
    ///价格
    TThostFtdcPriceType LimitPrice;
    ///数量
    TThostFtdcVolumeType    VolumeTotalOriginal;
    ///有效期类型
    TThostFtdcTimeConditionType TimeCondition;
    ///GTD日期
    TThostFtdcDateType  GTDDate;
    ///成交量类型


    TThostFtdcVolumeConditionType   VolumeCondition;
    ///最小成交量
    TThostFtdcVolumeType    MinVolume;
    ///触发条件
    TThostFtdcContingentConditionType   ContingentCondition;
    ///止损价
    TThostFtdcPriceType StopPrice;
    ///强平原因
    TThostFtdcForceCloseReasonType  ForceCloseReason;
    ///自动挂起标志
    TThostFtdcBoolType  IsAutoSuspend;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///用户强平标志
    TThostFtdcBoolType  UserForceClose;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///预埋报单编号
    TThostFtdcParkedOrderIDType ParkedOrderID;
    ///用户类型
    TThostFtdcUserTypeType  UserType;
    ///预埋单状态
    TThostFtdcParkedOrderStatusType Status;
    ///错误代码
    TThostFtdcErrorIDType   ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType  ErrorMsg;
    ///互换单标志
    TThostFtdcBoolType  IsSwapOrder;
    ///资金账号
    TThostFtdcAccountIDType AccountID;
    ///币种代码


    TThostFtdcCurrencyIDType    CurrencyID;
    ///交易编码
    TThostFtdcClientIDType  ClientID;
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
Status: 状态信息。
目前只返回status=1（已提交）的响应，当状态变为2的时候
不会主动推送，而是要通过查询才能得知。
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

## OnRspQryAccountregister

OnRspQryAccountregister
请求查询银期签约关系响应，当执行ReqQryAccountregister后，
该方法被调用


◇ 1.函数原型
virtual void
OnRspQryAccountregister(CThostFtdcAccountregisterField
*pAccountregister, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pAccountregister：客户开销户信息表
struct CThostFtdcAccountregisterField
{
    ///交易日期
    TThostFtdcTradeDateType TradeDay;
    ///银行编码
    TThostFtdcBankIDType BankID;
    ///银行分支机构编码
    TThostFtdcBankBrchIDType BankBranchID;
    ///银行帐号
    TThostFtdcBankAccountType BankAccount;
    ///期货公司编码
    TThostFtdcBrokerIDType BrokerID;
    ///期货公司分支机构编码
    TThostFtdcFutureBranchIDType BrokerBranchID;
    ///投资者帐号
    TThostFtdcAccountIDType AccountID;
    ///证件类型
    TThostFtdcIdCardTypeType IdCardType;
    ///证件号码
    TThostFtdcIdentifiedCardNoType IdentifiedCardNo;
    ///客户姓名
    TThostFtdcIndividualNameType CustomerName;
    ///币种代码
    TThostFtdcCurrencyIDType CurrencyID;
    ///开销户类别
    TThostFtdcOpenOrDestroyType OpenOrDestroy;
    ///签约日期
    TThostFtdcTradeDateType RegDate;
    ///解约日期


    TThostFtdcTradeDateType OutDate;
    ///交易ID
    TThostFtdcTIDType TID;
    ///客户类型
    TThostFtdcCustTypeType CustType;
    ///银行帐号类型
    TThostFtdcBankAccTypeType BankAccType;
    ///长客户姓名
    TThostFtdcLongIndividualNameType LongCustomerName;
};
BankID：银行编码
BankBranchID：值为0000
BrokerBranchID：值为0000
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


