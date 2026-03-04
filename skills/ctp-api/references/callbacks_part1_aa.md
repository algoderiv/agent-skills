# Callbacks - CTP 6.7.8 API

本节包含 147 个主题。

## 目录

      - [OnRspError](#OnRspError) (第 324 页)
      - [OnRspQryMulticastInstrument](#OnRspQryMulticastInstrument) (第 329 页)
      - [OnRspSubForQuoteRsp](#OnRspSubForQuoteRsp) (第 334 页)
      - [OnRspSubMarketData](#OnRspSubMarketData) (第 339 页)
      - [OnRspUnSubForQuoteRsp](#OnRspUnSubForQuoteRsp) (第 344 页)
      - [OnRspUnSubMarketData](#OnRspUnSubMarketData) (第 349 页)
      - [OnRspUserLogin](#OnRspUserLogin) (第 354 页)
      - [OnRspUserLogout](#OnRspUserLogout) (第 360 页)
      - [OnRtnDepthMarketData](#OnRtnDepthMarketData) (第 365 页)
      - [OnRtnForQuoteRsp](#OnRtnForQuoteRsp) (第 373 页)
      - [OnRspAuthenticate](#OnRspAuthenticate) (第 554 页)
      - [OnRspBatchOrderAction](#OnRspBatchOrderAction) (第 560 页)
      - [OnRspCombActionInsert](#OnRspCombActionInsert) (第 566 页)
      - [OnRspError](#OnRspError) (第 572 页)
      - [OnRspExecOrderAction](#OnRspExecOrderAction) (第 577 页)
      - [OnRspExecOrderInsert](#OnRspExecOrderInsert) (第 583 页)
      - [OnRspForQuoteInsert](#OnRspForQuoteInsert) (第 590 页)
      - [OnRspFromBankToFutureByFuture](#OnRspFromBankToFutureByFuture) (第 596 页)
      - [OnRspFromFutureToBankByFuture](#OnRspFromFutureToBankByFuture) (第 604 页)
      - [OnRspGenUserCaptcha](#OnRspGenUserCaptcha) (第 612 页)
      - [OnRspGenUserText](#OnRspGenUserText) (第 617 页)
      - [OnRspOptionSelfCloseAction](#OnRspOptionSelfCloseAction) (第 622 页)
      - [OnRspOptionSelfCloseInsert](#OnRspOptionSelfCloseInsert) (第 628 页)
      - [OnRspOrderAction](#OnRspOrderAction) (第 634 页)
      - [OnRspOrderInsert](#OnRspOrderInsert) (第 641 页)
      - [OnRspParkedOrderAction](#OnRspParkedOrderAction) (第 648 页)
      - [OnRspParkedOrderInsert](#OnRspParkedOrderInsert) (第 655 页)
      - [OnRspQryAccountregister](#OnRspQryAccountregister) (第 663 页)
      - [OnRspQryBrokerTradingAlgos](#OnRspQryBrokerTradingAlgos) (第 669 页)
      - [OnRspQryBrokerTradingParams](#OnRspQryBrokerTradingParams) (第 675 页)
      - [OnRspQryCFMMCTradingAccountKey](#OnRspQryCFMMCTradingAccountKey) (第 681 页)
      - [OnRspQryCombAction](#OnRspQryCombAction) (第 687 页)
      - [OnRspQryCombInstrumentGuard](#OnRspQryCombInstrumentGuard) (第 694 页)
      - [OnRspQryContractBank](#OnRspQryContractBank) (第 700 页)
      - [OnRspQryDepthMarketData](#OnRspQryDepthMarketData) (第 706 页)
      - [OnRspQryEWarrantOffset](#OnRspQryEWarrantOffset) (第 714 页)
      - [OnRspQryExchange](#OnRspQryExchange) (第 720 页)
      - [OnRspQryExchangeMarginRate](#OnRspQryExchangeMarginRate) (第 725 页)
      - [OnRspQryExchangeMarginRateAdjust](#OnRspQryExchangeMarginRateAdjust) (第 731 页)
      - [OnRspQryExchangeRate](#OnRspQryExchangeRate) (第 737 页)
      - [OnRspQryExecOrder](#OnRspQryExecOrder) (第 743 页)
      - [OnRspQryForQuote](#OnRspQryForQuote) (第 751 页)
      - [OnRspQryInstrument](#OnRspQryInstrument) (第 758 页)
      - [OnRspQryInstrumentCommissionRate](#OnRspQryInstrumentCommissionRate) (第 767 页)
      - [OnRspQryInstrumentMarginRate](#OnRspQryInstrumentMarginRate) (第 773 页)
      - [OnRspQryInstrumentOrderCommRate](#OnRspQryInstrumentOrderCommRate) (第 779 页)
      - [OnRspQryInvestor](#OnRspQryInvestor) (第 785 页)
      - [OnRspQryInvestorPosition](#OnRspQryInvestorPosition) (第 791 页)
      - [OnRspQryInvestorPositionCombineDetail](#OnRspQryInvestorPositionCombineDetail) (第 800 页)
      - [OnRspQryInvestorPositionDetail](#OnRspQryInvestorPositionDetail) (第 807 页)
      - [OnRspQryInvestorProductGroupMargin](#OnRspQryInvestorProductGroupMargin) (第 814 页)
      - [OnRspQryInvestUnit](#OnRspQryInvestUnit) (第 821 页)
      - [OnRspQryMMInstrumentCommissionRate](#OnRspQryMMInstrumentCommissionRate) (第 827 页)
      - [OnRspQryMMOptionInstrCommRate](#OnRspQryMMOptionInstrCommRate) (第 833 页)
      - [OnRspQryNotice](#OnRspQryNotice) (第 839 页)
      - [OnRspQryOptionInstrCommRate](#OnRspQryOptionInstrCommRate) (第 844 页)
      - [OnRspQryOptionInstrTradeCost](#OnRspQryOptionInstrTradeCost) (第 850 页)
      - [OnRspQryOptionSelfClose](#OnRspQryOptionSelfClose) (第 856 页)
      - [OnRspQryOrder](#OnRspQryOrder) (第 864 页)
      - [OnRspQryParkedOrder](#OnRspQryParkedOrder) (第 874 页)
      - [OnRspQryParkedOrderAction](#OnRspQryParkedOrderAction) (第 881 页)
      - [OnRspQryProduct](#OnRspQryProduct) (第 888 页)
      - [OnRspQryProductExchRate](#OnRspQryProductExchRate) (第 894 页)
      - [OnRspQryProductGroup](#OnRspQryProductGroup) (第 900 页)
      - [OnRspQryQuote](#OnRspQryQuote) (第 906 页)
      - [OnRspQrySecAgentACIDMap](#OnRspQrySecAgentACIDMap) (第 915 页)
      - [OnRspQrySecAgentCheckMode](#OnRspQrySecAgentCheckMode) (第 921 页)
      - [OnRspQrySecAgentTradeInfo](#OnRspQrySecAgentTradeInfo) (第 927 页)
      - [OnRspQrySecAgentTradingAccount](#OnRspQrySecAgentTradingAccount) (第 932 页)
      - [OnRspQrySettlementInfo](#OnRspQrySettlementInfo) (第 940 页)
      - [OnRspQrySettlementInfoConfirm](#OnRspQrySettlementInfoConfirm) (第 946 页)
      - [OnRspQryTrade](#OnRspQryTrade) (第 952 页)
      - [OnRspQryTradingAccount](#OnRspQryTradingAccount) (第 959 页)
      - [OnRspQryTradingCode](#OnRspQryTradingCode) (第 968 页)
      - [OnRspQryTradingNotice](#OnRspQryTradingNotice) (第 974 页)
      - [OnRspQryTransferBank](#OnRspQryTransferBank) (第 980 页)
      - [OnRspQryTransferSerial](#OnRspQryTransferSerial) (第 985 页)
      - [OnRspQueryBankAccountMoneyByFuture](#OnRspQueryBankAccountMoneyByFuture) (第 992 页)
      - [OnRspQueryCFMMCTradingAccountToken](#OnRspQueryCFMMCTradingAccountToken) (第 1000 页)
      - [OnRspQuoteAction](#OnRspQuoteAction) (第 1005 页)
      - [OnRspQuoteInsert](#OnRspQuoteInsert) (第 1011 页)
      - [OnRspRemoveParkedOrder](#OnRspRemoveParkedOrder) (第 1018 页)
      - [OnRspRemoveParkedOrderAction](#OnRspRemoveParkedOrderAction) (第 1023 页)
      - [OnRspSettlementInfoConfirm](#OnRspSettlementInfoConfirm) (第 1028 页)
      - [OnRspTradingAccountPasswordUpdate](#OnRspTradingAccountPasswordUpdate) (第 1034 页)
      - [OnRspUserAuthMethod](#OnRspUserAuthMethod) (第 1040 页)
      - [OnRspUserLogin](#OnRspUserLogin) (第 1045 页)
      - [OnRspUserLogout](#OnRspUserLogout) (第 1051 页)
      - [OnRspUserPasswordUpdate](#OnRspUserPasswordUpdate) (第 1056 页)
      - [OnRtnBulletin](#OnRtnBulletin) (第 1061 页)
      - [OnRtnCancelAccountByBank](#OnRtnCancelAccountByBank) (第 1066 页)
      - [OnRtnCFMMCTradingAccountToken](#OnRtnCFMMCTradingAccountToken) (第 1074 页)
      - [OnRtnChangeAccountByBank](#OnRtnChangeAccountByBank) (第 1079 页)
      - [OnRtnCombAction](#OnRtnCombAction) (第 1086 页)
      - [OnRtnErrorConditionalOrder](#OnRtnErrorConditionalOrder) (第 1093 页)
      - [OnRtnExecOrder](#OnRtnExecOrder) (第 1102 页)
      - [OnRtnForQuoteRsp](#OnRtnForQuoteRsp) (第 1110 页)
      - [OnRtnFromBankToFutureByBank](#OnRtnFromBankToFutureByBank) (第 1115 页)
      - [OnRtnFromBankToFutureByFuture](#OnRtnFromBankToFutureByFuture) (第 1123 页)
      - [OnRtnFromFutureToBankByBank](#OnRtnFromFutureToBankByBank) (第 1132 页)
      - [OnRtnFromFutureToBankByFuture](#OnRtnFromFutureToBankByFuture) (第 1140 页)
      - [OnRtnInstrumentStatus](#OnRtnInstrumentStatus) (第 1148 页)
      - [OnRtnOpenAccountByBank](#OnRtnOpenAccountByBank) (第 1153 页)
      - [OnRtnOptionSelfClose](#OnRtnOptionSelfClose) (第 1161 页)
      - [OnRtnOrder](#OnRtnOrder) (第 1169 页)
      - [OnRtnQueryBankBalanceByFuture](#OnRtnQueryBankBalanceByFuture) (第 1179 页)
      - [OnRtnQuote](#OnRtnQuote) (第 1186 页)
      - [OnRtnRepealFromBankToFutureByBank](#OnRtnRepealFromBankToFutureByBank) (第 1194 页)
      - [OnRtnRepealFromBankToFutureByFuture](#OnRtnRepealFromBankToFutureByFuture) (第 1202 页)
      - [OnRtnRepealFromBankToFutureByFutureManual](#OnRtnRepealFromBankToFutureByFutureManual) (第 1210 页)
      - [OnRtnRepealFromFutureToBankByBank](#OnRtnRepealFromFutureToBankByBank) (第 1218 页)
      - [OnRtnRepealFromFutureToBankByFuture](#OnRtnRepealFromFutureToBankByFuture) (第 1226 页)
      - [OnRtnRepealFromFutureToBankByFutureManual](#OnRtnRepealFromFutureToBankByFutureManual) (第 1234 页)
      - [OnRtnTrade](#OnRtnTrade) (第 1242 页)
      - [OnRtnTradingNotice](#OnRtnTradingNotice) (第 1249 页)
      - [OnRspQryMaxOrderVolume](#OnRspQryMaxOrderVolume) (第 1254 页)
      - [OnRspQryClassifiedInstrument](#OnRspQryClassifiedInstrument) (第 1260 页)
      - [OnRspQryCombPromotionParam](#OnRspQryCombPromotionParam) (第 1267 页)
      - [OnRspQryRiskSettleInvstPosition](#OnRspQryRiskSettleInvstPosition) (第 1272 页)
      - [OnRspQryRiskSettleProductStatus](#OnRspQryRiskSettleProductStatus) (第 1280 页)
      - [OnRspQryTraderOffer](#OnRspQryTraderOffer) (第 1285 页)
      - [OnRspQrySPBMFutureParameter](#OnRspQrySPBMFutureParameter) (第 1291 页)
      - [OnRspQrySPBMOptionParameter](#OnRspQrySPBMOptionParameter) (第 1297 页)
      - [OnRspQrySPBMIntraParameter](#OnRspQrySPBMIntraParameter) (第 1303 页)
      - [OnRspQrySPBMInterParameter](#OnRspQrySPBMInterParameter) (第 1308 页)
      - [OnRspQrySPBMPortfDefinition](#OnRspQrySPBMPortfDefinition) (第 1313 页)
      - [OnRspQrySPBMInvestorPortfDef](#OnRspQrySPBMInvestorPortfDef) (第 1318 页)
      - [OnRspQryInvestorPortfMarginRatio](#OnRspQryInvestorPortfMarginRatio) (第 1323 页)
      - [OnRspQryInvestorProdSPBMDetail](#OnRspQryInvestorProdSPBMDetail) (第 1328 页)
      - [OnRspQryInvestorCommoditySPMMMargin](#OnRspQryInvestorCommoditySPMMMargin) (第 1334 页)
      - [OnRspQryInvestorCommodityGroupSPMMMargin](#OnRspQryInvestorCommodityGroupSPMMMargin) (第 1340 页)
      - [OnRspQrySPMMInstParam](#OnRspQrySPMMInstParam) (第 1346 页)
      - [OnRspQrySPMMProductParam](#OnRspQrySPMMProductParam) (第 1351 页)
      - [OnRspQrySPBMAddOnInterParameter](#OnRspQrySPBMAddOnInterParameter) (第 1356 页)
      - [OnRspQryRCAMSCombProductInfo](#OnRspQryRCAMSCombProductInfo) (第 1361 页)
      - [OnRspQryRCAMSInstrParameter](#OnRspQryRCAMSInstrParameter) (第 1366 页)
      - [OnRspQryRCAMSIntraParameter](#OnRspQryRCAMSIntraParameter) (第 1371 页)
      - [OnRspQryRCAMSInterParameter](#OnRspQryRCAMSInterParameter) (第 1376 页)
      - [OnRspQryRCAMSShortOptAdjustParam](#OnRspQryRCAMSShortOptAdjustParam) (第 1381 页)
      - [OnRspQryRCAMSInvestorCombPosition](#OnRspQryRCAMSInvestorCombPosition) (第 1386 页)
      - [OnRspQryInvestorProdRCAMSMargin](#OnRspQryInvestorProdRCAMSMargin) (第 1392 页)
      - [OnRspQryRULEInstrParameter](#OnRspQryRULEInstrParameter) (第 1399 页)
      - [OnRspQryRULEIntraParameter](#OnRspQryRULEIntraParameter) (第 1405 页)
      - [OnRspQryRULEInterParameter](#OnRspQryRULEInterParameter) (第 1410 页)
      - [OnRspQryInvestorProdRULEMargin](#OnRspQryInvestorProdRULEMargin) (第 1416 页)
      - [OnRspQryInvestorPortfSetting](#OnRspQryInvestorPortfSetting) (第 1423 页)
      - [OnRspQryInvestorInfoCommRec](#OnRspQryInvestorInfoCommRec) (第 1428 页)

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
无
回到顶部
< 前页 回目录 后页 >




---

## OnRspQryMulticastInstrument

OnRspQryMulticastInstrument
请求查询组播合约响应


◇ 1.函数原型
virtual void
OnRspQryMulticastInstrument(CThostFtdcMulticastInstrumentField
*pMulticastInstrument, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
MulticastInstrument：组播合约信息
    struct CThostFtdcMulticastInstrumentField
    {
        ///主题号
        TThostFtdcInstallIDType TopicID;
        ///合约代码
        TThostFtdcInstrumentIDType  InstrumentID;
        ///合约编号
        TThostFtdcInstallIDType InstrumentNo;
        ///基准价
        TThostFtdcPriceType CodePrice;
        ///合约数量乘数
        TThostFtdcVolumeMultipleType    VolumeMultiple;
        ///最小变动价位
        TThostFtdcPriceType PriceTick;
    };
pRspInfo：错误响应
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

## OnRspSubForQuoteRsp

OnRspSubForQuoteRsp
订阅询价应答，当调用SubscribeForQuoteRsp后，通过此接口返
回。


◇ 1.函数原型
virtual void
OnRspSubForQuoteRsp(CThostFtdcSpecificInstrumentField
*pSpecificInstrument, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pSpecificInstrument：指定的合约
struct CThostFtdcSpecificInstrumentField
{
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
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

## OnRspSubMarketData

OnRspSubMarketData
订阅行情应答，调用SubscribeMarketData后，通过此接口返回。


◇ 1.函数原型
virtual void
OnRspSubMarketData(CThostFtdcSpecificInstrumentField
*pSpecificInstrument, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pSpecificInstrument：指定的合约
struct CThostFtdcSpecificInstrumentField
{
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
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

## OnRspUnSubForQuoteRsp

OnRspUnSubForQuoteRsp
取消订阅询价应答，当UnSubscribeForQuoteRsp后，调用此接
口。


◇ 1.函数原型
virtual void
OnRspUnSubForQuoteRsp(CThostFtdcSpecificInstrumentField
*pSpecificInstrument, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pSpecificInstrument：指定的合约
struct CThostFtdcSpecificInstrumentField
{
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
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

## OnRspUnSubMarketData

OnRspUnSubMarketData
取消订阅行情应答，当UnSubscribeMarketData后，调用此接口。


◇ 1.函数原型
virtual void
OnRspUnSubMarketData(CThostFtdcSpecificInstrumentField
*pSpecificInstrument, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部


◇ 2.参数
pSpecificInstrument：指定的合约
struct CThostFtdcSpecificInstrumentField
{
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
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

## OnRspUserLogin

OnRspUserLogin
登录请求响应，当ReqUserLogin后，该方法被调用。


◇ 1.函数原型
virtual void OnRspUserLogin(CThostFtdcRspUserLoginField
*pRspUserLogin, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pRspUserLogin：用户登录应答
struct CThostFtdcRspUserLoginField
{
    ///交易日
    TThostFtdcDateType TradingDay;
    ///登录成功时间
    TThostFtdcTimeType LoginTime;
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///用户代码
    TThostFtdcUserIDType UserID;
    ///交易系统名称
    TThostFtdcSystemNameType SystemName;
    ///前置编号
    TThostFtdcFrontIDType FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///最大报单引用
    TThostFtdcOrderRefType MaxOrderRef;
    ///上期所时间
    TThostFtdcTimeType SHFETime;
    ///大商所时间
    TThostFtdcTimeType DCETime;
    ///郑商所时间
    TThostFtdcTimeType CZCETime;
    ///中金所时间
    TThostFtdcTimeType FFEXTime;
    ///能源中心时间
    TThostFtdcTimeType INETime;
    ///后台版本信息


    TThostFtdcSysVersionType    SysVersion;
    ///广期所时间
    TThostFtdcTimeType  GFEXTime;
    ///当前登录中心号
    TThostFtdcDRIdentityIDType  LoginDRIdentityID;
    ///用户所属中心号
    TThostFtdcDRIdentityIDType  UserDRIdentityID;
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

## OnRspUserLogout

OnRspUserLogout
登出请求响应，当ReqUserLogout后，该方法被调用。


◇ 1.函数原型
virtual void OnRspUserLogout(CThostFtdcUserLogoutField
*pUserLogout, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};
回到顶部


◇ 2.参数
pUserLogout：用户登出请求
struct CThostFtdcUserLogoutField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///用户代码
    TThostFtdcUserIDType UserID;
};
pRspInfo：响应信息
struct CThostFtdcRspInfoField
{
    ///错误代码
    TThostFtdcErrorIDType ErrorID;
    ///错误信息
    TThostFtdcErrorMsgType ErrorMsg;
};
nRequestID：返回用户操作请求的ID，该ID由用户在操作请求
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

## OnRtnDepthMarketData

OnRtnDepthMarketData
深度行情通知，当SubscribeMarketData订阅行情后，行情通知由
此推送。


◇ 1.函数原型
virtual void
OnRtnDepthMarketData(CThostFtdcDepthMarketDataField
*pDepthMarketData) {};
回到顶部


◇ 2.参数
pDepthMarketData：深度行情
struct CThostFtdcDepthMarketDataField
{
    ///交易日
    TThostFtdcDateType   TradingDay;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///交易所代码
    TThostFtdcExchangeIDType ExchangeID;
    ///合约在交易所的代码
    TThostFtdcExchangeInstIDType    ExchangeInstID;
    ///最新价
    TThostFtdcPriceType  LastPrice;
    ///上次结算价
    TThostFtdcPriceType  PreSettlementPrice;
    ///昨收盘
    TThostFtdcPriceType  PreClosePrice;
    ///昨持仓量
    TThostFtdcLargeVolumeType   PreOpenInterest;
    ///今开盘
    TThostFtdcPriceType  OpenPrice;
    ///最高价
    TThostFtdcPriceType  HighestPrice;
    ///最低价
    TThostFtdcPriceType  LowestPrice;
    ///数量
    TThostFtdcVolumeType Volume;
    ///成交金额
    TThostFtdcMoneyType  Turnover;
    ///持仓量


    TThostFtdcLargeVolumeType   OpenInterest;
    ///今收盘
    TThostFtdcPriceType  ClosePrice;
    ///本次结算价
    TThostFtdcPriceType  SettlementPrice;
    ///涨停板价
    TThostFtdcPriceType  UpperLimitPrice;
    ///跌停板价
    TThostFtdcPriceType  LowerLimitPrice;
    ///昨虚实度
    TThostFtdcRatioType  PreDelta;
    ///今虚实度
    TThostFtdcRatioType  CurrDelta;
    ///最后修改时间
    TThostFtdcTimeType   UpdateTime;
    ///最后修改毫秒
    TThostFtdcMillisecType   UpdateMillisec;
    ///申买价一
    TThostFtdcPriceType  BidPrice1;
    ///申买量一
    TThostFtdcVolumeType BidVolume1;
    ///申卖价一
    TThostFtdcPriceType  AskPrice1;
    ///申卖量一
    TThostFtdcVolumeType AskVolume1;
    ///申买价二
    TThostFtdcPriceType  BidPrice2;
    ///申买量二
    TThostFtdcVolumeType BidVolume2;
    ///申卖价二
    TThostFtdcPriceType  AskPrice2;
    ///申卖量二
    TThostFtdcVolumeType AskVolume2;
    ///申买价三


    TThostFtdcPriceType  BidPrice3;
    ///申买量三
    TThostFtdcVolumeType BidVolume3;
    ///申卖价三
    TThostFtdcPriceType  AskPrice3;
    ///申卖量三
    TThostFtdcVolumeType AskVolume3;
    ///申买价四
    TThostFtdcPriceType  BidPrice4;
    ///申买量四
    TThostFtdcVolumeType BidVolume4;
    ///申卖价四
    TThostFtdcPriceType  AskPrice4;
    ///申卖量四
    TThostFtdcVolumeType AskVolume4;
    ///申买价五
    TThostFtdcPriceType  BidPrice5;
    ///申买量五
    TThostFtdcVolumeType BidVolume5;
    ///申卖价五
    TThostFtdcPriceType  AskPrice5;
    ///申卖量五
    TThostFtdcVolumeType AskVolume5;
    ///当日均价
    TThostFtdcPriceType  AveragePrice;
    ///业务日期
    TThostFtdcDateType   ActionDay;
    ///上带价
    TThostFtdcPriceType BandingUpperPrice;
    ///下带价
    TThostFtdcPriceType BandingLowerPrice;
};
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
行情中有些字段出现极大值，为什么？
行情通知中，例如结算价、收盘价、买卖价出现doubl
e极大值，则表示该字段没有值。例如涨停板的时候，因
为没有卖价，会给出一个double极大值，同时卖量为0。
郑商所的结算价在盘中也会送出，但数值在盘中不会变
化，这和盘后推送的结算价怎么区分？
郑商所夜盘收盘后会推送结算价，这也就是早上行情
中有可能会收到结算价的原因，盘中行情中推送的结算价
数值不会发生变化。无夜盘的合约，盘中不会收到结算
价。白盘收盘后郑商所也会推送所有合约最新的结算价，
CTP对交易所推送的结算价不做处理，直接转发。
郑商所的UDP行情里，Tradingday和AveragePrice字段都
是空的，但是TCP行情里有，这是为什么？
UDP行情不推送AveragePrice字段；TCP行情里的Tradi
ngday和AveragePrice是CTP给的。
行情中哪些字段是CTP自己计算发出的？
郑商所的套利合约的涨跌停板价由CTP计算，交易所
不发
成交总额郑商所不推送由CTP计算，另外cffex,dce,shf
e,ine这几家的成交均价不推送，由ctp计算。
发现连上前置后，为什么收到的行情涨跌停价是0？
连上的如果是mdfront的话可能mdfront是级联的，也
就是说上一级还是mdfront这两个mdfront如果版本不对，


也同样会导致这个问题
订阅后大商所组合合约行情不跳动，其他交易终端（闪
电王、快期等）的组合合约都有行情且在跳跃，为什
么？
大商所只会推送单腿合约的行情，组合合约的行情需
要下端自行计算。其他终端则是实时计算两条腿的价差，
故行情看上去一直跳跃。
行情中为什么T日的OpenInterest和T+1日的
PreOpenInterest手数不一致？
交易所有大笔报单产生，大笔报单不是通过交易方式
报出的。不在盘中行情中体现，盘中交易所会公示出来，
盘后才汇总到行情中。
回到顶部
< 前页 回目录 后页 >




---

## OnRtnForQuoteRsp

OnRtnForQuoteRsp
询价通知，使用SubscribeForQuoteRsp订阅该询价通知。私有流回
报。


◇ 1.函数原型
virtual void OnRtnForQuoteRsp(CThostFtdcForQuoteRspField
*pForQuoteRsp) {};
回到顶部


◇ 2.参数
pForQuoteRsp：发给做市商的询价请求
struct CThostFtdcForQuoteRspField
{
    ///交易日
    TThostFtdcDateType TradingDay;
    ///合约代码
    TThostFtdcInstrumentIDType InstrumentID;
    ///询价编号
    TThostFtdcOrderSysIDType ForQuoteSysID;
    ///询价时间
    TThostFtdcTimeType ForQuoteTime;
    ///业务日期
    TThostFtdcDateType ActionDay;
    ///交易所代码
    TThostFtdcExchangeIDType ExchangeID;
};
ForQuoteSysID：交易所给的一个询价编号，以此定位一笔询
价。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >


交易接口
■ 6.7.8_API接口说明
└◆ 交易接口


◇ 1.说明
交易员API提供了两个接口，分别为CThostFtdcTraderApi和
CThostFtdcTraderSpi。这两个接口对FTD协议进行了封装，方便客
户端应用程序的开发。客户端应用程序可以通过
CThostFtdcTraderApi发出操作请求，通过继承CThostFtdcTraderSpi
并重载回调函数来处理后台服务的响应。
交易员客户端应用程序至少由两个线程组成，一个是应用程序
主线程，一个是交易员API工作线程。应用程序与交易系统的通讯
是由 API工作线程驱动的。CThostFtdcTraderApi提供的接口是线程
安全的，可以有多个应用程序线程同时发出请求。
CThostFtdcTraderSpi提供的接口回调是由API工作线程驱动，通过实
现SPI中的接口方法，可以从交易托管系统收取所需数据。
如果重载的某个回调函数阻塞，则等于阻塞了API工作线程，
API与交易系统的通讯会停止。因此，在 CThostFtdcTraderSpi 派生
类的回调函数中，通常应迅速返回，可以利用将数据放入缓冲区或
通过Windows的消息机制来实现。
交易员 API 在运行过程中，会将一些数据写入本地文件中。调
用CreateFtdcTraderApi函数，可以传递一个参数，指明存贮本地文件
的路径。该路径必须在运行前已创建好。本地文件的扩展名都
是”.con”。
注意以下事项：
init和release不是线程安全的，多线程使用需要加锁
在CThostFtdcTraderSpi 派生类的回调函数中应及时返回，不
要阻塞。
API请求的输入参数不能为 NULL。


API请求的返回参数，0表示正确，其他表示错误，详细错误
编码请查error.xml。
API一个进程中linux最多开180-200左右的连接数，windows
最多开400左右连接数，想建立更多连接请多开进程。
CTP系统支持的通讯模式有三种：对话通讯模式，私有通讯
模式，广播通讯模式。详细说明见[通讯模式]
回到顶部


◇ 2.代码示例
下面通过一个简单的代码示例，带大家快速了解下交易API的
使用方法。该示例演示了API的初始化、订阅私有流、连接前置、
登录交易系统、报单和查询等过程。


◇ 2.1.源代码
01.     //以下是tradehandler.h文件
02.     
03.     #include "ThostFtdcTraderApi.h"
04.     
05.     #include "traderApi.h"
06.     
07.     #include <string.h>
08.     
09.     #include <stdio.h>
10.     
11.     #include <Windows.h>
12.     
13.      
14.     
15.     class CTraderHandler : public CThostFtdcTraderSpi
16.     
17.     {
18.     
19.     private:
20.     
21.       CThostFtdcTraderApi *m_ptraderapi;
22.     
23.      
24.     
25.     public:
26.     
27.       void connect()
28.     
29.       {
30.     


31.           //创建API实例
32.     
33.           m_ptraderapi = 
CThostFtdcTraderApi::CreateFtdcTraderApi(".//flow/"); //必须提前
创建好flow目录，流文件*.con就会在该文件夹下面创建，如果
想要区别不同session创建的流水文件名称可见如下示
例“.//flow/a_”
34.     
35.           m_ptraderapi->RegisterSpi(this);
36.     
37.           m_ptraderapi-
>SubscribePublicTopic(THOST_TERT_QUICK);
38.     
39.           m_ptraderapi-
>SubscribePrivateTopic(THOST_TERT_QUICK); //设置私有流订
阅模式
40.     
41.           m_ptraderapi->RegisterFront("tcp://127.0.0.1:41205");
42.     
43.           m_ptraderapi->Init();
44.     
45.           //输出API版本信息
46.     
47.           printf("%s\n", m_ptraderapi->GetApiVersion());
48.     
49.       }
50.     
51.      
52.     
53.       //释放
54.     
55.       void release()
56.     
57.       {


58.     
59.           m_ptraderapi->Release();
60.     
61.       }
62.     
63.      
64.     
65.       //登陆
66.     
67.       void login()
68.     
69.       {
70.     
71.           CThostFtdcReqUserLoginField t = {0};
72.     
73.           strcpy_s(t.BrokerID, "1701");
74.     
75.           strcpy_s(t.UserID, "1701_admin");
76.     
77.           strcpy_s(t.Password, "1701_admin");
78.     
79.           while (m_ptraderapi->ReqUserLogin(&t, 1)!=0)   
Sleep(1000);
80.     
81.       }
82.     
83.      
84.     
85.       //结算单确认
86.     
87.       void settlementinfoConfirm()
88.     
89.       {
