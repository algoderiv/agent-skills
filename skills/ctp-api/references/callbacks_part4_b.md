    TThostFtdcOperNoType OperNo;
    ///交易ID
    TThostFtdcTIDType TID;
    ///用户标识
    TThostFtdcUserIDType UserID;
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
    ///长客户姓名
    TThostFtdcLongIndividualNameType LongCustomerName;
};


TradeCode：101001 银行发起转账开户
BankBranchID：填0000
BrokerBranchID：一般为空
InstallID：CTP内部使用
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnRtnOptionSelfClose

OnRtnOptionSelfClose
期权自对冲通知，当执行ReqOptionSelfCloseInsert后并且报出
后，收到返回则调用此接口，私有流回报。


◇ 1.函数原型
virtual void
OnRtnOptionSelfClose(CThostFtdcOptionSelfCloseField
*pOptionSelfClose) {};
回到顶部


◇ 2.参数
pOptionSelfClose：期权自对冲
struct CThostFtdcOptionSelfCloseField
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
    ///本地期权自对冲编号
    TThostFtdcOrderLocalIDType  OptionSelfCloseLocalID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///会员代码
    TThostFtdcParticipantIDType ParticipantID;
    ///客户代码


    TThostFtdcClientIDType  ClientID;
    ///保留的无效字段
    TThostFtdcOldExchangeInstIDType reserve2;
    ///交易所交易员代码
    TThostFtdcTraderIDType  TraderID;
    ///安装编号
    TThostFtdcInstallIDType InstallID;
    ///期权自对冲提交状态
    TThostFtdcOrderSubmitStatusType OrderSubmitStatus;
    ///报单提示序号
    TThostFtdcSequenceNoType    NotifySequence;
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///结算编号
    TThostFtdcSettlementIDType  SettlementID;
    ///期权自对冲编号
    TThostFtdcOrderSysIDType    OptionSelfCloseSysID;
    ///报单日期
    TThostFtdcDateType  InsertDate;
    ///插入时间
    TThostFtdcTimeType  InsertTime;
    ///撤销时间
    TThostFtdcTimeType  CancelTime;
    ///自对冲结果
    TThostFtdcExecResultType    ExecResult;
    ///结算会员编号
    TThostFtdcParticipantIDType ClearingPartID;
    ///序号
    TThostFtdcSequenceNoType    SequenceNo;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///用户端产品信息


    TThostFtdcProductInfoType   UserProductInfo;
    ///状态信息
    TThostFtdcErrorMsgType  StatusMsg;
    ///操作用户代码
    TThostFtdcUserIDType    ActiveUserID;
    ///经纪公司报单编号
    TThostFtdcSequenceNoType    BrokerOptionSelfCloseSeq;
    ///营业部编号
    TThostFtdcBranchIDType  BranchID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///资金账号
    TThostFtdcAccountIDType AccountID;
    ///币种代码
    TThostFtdcCurrencyIDType    CurrencyID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve3;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///合约在交易所的代码
    TThostFtdcExchangeInstIDType    ExchangeInstID;
    ///IP地址
    TThostFtdcIPAddressType IPAddress;
};
InstallID：CTP内部使用
OrderSubmitStatus：报单报入状态
ExecResult：最后的结果，检查是否自对冲成功
StatusMsg：错误信息


ActiveUserID：委托回报报文中此字段值均为空，无论是否是
首次委托其请求报文中此字段值也置为空。撤单回报报文中此字段
值有值，同撤单请求报文。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---



... 更多内容请参考 PDF 原文 ...


