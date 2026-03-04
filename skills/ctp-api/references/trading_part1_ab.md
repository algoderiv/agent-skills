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
    TT

---

## ReqQryAccountregister

ReqQryAccountregister
请求查询银期签约关系
响应: OnRspQryAccountregister


◇ 1.函数原型
virtual int
ReqQryAccountregister(CThostFtdcQryAccountregisterField
*pQryAccountregister, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryAccountregister：请求查询银期签约关系
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
否
TThostFtdcAccountIDType
AccountID
投资者帐
号
否
TThostFtdcBankIDType
BankID
银行代码
否
TThostFtdcBankBrchIDType BankBranchID 银行分支
机构代码
否
TThostFtdcCurrencyIDType
CurrencyID
币种代码
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
CThostFtdcQryAccountregisterField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.AccountID, "1000001");
strcpy_s(a.BankID, "1");
strcpy_s(a.CurrencyID, "CNY");
m_pUserApi->ReqQryAccountregister(&a, 1);
回到顶部


◇ 5.FAQ
查询无结果是什么原因？
只有和银行签约成功后，并且银行的签约信息返回CT
P柜台后才能查询到信息。对应柜台菜单【银期转账账户
信息查询】
回到顶部
< 前页 回目录 后页 >


ReqQryBrokerTradingAlgos
请求查询经纪公司交易算法
响应: OnRspQryBrokerTradingAlgos


◇ 1.函数原型
virtual int
ReqQryBrokerTradingAlgos(CThostFtdcQryBrokerTradingAlgosField
*pQryBrokerTradingAlgos, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryBrokerTradingAlgos：查询经纪公司交易算法
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
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
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


ReqQryBrokerTradingParams
请求查询经纪公司交易参数
只能用投资者账号登录才能查到，不能用操作员登录去查
响应: OnRspQryBrokerTradingParams


◇ 1.函数原型
virtual int
ReqQryBrokerTradingParams(CThostFtdcQryBrokerTradingParamsField
*pQryBrokerTradingParams, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryBrokerTradingParams：查询经纪公司交易参数
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
CThostFtdcQryBrokerTradingParamsField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
m_pUserApi->ReqQryBrokerTradingParams(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryCFMMCTradingAccountKey

ReqQryCFMMCTradingAccountKey
请求查询保证金监管系统经纪公司资金账户密钥，此接口已弃
用，请使用ReqQueryCFMMCTradingAccountToken查询。
响应: OnRspQryCFMMCTradingAccountKey


◇ 1.函数原型
virtual int
ReqQryCFMMCTradingAccountKey(CThostFtdcQryCFMMCTradingA
ccountKeyField *pQryCFMMCTradingAccountKey, int nRequestID) =
0;
回到顶部


◇ 2.参数
pQryCFMMCTradingAccountKey：请求查询保证金监管系统经
纪公司资金账户密钥
字段类型
字段名称
含义
是否可作为过滤
条件
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
否
TThostFtdcInvestorIDType InvestorID 投资者代
码
否
接口弃用
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
CThostFtdcQryCFMMCTradingAccountKeyField a = { 0 };
strcpy(a.BrokerID, "9999");
strcpy(a.InvestorID, "1000001");
m_pUserApi->ReqQryCFMMCTradingAccountKey(&a, 
nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryCombAction
请求查询申请组合
响应: OnRspQryCombAction


◇ 1.函数原型
virtual int ReqQryCombAction(CThostFtdcQryCombActionField
*pQryCombAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryCombAction：申请组合查询
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
是
TThostFtdcInvestUnitIDType
InvestUnitID 投资单
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
CThostFtdcQryCombActionField a = { 0 };
strcpy(a.BrokerID, "9999");
strcpy(a.InvestorID, "1000001");
strcpy(a.InstrumentID, "rb1809");//不填写则返回全部
strcpy(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryCombAction(&a, 1);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryCombInstrumentGuard
请求查询组合合约安全系数
响应：OnRspQryCombInstrumentGuard


◇ 1.函数原型
virtual int
ReqQryCombInstrumentGuard(CThostFtdcQryCombInstrumentGuardFi
eld *pQryCombInstrumentGuard, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryCombInstrumentGuard：组合合约安全系数查询
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
TThostFtdcInstrumentIDType InstrumentID 合约代码否
TThostFtdcExchangeIDType
ExchangeID
交易所代
码
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


ReqQryContractBank
请求查询签约银行
响应: OnRspQryContractBank


◇ 1.函数原型
virtual int ReqQryContractBank(CThostFtdcQryContractBankField
*pQryContractBank, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryContractBank：查询签约银行请求
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
CThostFtdcQryContractBankField a = { 0 };
strcpy_s(a.BrokerID, "9999");
m_pUserApi->ReqQryContractBank(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryDepthMarketData
请求查询行情，只能查询当前快照，不能查询历史行情。
响应: OnRspQryDepthMarketData


◇ 1.函数原型
virtual int
ReqQryDepthMarketData(CThostFtdcQryDepthMarketDataField
*pQryDepthMarketData, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryDepthMarketData：查询行情
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
TThostFtdcProductClassType
ProductClass 产品类
型
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

## ReqQryInvestor

ReqQryInvestor
请求查询投资者
响应：OnRspQryInvestor


◇ 1.函数原型
virtual int ReqQryInvestor(CThostFtdcQryInvestorField
*pQryInvestor, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestor：查询投资者
字段类型
字段名称
含义
是否可作为过滤
条件
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
是
TThostFtdcInvestorIDType InvestorID 投资者代
码
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
CThostFtdcQryInvestorField a = { 0 };
strcpy(a.BrokerID, "9999");
strcpy(a.InvestorID, "1000001");
m_pUserApi->ReqQryInvestor(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryInvestorPosition

ReqQryInvestorPosition
请求查询投资者持仓，对应响应OnRspQryInvestorPosition。CTP
系统将持仓明细记录按合约，持仓方向，开仓日期（仅针对上期所和
能源所，区分昨仓、今仓）进行汇总。


◇ 1.函数原型
virtual int
ReqQryInvestorPosition(CThostFtdcQryInvestorPositionField
*pQryInvestorPosition, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPosition：查询投资者持仓
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
CThostFtdcQryInvestorPositionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");//不填写合约则返回所有持仓
m_pUserApi->ReqQryInvestorPosition(&a, 1);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryInvestorPositionCombineDetail

ReqQryInvestorPositionCombineDetail
请求查询投资者持仓明细
响应: OnRspQryInvestorPositionCombineDetail


◇ 1.函数原型
virtual int
ReqQryInvestorPositionCombineDetail(CThostFtdcQryInvestorPosition
CombineDetailField *pQryInvestorPositionCombineDetail, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPositionCombineDetail：查询组合持仓明细
字段类型
字段名称
含义
是否可
作为过
滤条件
TThostFtdcBrokerIDType
BrokerID
经纪
公司
代码
是
TThostFtdcInvestorIDType
InvestorID
投资
者代
码
是
TThostFtdcInstrumentIDType
CombInstrumentID
组合
持仓
合约
编码
是
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
否
TThostFtdcInvestUnitIDType
InvestUnitID
投资
单元
代码
否
TThostFtdcOldInstrumentIDType reserve1
保留
的无
效字
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
大商所下套利单成交后查询接口没有找到持仓，是什么
原因呢？
因为柜台开启了大商所rule新型组合保证金算法，就
没有套利持仓了。
回到顶部
< 前页 回目录 后页 >




---

## ReqQryInvestorPositionDetail

ReqQryInvestorPositionDetail
请求查询投资者持仓明细，对应响应
OnRspQryInvestorPositionDetail。
CTP系统根据来自交易所的成交记录生成持仓明细记录，一笔成
交记录对应一条持仓明细记录。


◇ 1.函数原型
virtual int
ReqQryInvestorPositionDetail(CThostFtdcQryInvestorPositionDetailFiel
d *pQryInvestorPositionDetail, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPositionDetail：查询投资者持仓明细
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

## ReqQryInvestorProductGroupMargin

ReqQryInvestorProductGroupMargin
请求查询投资者品种/跨品种保证金
响应: OnRspQryInvestorProductGroupMargin


◇ 1.函数原型
virtual int
ReqQryInvestorProductGroupMargin(CThostFtdcQryInvestorProductGr
oupMarginField *pQryInvestorProductGroupMargin, int nRequestID) =
0;
回到顶部


◇ 2.参数
pQryInvestorProductGroupMargin：查询投资者品种/跨品种保证
金
字段类型
字段名称
含义
是否可
作为过
滤条件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcInstrumentIDType
ProductGroupID 品种
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcInvestUnitIDType
InvestUnitID
投资单
元代码否
TThostFtdcHedgeFlagType
HedgeFlag
投机套
保标志
不需要
填写
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


ReqQryInvestUnit
请求查询投资单元，暂不支持此功能
响应: OnRspQryInvestUnit


◇ 1.函数原型
virtual int ReqQryInvestUnit(CThostFtdcQryInvestUnitField
*pQryInvestUnit, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestUnit：查询投资单元
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


ReqQryMMInstrumentCommissionRate
请求查询做市商合约手续费率
响应: OnRspQryMMInstrumentCommissionRate


◇ 1.函数原型
virtual int
ReqQryMMInstrumentCommissionRate(CThostFtdcQryMMInstrument
CommissionRateField *pQryMMInstrumentCommissionRate, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryMMInstrumentCommissionRate：查询做市商合约手续费率
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


ReqQryMMOptionInstrCommRate
请求查询做市商期权合约手续费
响应: OnRspQryMMOptionInstrCommRate


◇ 1.函数原型
virtual int
ReqQryMMOptionInstrCommRate(CThostFtdcQryMMOptionInstrCom
mRateField *pQryMMOptionInstrCommRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryMMOptionInstrCommRate：做市商期权手续费率查询
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


ReqQryNotice
请求查询客户通知
响应: OnRspQryNotice


◇ 1.函数原型
virtual int ReqQryNotice(CThostFtdcQryNoticeField *pQryNotice,
int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryNotice：查询客户通知
字段类型
字段名
称
含义
是否可作为过滤
条件
TThostFtdcBrokerIDType BrokerID 经纪公司代
码
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

## ReqQryOptionInstrTradeCost

ReqQryOptionInstrTradeCost
请求查询期权交易成本，该函数用于查期权保证金，对应响应
OnRspQryOptionInstrTradeCost。
保证金=max(权利金+FixedMargin,MiniMargin)，用户可根据此
公式计算实时保证金。


◇ 1.函数原型
virtual int
ReqQryOptionInstrTradeCost(CThostFtdcQryOptionInstrTradeCostField
*pQryOptionInstrTradeCost, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryOptionInstrTradeCost：期权交易成本查询
字段类型
字段名称
含义
是否可
作为过
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
TThostFtdcInstrumentIDType
InstrumentID
合约代码是
TThostFtdcExchangeIDType
ExchangeID
交易所代
码
否
TThostFtdcInvestUnitIDType
InvestUnitID
投资单元
代码
否
TThostFtdcHedgeFlagType
HedgeFlag
投机套保
标志
是
TThostFtdcPriceType
InputPrice
期权合约
报价
是
TThostFtdcPriceType
UnderlyingPrice
标的价
格,填0则
用昨结算
价
是
TThostFtdcOldInstrumentIDType reserve1
保留的无
效字段
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
CThostFtdcQryOptionInstrTradeCostField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
a.HedgeFlag = THOST_FTDC_HF_Speculation;
a.InputPrice = 300; 
m_pUserApi->ReqQryOptionInstrTradeCost(&a, nRequestID++);
回到顶部


◇ 5.FAQ
为什么我用这个接口算出来的保证金跟资金查询得到的
保证金占用不一致？
ReqQryOptionInstrTradeCost计算出的期权保证金跟资
金查询里的期权保证金的计算方式不一样。
ReqQryOptionInstrTradeCost只是估计计算，因为其使
用的公式（保证金=max(权利金+FixedMargin,MiniMargi
n)）中的权利金部分在计算时使用的期权价格是InputPric
e。
而资金查询里的期权保证金计算公式中的期权价格是
使用max算法（max(昨结算，最新价)）得到的。
所以两者存在差别。
回到顶部
< 前页 回目录 后页 >


ReqQryOptionSelfClose
请求查询期权自对冲
响应: OnRspQryOptionSelfClose


◇ 1.函数原型
virtual int
ReqQryOptionSelfClose(CThostFtdcQryOptionSelfCloseField
*pQryOptionSelfClose, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryOptionSelfClose：期权自对冲查询
字段类型
字段名称
含义
是否
可作
为过
滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪
公司
代码
是
TThostFtdcInvestorIDType
InvestorID
投资
者代
码
是
TThostFtdcInstrumentIDType
InstrumentID
合约
代码是
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
是
TThostFtdcOrderSysIDType
OptionSelfCloseSysID
期权
自对
冲编
号
是
TThostFtdcTimeType
InsertTimeStart
开始
时间是
TThostFtdcTimeType
InsertTimeEnd
结束
时间是


TThostFtdcOldInstrumentIDType reserve1
保留
的无
效字
段
否
OnRspQryOptionSelfClose：由此能定位一笔期权自对冲的报单
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
CThostFtdcQryOptionSelfCloseField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryOptionSelfClose(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryOrder
请求查询报单
响应: OnRspQryOrder


◇ 1.函数原型
virtual int ReqQryOrder(CThostFtdcQryOrderField *pQryOrder, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryOrder：查询报单
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
TThostFtdcOrderSysIDType
OrderSysID
报单编
号
是
TThostFtdcTimeType
InsertTimeStart 开始时
间
是
TThostFtdcTimeType
InsertTimeEnd
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
CThostFtdcQryOrderField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryOrder(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryParkedOrder
请求查询预埋单
响应: OnRspQryParkedOrder


◇ 1.函数原型
virtual int ReqQryParkedOrder(CThostFtdcQryParkedOrderField
*pQryParkedOrder, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryParkedOrder：查询预埋单
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
是
TThostFtdcInvestUnitIDType
InvestUnitID 投资单
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


ReqQryParkedOrderAction
请求查询预埋撤单
响应: OnRspQryParkedOrderAction


◇ 1.函数原型
virtual int
ReqQryParkedOrderAction(CThostFtdcQryParkedOrderActionField
*pQryParkedOrderAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryParkedOrderAction：查询预埋撤单
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
是
TThostFtdcInvestUnitIDType
InvestUnitID 投资单
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





