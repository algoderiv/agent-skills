90.     


91.           CThostFtdcSettlementInfoConfirmField t = {0};
92.     
93.           strcpy_s(t.BrokerID, "1701");
94.     
95.           strcpy_s(t.InvestorID, "00001");
96.     
97.           while (m_ptraderapi->ReqSettlementInfoConfirm(&t, 
2)!=0)  Sleep(1000);
98.     
99.       }
100.        
101.         
102.        
103.          //报单
104.        
105.          void orderinsert()
106.        
107.          {
108.        
109.              CThostFtdcInputOrderField t = { 0 };
110.        
111.              strcpy_s(t.BrokerID, "9999");
112.        
113.              strcpy_s(t.InvestorID, "00001");
114.        
115.              strcpy_s(t.UserID, "00001");
116.        
117.              t.Direction = THOST_FTDC_D_Buy;
118.        
119.              t.CombOffsetFlag[0] = THOST_FTDC_OF_Open;
120.        
121.              t.CombHedgeFlag[0] = 
THOST_FTDC_HF_Speculation;
122.        


123.              t.ContingentCondition = 
THOST_FTDC_CC_Immediately;
124.    
125.              strcpy_s(t.InstrumentID, "sc1807");
126.        
127.              t.ForceCloseReason = 
THOST_FTDC_FCC_NotForceClose;
128.        
129.              t.LimitPrice = 490;
130.        
131.              t.OrderPriceType = THOST_FTDC_OPT_LimitPrice;
132.        
133.              t.VolumeCondition = THOST_FTDC_VC_AV;
134.        
135.              t.TimeCondition = THOST_FTDC_TC_GFD;
136.        
137.              t.VolumeTotalOriginal = 1;
138.        
139.              strcpy_s(t.OrderRef, "0000001");
140.    
141.              strcpy_s(t.ExchangeID, "INE");
142.        
143.              while (m_ptraderapi->ReqOrderInsert(&t, 3) != 0) 
Sleep(1000);
144.        
145.          }
146.        
147.          //查询合约
148.        
149.          void qryInstrument()
150.        
151.          {
152.        
153.              CThostFtdcQryInstrumentField t = { 0 };


154.        
155.              strcpy_s(t.ExchangeID, "SHFE");
156.        
157.              strcpy_s(t.InstrumentID, "zn1803");
158.        
159.              while (m_ptraderapi->ReqQryInstrument(&t, 4) != 0) 
Sleep(1000);
160.        
161.          }
162.        
163.         
164.        
165.        //前置连接响应
166.        
167.          void OnFrontConnected()
168.        
169.          {
170.        
171.              printf("OnFrontConnected\n");
172.        
173.          }
174.        
175.         
176.        
177.          //登陆请求响应
178.        
179.          void OnRspUserLogin(CThostFtdcRspUserLoginField 
*pRspUserLogin, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
180.        
181.          {
182.        
183.              printf("OnRspUserLogin\n");
184.        


185.          }
186.        
187.         
188.        
189.          //结算单确认响应
190.        
191.          void 
OnRspSettlementInfoConfirm(CThostFtdcSettlementInfoConfirmFie
ld *pSettlementInfoConfirm, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
192.        
193.          {
194.        
195.              printf("OnRspSettlementInfoConfirm\n");
196.        
197.          }
198.        
199.         
200.        
201.          //查询合约响应
202.        
203.          void OnRspQryInstrument(CThostFtdcInstrumentField 
*pInstrument, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast)
204.        
205.          {
206.        
207.              printf("OnRspQryInstrument\n");
208.        
209.          }
210.         
211.        
212.          //报单通知
213.        


214.          void OnRtnOrder(CThostFtdcOrderField *pOrder)
215.        
216.          {
217.        
218.              printf("OnRtnOrder\n");
219.        
220.          }
221.        
222.          
223.        
224.          //成交通知
225.        
226.          void OnRtnTrade(CThostFtdcTradeField *pTrade)
227.        
228.          {
229.        
230.              printf("OnRtnTrade\n");
231.        
232.          }
233.        
234.         
235.        
236.          //报单录入请求响应
237.        
238.          void OnRspOrderInsert(CThostFtdcInputOrderField 
*pInputOrder, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast)
239.        
240.          {
241.        
242.              printf("OnRspOrderInsert\n");
243.        
244.          }
245.        


246.          
247.        
248.          //错误应答
249.        
250.          void OnRspError(CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
251.        
252.          {
253.        
2

---

## OnRspAuthenticate

OnRspAuthenticate
客户端认证响应,当执行ReqAuthenticate后，该方法被调用


◇ 1.函数原型
virtual void OnRspAuthenticate(CThostFtdcRspAuthenticateField
*pRspAuthenticateField, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pRspAuthenticateField：客户端认证响应
struct CThostFtdcRspAuthenticateField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///用户代码
    TThostFtdcUserIDType UserID;
    ///用户端产品信息
    TThostFtdcProductInfoType UserProductInfo;
    ///App代码
    TThostFtdcAppIDType AppID;
    ///App类型
    TThostFtdcAppTypeType AppType;
};
UserProductInfo：客户端的产品信息，如软件开发商、版本号
等。
例如：SFITTraderV100。
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

## OnRspBatchOrderAction

OnRspBatchOrderAction
批量报单操作请求响应，当执行ReqBatchOrderAction后有字段填
写不对之类的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void
OnRspBatchOrderAction(CThostFtdcInputBatchOrderActionField
*pInputBatchOrderAction, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInputBatchOrderAction：输入批量报单操作
struct CThostFtdcInputBatchOrderActionField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///报单操作引用
    TThostFtdcOrderActionRefType    OrderActionRef;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve1;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
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

## OnRspCombActionInsert

OnRspCombActionInsert
申请组合录入请求响应，当执行ReqCombActionInsert后，该方法
被调用


◇ 1.函数原型
virtual void
OnRspCombActionInsert(CThostFtdcInputCombActionField
*pInputCombAction, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInputCombAction：输入的申请组合
struct CThostFtdcInputCombActionField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///组合引用
    TThostFtdcOrderRefType  CombActionRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///买卖方向
    TThostFtdcDirectionType Direction;
    ///数量
    TThostFtdcVolumeType    Volume;
    ///组合指令方向
    TThostFtdcCombDirectionType CombDirection;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve2;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///前置编号


    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
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

## OnRspError

OnRspError
针对用户请求的出错通知。


◇ 1.函数原型
virtual void OnRspError(CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
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
查询时遇到报“OnRspError[90]: CTP：查询未就绪，请
稍后重试”
可能此时查询核心还没有就绪，建议稍后再做一次查
询操作。此类情况一般发生在用户登录后立即发起查询，
建议登录后隔一秒再查询。
还有一种情况就是遇到前置流控了，报单流控、查询
流控和会话数控制。
回到顶部
< 前页 回目录 后页 >




---

## OnRspExecOrderAction

OnRspExecOrderAction
执行宣告操作请求响应，当执行ReqExecOrderAction后，该方法
被调用


◇ 1.函数原型
virtual void
OnRspExecOrderAction(CThostFtdcInputExecOrderActionField
*pInputExecOrderAction, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pInputExecOrderAction：输入执行宣告操作
struct CThostFtdcInputExecOrderActionField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///执行宣告操作引用
    TThostFtdcOrderActionRefType    ExecOrderActionRef;
    ///执行宣告引用
    TThostFtdcOrderRefType  ExecOrderRef;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///执行宣告操作编号
    TThostFtdcExecOrderSysIDType    ExecOrderSysID;
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
ActionFlag：撤单标志，目前只有删除（撤单功能），没有修改
（改单功能）。
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

## OnRspExecOrderInsert

OnRspExecOrderInsert
执行宣告录入请求响应，当执行ReqExecOrderInsert后，该方法被
调用


◇ 1.函数原型
virtual void
OnRspExecOrderInsert(CThostFtdcInputExecOrderField
*pInputExecOrder, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pInputExecOrder：输入的执行宣告
struct CThostFtdcInputExecOrderField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///执行宣告引用
    TThostFtdcOrderRefType  ExecOrderRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///数量
    TThostFtdcVolumeType    Volume;
    ///请求编号
    TThostFtdcRequestIDType RequestID;
    ///业务单元
    TThostFtdcBusinessUnitType  BusinessUnit;
    ///开平标志
    TThostFtdcOffsetFlagType    OffsetFlag;
    ///投机套保标志
    TThostFtdcHedgeFlagType HedgeFlag;
    ///执行类型
    TThostFtdcActionTypeType    ActionType;
    ///保留头寸申请的持仓方向
    TThostFtdcPosiDirectionType PosiDirection;
    ///期权行权后是否保留期货头寸的标记,该字段已废弃
    TThostFtdcExecOrderPositionFlagType ReservePositionFlag;
    ///期权行权后生成的头寸是否自动平仓


    TThostFtdcExecOrderCloseFlagType    CloseFlag;
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

## OnRspForQuoteInsert

OnRspForQuoteInsert
询价录入请求响应，当执行ReqForQuoteInsert后有字段填写不对
之类的CTP报错则通过此接口返回。


◇ 1.函数原型
virtual void OnRspForQuoteInsert(CThostFtdcInputForQuoteField
*pInputForQuote, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pInputForQuote：输入的询价
struct CThostFtdcInputForQuoteField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///询价引用
    TThostFtdcOrderRefType  ForQuoteRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
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

## OnRspFromBankToFutureByFuture

OnRspFromBankToFutureByFuture
期货发起银行资金转期货应答，当执行
ReqFromBankToFutureByFuture后有字段填写不对之类的CTP报错则通
过此接口返回。


◇ 1.函数原型
virtual void
OnRspFromBankToFutureByFuture(CThostFtdcReqTransferField
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
TradeCode：202001 期货发起银行资金转期货
BankID：返回银行在期货公司的代码
BankBranchID：填0000
BrokerBranchID：一般为空


CustomerName：客户姓名
BankPassWord：返回银行的密码，都是“*”密码不显示
Password：返回期货账户的密码，都是“*”密码不显示
TransferStatus: 转账交易的最后状态
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





