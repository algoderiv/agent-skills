# Api Reference - CTP 6.7.8 API

本节包含 107 个主题。


## ReqQryCombPromotionParam

ReqQryCombPromotionParam
请求组合优惠比例，对应响应请求
OnRspQryCombPromotionParam
详见 6.5.1版本更新说明补充说明


◇ 1.函数原型
virtual int
ReqQryCombPromotionParam(CThostFtdcQryCombPromotionParamFie
ld *pQryCombPromotionParam, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryCombPromotionParam：查询组合优惠比例
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
否
TThostFtdcInstrumentIDType InstrumentID 合约代
码
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


ReqQryRiskSettleInvstPosition
投资者风险结算持仓查询，对应响应请求
OnRspQryRiskSettleInvstPosition
详见 6.6.1P1版本更新说明


◇ 1.函数原型
virtual int
ReqQryRiskSettleInvstPosition(CThostFtdcQryRiskSettleInvstPositionFi
eld *pQryRiskSettleInvstPosition, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRiskSettleInvstPosition：投资者风险结算持仓查询
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
TThostFtdcInstrumentIDType InstrumentID 合约代码否
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

## ReqQryRiskSettleProductStatus

ReqQryRiskSettleProductStatus
风险结算产品查询，对应响应请求
OnRspQryRiskSettleProductStatus
详见 6.6.1P1版本更新说明


◇ 1.函数原型
virtual int
ReqQryRiskSettleProductStatus(CThostFtdcQryRiskSettleProductStatus
Field *pQryRiskSettleProductStatus, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRiskSettleProductStatus：风险结算产品查询
字段类型
字段名称
含义
是否可作为过滤
条件
TThostFtdcInstrumentIDType ProductID 产品代
码
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


ReqQryTraderOffer
请求查询交易员报盘机，对应响应请求OnRspQryTraderOffer


◇ 1.函数原型
virtual int ReqQryTraderOffer(CThostFtdcQryTraderOfferField
*pQryTraderOffer, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryTraderOffer：查询交易员报盘机
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcExchangeIDType
ExchangeID
交易所代
码
否
TThostFtdcParticipantIDType ParticipantID 会员代码
否
TThostFtdcTraderIDType
TraderID
交易所交
易员代码
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

## ReqQrySPBMFutureParameter

ReqQrySPBMFutureParameter
请求SPBM期货合约参数查询，对应响应请求
OnRspQrySPBMFutureParameter


◇ 1.函数原型
virtual int
ReqQrySPBMFutureParameter(CThostFtdcQrySPBMFutureParameterFi
eld *pQrySPBMFutureParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPBMFutureParameter：SPBM期货合约保证金参数查询
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcInstrumentIDType InstrumentID
合约代
码
是
TThostFtdcInstrumentIDType ProdFamilyCode 品种代
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

## ReqQrySPBMOptionParameter

ReqQrySPBMOptionParameter
请求SPBM期权合约参数查询，对应响应请求
OnRspQrySPBMOptionParameter


◇ 1.函数原型
virtual int
ReqQrySPBMOptionParameter(CThostFtdcQrySPBMOptionParameterF
ield *pQrySPBMOptionParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPBMOptionParameter：SPBM期权合约保证金参数查询
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcInstrumentIDType InstrumentID
合约代
码
是
TThostFtdcInstrumentIDType ProdFamilyCode 品种代
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

## ReqQrySPBMIntraParameter

ReqQrySPBMIntraParameter
请求SPBM品种内对锁仓折扣参数查询，对应响应请求
OnRspQrySPBMIntraParameter


◇ 1.函数原型
virtual int
ReqQrySPBMIntraParameter(CThostFtdcQrySPBMIntraParameterField
*pQrySPBMIntraParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPBMIntraParameter：SPBM品种内对锁仓折扣参数查询
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcInstrumentIDType ProdFamilyCode 品种代
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

## ReqQrySPBMInterParameter

ReqQrySPBMInterParameter
请求SPBM跨品种抵扣参数查询，对应响应请求
OnRspQrySPBMInterParameter


◇ 1.函数原型
virtual int
ReqQrySPBMInterParameter(CThostFtdcQrySPBMInterParameterField
*pQrySPBMInterParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPBMInterParameter：SPBM跨品种抵扣参数查询
字段类型
字段名称
含义
是否可
作为过
滤条件
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
是
TThostFtdcInstrumentIDType Leg1ProdFamilyCode
第一
腿构
成品
种
是
TThostFtdcInstrumentIDType Leg2ProdFamilyCode
第二
腿构
成品
种
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

## ReqQrySPBMPortfDefinition

ReqQrySPBMPortfDefinition
请求SPBM组合保证金套餐查询，对应响应请求
OnRspQrySPBMPortfDefinition


◇ 1.函数原型
virtual int
ReqQrySPBMPortfDefinition(CThostFtdcQrySPBMPortfDefinitionField
*pQrySPBMPortfDefinition, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPBMPortfDefinition：组合保证金套餐查询
字段类型
字段名称
含义
是否可
作为过
滤条件
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcPortfolioDefIDType PortfolioDefID
组合保
证金套
餐代码
是
TThostFtdcInstrumentIDType
ProdFamilyCode 品种代
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


ReqQrySPBMInvestorPortfDef
请求投资者SPBM套餐选择查询，对应响应请求
OnRspQrySPBMInvestorPortfDef


◇ 1.函数原型
virtual int
ReqQrySPBMInvestorPortfDef(CThostFtdcQrySPBMInvestorPortfDefFi
eld *pQrySPBMInvestorPortfDef, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPBMInvestorPortfDef：投资者套餐选择查询
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcExchangeIDType ExchangeID 交易所代
码
是
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


ReqQryInvestorPortfMarginRatio
请求投资者新型组合保证金系数查询，对应响应请求
OnRspQryInvestorPortfMarginRatio


◇ 1.函数原型
virtual int
ReqQryInvestorPortfMarginRatio(CThostFtdcQryInvestorPortfMarginRa
tioField *pQryInvestorPortfMarginRatio, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPortfMarginRatio：投资者新型组合保证金系数查询
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
TThostFtdcExchangeIDType ExchangeID
交易所
代码
是
TThostFtdcProductIDType
ProductGroupID 产品群
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


ReqQryInvestorProdSPBMDetail
请求投资者产品SPBM明细查询，对应响应请求
OnRspQryInvestorProdSPBMDetail


◇ 1.函数原型
virtual int
ReqQryInvestorProdSPBMDetail(CThostFtdcQryInvestorProdSPBMDet
ailField *pQryInvestorProdSPBMDetail, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorProdSPBMDetail：投资者产品SPBM明细查询
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
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
TThostFtdcInstrumentIDType ProdFamilyCode 品种代
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


ReqQryInvestorCommoditySPMMMarg
in
请求投资者商品组SPMM记录查询，对应响应请求
OnRspQryInvestorCommoditySPMMMargin


◇ 1.函数原型
virtual int
ReqQryInvestorCommoditySPMMMargin(CThostFtdcQryInvestorCom
moditySPMMMarginField *pQryInvestorCommoditySPMMMargin, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorCommoditySPMMMargin：投资者商品组SPMM记
录查询
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcSPMMProductIDType CommodityID 商品组
代码
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
CThostFtdcQryInvestorCommoditySPMMMarginField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.CommodityID, "cu&cu_o");
m_pUserApi->ReqQryInvestorCommoditySPMMMargin(&a, 
nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQrySPMMInstParam

ReqQrySPMMInstParam
请求SPMM合约参数查询，对应响应请求
OnRspQrySPMMInstParam


◇ 1.函数原型
virtual int
ReqQrySPMMInstParam(CThostFtdcQrySPMMInstParamField
*pQrySPMMInstParam, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPMMInstParam：SPMM合约参数查询
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcInstrumentIDType InstrumentID 合约代
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

## ReqQrySPMMProductParam

ReqQrySPMMProductParam
请求SPMM产品参数查询，对应响应请求
OnRspQrySPMMProductParam


◇ 1.函数原型
virtual int
ReqQrySPMMProductParam(CThostFtdcQrySPMMProductParamField
*pQrySPMMProductParam, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPMMProductParam：SPMM产品参数查询
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcSPMMProductIDType ProductID 产品代
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

## ReqQrySPBMAddOnInterParameter

ReqQrySPBMAddOnInterParameter
请求SPBM附加跨品种抵扣参数查询，对应响应请求
OnRspQrySPBMAddOnInterParameter


◇ 1.函数原型
virtual int
ReqQrySPBMAddOnInterParameter(CThostFtdcQrySPBMAddOnInterP
arameterField *pQrySPBMAddOnInterParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQrySPBMAddOnInterParameter：SPBM附加跨品种抵扣参数查
询
字段类型
字段名称
含义
是否可
作为过
滤条件
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
是
TThostFtdcInstrumentIDType Leg1ProdFamilyCode
第一
腿构
成品
种
是
TThostFtdcInstrumentIDType Leg2ProdFamilyCode
第二
腿构
成品
种
是
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

## ReqQryRCAMSCombProductInfo

ReqQryRCAMSCombProductInfo
请求RCAMS产品组合信息查询，对应响应请求
OnRspQryRCAMSCombProductInfo


◇ 1.函数原型
virtual int
ReqQryRCAMSCombProductInfo(CThostFtdcQryRCAMSCombProduc
tInfoField *pQryRCAMSCombProductInfo, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRCAMSCombProductInfo：RCAMS产品组合信息查询
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcProductIDType ProductID
产品代
码
是
TThostFtdcProductIDType CombProductID 商品组
代码
是
TThostFtdcProductIDType ProductGroupID 商品群
代码
是
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

## ReqQryRCAMSInstrParameter

ReqQryRCAMSInstrParameter
请求RCAMS同合约风险对冲参数查询，对应响应请求
OnRspQryRCAMSInstrParameter


◇ 1.函数原型
virtual int
ReqQryRCAMSInstrParameter(CThostFtdcQryRCAMSInstrParameterFi
eld *pQryRCAMSInstrParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRCAMSInstrParameter：RCAMS同合约风险对冲参数查询
字段类型
字段名称
含义
是否可作为过滤条
件
TThostFtdcProductIDType ProductID 产品代
码
是
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

## ReqQryRCAMSIntraParameter

ReqQryRCAMSIntraParameter
请求RCAMS品种内风险对冲参数查询，对应响应请求
OnRspQryRCAMSIntraParameter


◇ 1.函数原型
virtual int
ReqQryRCAMSIntraParameter(CThostFtdcQryRCAMSIntraParameterFi
eld *pQryRCAMSIntraParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRCAMSIntraParameter：RCAMS品种内风险对冲参数查询
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcProductIDType CombProductID 产品组合
代码
是
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

## ReqQryRCAMSInterParameter

ReqQryRCAMSInterParameter
请求RCAMS跨品种风险折抵参数查询，对应响应请求
OnRspQryRCAMSInterParameter


◇ 1.函数原型
virtual int
ReqQryRCAMSInterParameter(CThostFtdcQryRCAMSInterParameterFi
eld *pQryRCAMSInterParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRCAMSInterParameter：RCAMS跨品种风险折抵参数查询
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcProductIDType ProductGroupID 商品群代
码
是
TThostFtdcProductIDType CombProduct1
产品组合
代码1
是
TThostFtdcProductIDType CombProduct2
产品组合
代码2
是
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

## ReqQryRCAMSShortOptAdjustParam

ReqQryRCAMSShortOptAdjustParam
请求RCAMS空头期权风险调整参数查询，对应响应请求
OnRspQryRCAMSShortOptAdjustParam


◇ 1.函数原型
virtual int
ReqQryRCAMSShortOptAdjustParam(CThostFtdcQryRCAMSShortOpt
AdjustParamField *pQryRCAMSShortOptAdjustParam, int nRequestID)
= 0;
回到顶部


◇ 2.参数
pQryRCAMSShortOptAdjustParam：RCAMS空头期权风险调整
参数查询
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcProductIDType CombProductID 产品组合
代码
是
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


ReqQryRCAMSInvestorCombPosition
请求RCAMS策略组合持仓查询，对应响应请求
OnRspQryRCAMSInvestorCombPosition


◇ 1.函数原型
virtual int
ReqQryRCAMSInvestorCombPosition(CThostFtdcQryRCAMSInvestor
CombPositionField *pQryRCAMSInvestorCombPosition, int
nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRCAMSInvestorCombPosition：RCAMS策略组合持仓查询
字段类型
字段名称
含义
是否可作
为过滤条
件
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码是
TThostFtdcInvestorIDType
InvestorID
投资者
代码
是
TThostFtdcInstrumentIDType InstrumentID
合约代
码
是
TThostFtdcInstrumentIDType CombInstrumentID 组合合
约代码是
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


ReqQryInvestorProdRCAMSMargin
请求投资者品种RCAMS保证金查询，对应响应请求
OnRspQryInvestorProdRCAMSMargin


◇ 1.函数原型
virtual int
ReqQryInvestorProdRCAMSMargin(CThostFtdcQryInvestorProdRCAM
SMarginField *pQryInvestorProdRCAMSMargin, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorProdRCAMSMargin：投资者品种RCAMS保证金查
询
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
TThostFtdcInvestorIDType InvestorID
投资者
代码
是
TThostFtdcProductIDType CombProductID 产品组
合代码
是
TThostFtdcProductIDType ProductGroupID 商品群
代码
是
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

## ReqQryRULEInstrParameter

ReqQryRULEInstrParameter
请求RULE合约保证金参数查询，对应响应请求
OnRspQryRULEInstrParameter


◇ 1.函数原型
virtual int
ReqQryRULEInstrParameter(CThostFtdcQryRULEInstrParameterField
*pQryRULEInstrParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRULEInstrParameter：RULE合约保证金参数查询
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcInstrumentIDType InstrumentID 合约代
码
是
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

## ReqQryRULEIntraParameter

ReqQryRULEIntraParameter
请求RULE品种内对锁仓折扣参数查询，对应响应请求
OnRspQryRULEIntraParameter


◇ 1.函数原型
virtual int
ReqQryRULEIntraParameter(CThostFtdcQryRULEIntraParameterField
*pQryRULEIntraParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRULEIntraParameter：RULE品种内对锁仓折扣参数查询
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcInstrumentIDType ProdFamilyCode 品种代
码
是
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

## ReqQryRULEInterParameter

ReqQryRULEInterParameter
请求RULE跨品种抵扣参数查询，对应响应请求
OnRspQryRULEInterParameter


◇ 1.函数原型
virtual int
ReqQryRULEInterParameter(CThostFtdcQryRULEInterParameterField
*pQryRULEInterParameter, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryRULEInterParameter：RULE跨品种抵扣参数查询
字段类型
字段名称
含
义
是
否
可
作
为
过
滤
条
件
TThostFtdcExchangeIDType
ExchangeID
交
易
所
代
码
是
TThostFtdcInstrumentIDType
Leg1ProdFamilyCode
第
一
腿
构
成
品
种
是
TThostFtdcInstrumentIDType
Leg1ProdFamilyCode 第
二
腿
是


构
成
品
种
TThostFtdcCommodityGroupIDType CommodityGroupID
商
品
群
号
是
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


ReqQryInvestorProdRULEMargin
请求投资者产品RULE保证金查询，对应响应请求
OnRspQryInvestorProdRULEMargin


◇ 1.函数原型
virtual int
ReqQryInvestorProdRULEMargin(CThostFtdcQryInvestorProdRULEM
arginField *pQryInvestorProdRULEMargin, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorProdRULEMargin：投资者产品RULE保证金查询
字段类型
字段名称
含
义
是否
可作
为过
滤条
件
TThostFtdcExchangeIDType
ExchangeID
交
易
所
代
码
是
TThostFtdcBrokerIDType
BrokerID
经
纪
公
司
代
码
是
TThostFtdcInvestorIDType
InvestorID
投
资
者
代
码
是
TThostFtdcInstrumentIDType
ProdFamilyCode
品
种
是


代
码
TThostFtdcCommodityGroupIDType CommodityGroupID
商
品
群
号
是
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


ReqQryInvestorPortfSetting
投资者新型组合保证金开关查询，对应响应请求
OnRspQryInvestorPortfSetting


◇ 1.函数原型
virtual int
ReqQryInvestorPortfSetting(CThostFtdcQryInvestorPortfSettingField
*pQryInvestorPortfSetting, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPortfSetting：投资者新组保设置查询
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcExchangeIDType ExchangeID 交易所代
码
是
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


ReqQryInvestorInfoCommRec
投资者申报费阶梯收取记录查询，对应响应请求
OnRspQryInvestorInfoCommRec
请求查询时3个入参都支持为空，查询期权系列的申报费时通过填
写标的的商品代码查询。同一个投资者、同一个商品代码可能会返回
两条记录：
一条记录为期货合约的申报费收取记录，IsOptSeries字段返回为
0；
一条记录为以此期货合约为标的的系列期权的申报费收取记录，
IsOptSeries字段返回为1。


◇ 1.函数原型
virtual int
ReqQryInvestorPortfSetting(CThostFtdcQryInvestorPortfSettingField
*pQryInvestorPortfSetting, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorPortfSetting：投资者申报费阶梯收取记录查询
字段类型
字段名称
含义
是否可作为
过滤条件
TThostFtdcInstrumentIDType InstrumentID 交易所代
码
是
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

## 其他业务规则

其他业务规则
■ 6.7.8_API接口说明
└◆ 其他业务规则
请参阅：
    做市商询价和报价
    报单回调规则
    大商所组保
    报价回调规则
    协议方式接收二代组播行情
    fens连接说明
    各交易所行情区别
    合约状态变化说明
    报单流控、查询流控和会话数控制
    通讯模式
    TAS介绍
    条件单规则
    各交易所特殊指令
    接口中一些重要序号说明
    期货期权的行权、自对冲
    行情流控


    TGate网关接入指南
回到顶部
< 前页 回目录 后页 >




---

## 做市商询价和报价

做市商询价和报价


◇ 1.指令介绍
1.请求询价指令
发起询价指令使用ReqForQuoteInsert函数。
参数CThostFtdcInputForQuoteField中，ForQuoteRef非必填，可
由CTP交易核心自动生成。
交易所有询价价差的限制，期货公司可以在柜台上进行设置，
一般如下
经纪公司代码
合约代码
交易所代码
最新价
价差
1008
SRC
CZCE
0
8
1008
SRC
CZCE
50
10
1008
SRC
CZCE
100
20
1008
SRC
CZCE
200
30
1008
SRC
CZCE
300
50
1008
SRC
CZCE
500
75
询价价差的判断过程
1) 看最新价对应于上面的哪一档次，确定价差的最小值
2) 计算买价和卖价的价差，看是否大于设置的价差（等于也不
行）
3) 如果2)通过，那么询价单报入交易所，否则会被CTP直接拒
绝。
如果询价的时候，当前合约价差不在范围，则报“CTP：当前价
差禁止询价”。


另外，交易所还有询价时间间隔限制，一般在交易所端有控
制，但郑商所控制在CTP柜台端，柜台设置例如：
对于郑商所、中金所，如果询价过于频繁，则会报“CTP：当前
时间禁止询价”。其他交易所报错类似。
中金所询价开放了交易编码类型，支持“投机，套保，套利”的
编码询价。
2.接收询价响应
