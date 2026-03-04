# Trader Spi - CTP 6.7.8 API

本节包含 1 个主题。

## 目录

    - [CThostFtdcTraderSpi](#CThostFtdcTraderSpi) (第 398 页)

---

## CThostFtdcTraderSpi

CThostFtdcTraderSpi
■ 6.7.8_API接口说明
└△ 交易接口
　└◆ CThostFtdcTraderSpi
CThostFtdcTraderSpi类提供了交易相关的回调接口，用户需要继
承该类并重载这些接口，以获取响应数据。


◇ 1.接口
class CThostFtdcTraderSpi
{
public:
    ///当客户端与交易后台建立起通信连接时（还未登录前），该方
法被调用。
    virtual void OnFrontConnected(){};
    ///当客户端与交易后台通信连接断开时，该方法被调用。当发生
这个情况后，API会自动重新连接，客户端可不做处理。
    ///@param nReason 错误原因
    ///        0x1001 网络读失败
    ///        0x1002 网络写失败
    ///        0x2001 接收心跳超时
    ///        0x2002 发送心跳失败
    ///        0x2003 收到错误报文
    virtual void OnFrontDisconnected(int nReason){};
    ///心跳超时警告。当长时间未收到报文时，该方法被调用。
    ///@param nTimeLapse 距离上次接收报文的时间
    virtual void OnHeartBeatWarning(int nTimeLapse){};
    ///客户端认证响应
    virtual void OnRspAuthenticate(CThostFtdcRspAuthenticateField 
*pRspAuthenticateField, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///登录请求响应
    virtual void OnRspUserLogin(CThostFtdcRspUserLoginField 
*pRspUserLogin, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///登出请求响应
    virtual void OnRspUserLogout(CThostFtdcUserLogoutField 
*pUserLogout, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///用户口令更新请求响应


    virtual void 
OnRspUserPasswordUpdate(CThostFtdcUserPasswordUpdateField 
*pUserPasswordUpdate, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///资金账户口令更新请求响应
    virtual void 
OnRspTradingAccountPasswordUpdate(CThostFtdcTradingAccountPa
sswordUpdateField *pTradingAccountPasswordUpdate, 
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
    ///查询用户当前支持的认证模式的回复
    virtual void 
OnRspUserAuthMethod(CThostFtdcRspUserAuthMethodField 
*pRspUserAuthMethod, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///获取图形验证码请求的回复
    virtual void 
OnRspGenUserCaptcha(CThostFtdcRspGenUserCaptchaField 
*pRspGenUserCaptcha, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///获取短信验证码请求的回复
    virtual void OnRspGenUserText(CThostFtdcRspGenUserTextField 
*pRspGenUserText, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///报单录入请求响应
    virtual void OnRspOrderInsert(CThostFtdcInputOrderField 
*pInputOrder, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///预埋单录入请求响应
    virtual void OnRspParkedOrderInsert(CThostFtdcParkedOrderField 
*pParkedOrder, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///预埋撤单录入请求响应
    virtual void 
OnRspParkedOrderAction(CThostFtdcParkedOrderActionField 


*pParkedOrderAction, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///报单操作请求响应
    virtual void OnRspOrderAction(CThostFtdcInputOrderActionField 
*pInputOrderAction, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///查询最大报单数量响应
    virtual void 
OnRspQryMaxOrderVolume(CThostFtdcQryMaxOrderVolumeField 
*pQryMaxOrderVolume, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///投资者结算结果确认响应
    virtual void 
OnRspSettlementInfoConfirm(CThostFtdcSettlementInfoConfirmField 
*pSettlementInfoConfirm, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///删除预埋单响应
    virtual void 
OnRspRemoveParkedOrder(CThostFtdcRemoveParkedOrderField 
*pRemoveParkedOrder, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///删除预埋撤单响应
    virtual void 
OnRspRemoveParkedOrderAction(CThostFtdcRemoveParkedOrderAct
ionField *pRemoveParkedOrderAction, CThostFtdcRspInfoField 
*pRspInfo, int nRequestID, bool bIsLast) {};
    ///执行宣告录入请求响应
    virtual void OnRspExecOrderInsert(CThostFtdcInputExecOrderField 
*pInputExecOrder, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///执行宣告操作请求响应
    virtual void 
OnRspExecOrderAction(CThostFtdcInputExecOrderActionField 
*pInputExecOrderAction, CThostFtdcRspInfoField *pRspInfo, int 


nRequestID, bool bIsLast) {};
    ///询价录入请求响应
    virtual void OnRspForQuoteInsert(CThostFtdcInputForQuoteField 
*pInputForQuote, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///报价录入请求响应
    virtual void OnRspQuoteInsert(CThostFtdcInputQuoteField 
*pInputQuote, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///报价操作请求响应
    virtual void OnRspQuoteAction(CThostFtdcInputQuoteActionField 
*pInputQuoteAction, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///批量报单操作请求响应
    virtual void 
OnRspBatchOrderAction(CThostFtdcInputBatchOrderActionField 
*pInputBatchOrderAction, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///期权自对冲录入请求响应
    virtual void 
OnRspOptionSelfCloseInsert(CThostFtdcInputOptionSelfCloseField 
*pInputOptionSelfClose, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///期权自对冲操作请求响应
    virtual void 
OnRspOptionSelfCloseAction(CThostFtdcInputOptionSelfCloseAction
Field *pInputOptionSelfCloseAction, CThostFtdcRspInfoField 
*pRspInfo, int nRequestID, bool bIsLast) {};
    ///申请组合录入请求响应
    virtual void 
OnRspCombActionInsert(CThostFtdcInputCombActionField 
*pInputCombAction, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///请求查询报单响应


    virtual void OnRspQryOrder(CThostFtdcOrderField *pOrder, 
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
    ///请求查询成交响应
    virtual void OnRspQryTrade(CThostFtdcTradeField *pTrade, 
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
    ///请求查询投资者持仓响应
    virtual void 
OnRspQryInvestorPosition(CThostFtdcInvestorPositionField 
*pInvestorPosition, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///请求查询资金账户响应
    virtual void 
OnRspQryTradingAccount(CThostFtdcTradingAccountField 
*pTradingAccount, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///请求查询投资者响应
    virtual void OnRspQryInvestor(CThostFtdcInvestorField *pInvestor, 
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
    ///请求查询交易编码响应
    virtual void OnRspQryTradingCode(CThostFtdcTradingCodeField 
*pTradingCode, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///请求查询合约保证金率响应
    virtual void 
OnRspQryInstrumentMarginRate(CThostFtdcInstrumentMarginRateFie
ld *pInstrumentMarginRate, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///请求查询合约手续费率响应
    virtual void 
OnRspQryInstrumentCommissionRate(CThostFtdcInstrumentCommiss
ionRateField *pInstrumentCommissionRate, CThostFtdcRspInfoField 
*pRspInfo, int nRequestID, bool bIsLast) {};
    ///请求查询交易所响应
    virtual void OnRspQryExchange(CThostFtdcExchangeField 


*pExchange, CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool 
bIsLast) {};
    ///请求查询产品响应
    virtual void OnRspQryProduct(CThostFtdcProductField *pProduct, 
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
    ///请求查询合约响应
    virtual void OnRspQryInstrument(CThostFtdcInstrumentField 
*pInstrument, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///请求查询行情响应
    virtual void 
OnRspQryDepthMarketData(CThostFtdcDepthMarketDataField 
*pDepthMarketData, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///请求查询投资者结算结果响应
    virtual void 
OnRspQrySettlementInfo(CThostFtdcSettlementInfoField 
*pSettlementInfo, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///请求查询转帐银行响应
    virtual void OnRspQryTransferBank(CThostFtdcTransferBankField 
*pTransferBank, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///请求查询投资者持仓明细响应
    virtual void 
OnRspQryInvestorPositionDetail(CThostFtdcInvestorPositionDetailFie
ld *pInvestorPositionDetail, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///请求查询客户通知响应
    virtual void OnRspQryNotice(CThostFtdcNoticeField *pNotice, 
CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
    ///请求查询结算信息确认响应
    virtual void 
OnRspQrySettlementInfoConfirm(CThostFtdcSettle

---

