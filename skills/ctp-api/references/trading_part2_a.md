# Trading - CTP 6.7.8 API

本节包含 33 个主题。


## ReqQryTraderOffer

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

## ReqQrySPBMInvestorPortfDef

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




---

## ReqQryInvestorPortfMarginRatio

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




---

## ReqQryInvestorProdSPBMDetail

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




---

## ReqQryInvestorCommoditySPMMMargin

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

## ReqQryInvestorCommodityGroupSPMMMargin

ReqQryInvestorCommodityGroupSPM
MMargin
请求投资者商品群SPMM记录查询，对应响应请求
OnRspQryInvestorCommoditySPMMMargin


◇ 1.函数原型
virtual int
ReqQryInvestorCommodityGroupSPMMMargin(CThostFtdcQryInvestor
CommodityGroupSPMMMarginField
*pQryInvestorCommodityGroupSPMMMargin, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInvestorCommodityGroupSPMMMargin:投资者商品群
SPMM记录查询
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
TThostFtdcSPMMProductIDType CommodityGroupID
商品
群代
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
CThostFtdcQryInvestorCommodityGroupSPMMMarginField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.CommodityGroupID, "01");
m_pUserApi->ReqQryInvestorCommodityGroupSPMMMargin(&a, 
nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


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

## ReqQryRCAMSInvestorCombPosition

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




---

## ReqQryInvestorProdRCAMSMargin

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




---

## ReqQryInvestorProdRULEMargin

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




---

## ReqQryInvestorPortfSetting

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




---

## ReqQryInvestorInfoCommRec

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
询价响应有以下几种：
函数名称
说明
OnRspForQuoteInsert
只在录入错误的情况下返回，如字段校
验失败、被限制询价等；可以通过
ErrorID和ErrorMsg查看错误信息
OnRtnForQuoteRsp（交
易spi）
暂时不用
OnRtnForQuoteRsp（行
情spi）
录入成功后会推送，但是事先要调用行
情api里的SubscribeForQuoteRsp去订阅该
消息。
OnErrRtnForQuoteInsert
被交易所拒绝的询价，通过此接口返
回；可以通过ErrorID和ErrorMsg查看错
误信息
由于中金所、上期所和大商所的询价是来自交易所交易流，
而郑商所是来自交易所行情流，所以连接mdfront只能看到郑商所
的询价通知，连接行情front能收到所有交易所的询价通知。
3.查询询价


查询询价使用ReqQryForQuote函数。
接收查询结果使用OnRspQryForQuote函数
4.请求报价指令
请求报价指令使用ReqQuoteInsert函数。
上期所期货合约使用ReqOrderInsert报价
参数CThostFtdcInputQuoteField中，QuoteRef、AskOrderRef、
BidOrderRef非必填，如果手工填则要求AskOrderRef小于
BidOrderRef，否则会报“CTP：重复的报单”。
5.接收报价响应
报价响应有以下几种：
函数名称
说明
OnRspQuoteInsert
只在录入错误的情况下返回，如字段校验失
败、无做市商权限等；可以通过ErrorID和
ErrorMsg查看错误信息
OnRtnQuote
当报价录入成功后，CTP给出该报价响应
OnRtnOrder
报价录入成功后，如果是双边报价，CTP还会同
时衍生出买卖两笔报价衍生单报入交易所，并
返回相应的OnRtnOrder
OnRtnTrade
衍生单成交后，返回该成交回报
对于大商所，交易所在接受报价后，将不再维护报价状态的更
新，所以客户端在接收CTP的报价回报时，其报价状态没有完整生
命周期，因此采用CTP接口的开发人员需要重点关注报价对应衍生
报单的状态。上期所会维护报价的状态，但是跟报单状态独立，比
如，如果两腿报单都已经成交，依然可以撤报价单，并且能成功。


中金所的报价和撤销报价的时候，交易所会发送OnRtnQuote，会更
新为已撤销。但是如果单独撤销两个衍生单，中金所不再更新报价
的状态。
详见报价回调规则
6.撤销报价请求
撤销报价请求使用ReqQuoteAction函数。
参数CThostFtdcInputQuoteActionField中，使用
QuoteRef+SessionID+FrontID组合来撤单。
7.撤销报价衍生单
撤销报价衍生单使用ReqOrderAction函数。
参数CThostFtdcInputOrderActionField中，使用
FrontID+SessionID+OrderRef组合来撤单。
要注意的是，如果在报价的时候没有填写AskOrderRef和
BidOrderRef，那么报价衍生单的响应OnRtnOrder里的FrontID和
SessionID是0。
8.查询报价
查询报价使用ReqQryQuote函数。
接收查询结果使用OnRspQryQuote函数
回到顶部


◇ 2.四所区别
下面列出四所在询价和报价上的一些区别。注意：以下内容仅
供参考，如果交易所的规则发生变化，文档并不会实时更新。
1.四所询价对比
　
中金所
大商所
郑商所
上期所
询价间隔如何设置
交易所限
制
交易所限
制
CTP柜
台
交易所限
制
普通投资者是否可接收询
价申报
可以
可以
可以
可以
2.四所报价对比
　
中金所
大商所
郑商所
上期所
是否
支持
平仓
支持
支持
支持
支持
是否
必须
关联
询价
询价通知没
有询价编
号，不用填
询价通知有
编号，但是
非必填，如
做市商要完
成义务率则
要填
询价通知有编
号，但是非必
填，如做市商要
完成义务率则要
填
询价通知没
有询价编
号，不用填
是否
支持
单边
报价
不支持，提
示“CTP:报单
字段有误”
支持，数量
填0
不支持，提
示“CTP:报单字
段有误”
不支持，提
示“CTP:报单
字段有误”


是否
支持
市价
单
不支持，交
易所提
示：“已撤单
报单被拒绝
CFFEX:价格
跌破跌停板”
不支持，交
易所提示：
已撤单报单
被拒绝DCE:
期权价格必
须大于等于
TICK!
不支持，交易所
提示：“已撤单报
单被拒绝CZCE:
出错: 限价单价
格不能为0或负
数”
不支持
双边
报价
买卖
数量
可否
不一
致
可以不一致
可以不一致
CTP:不可以，提
示“CTP：报单字
段有误”
CTP:不可
以，提
示“CTP：报
单字段有误”
双边
报价
卖价
是否
必须
大于
买价
一定是低价
买，高价
卖。否则会
形成自成交
必须，否则
交易所提
示： 已撤单
报单被拒绝
DCE:双边报
价指令卖价
必须大于买
价
必须，否则交易
所提示：已撤单
报单被拒绝
CZCE: 出错：卖
价(40800)必需大
于买价(94800)
一定是低价
买，高价
卖。否则会
形成自成交
是否
支持
套保
属性
支持
支持
不支持，提
示“CTP：该期权
合约只支持投机
类型报单”
支持
是否
会撤
后台版本在
6.6.1以上后
不会撤销 接
入大商所七
会撤销
会撤销


销前
一次
报价
支持自动撤
销
期之后支持
自动撤销
是否
支持
撤单
腿
支持顶单功
能，不支持
撤单边衍生
单
可以
不可以
不可以
双边
报价
开平
可否
不一
致
可以
可以
可以
可以
回到顶部


◇ 3.OrderType和TradeType字段说明
一笔报价会有两笔报价衍生单（OnRtnOrder），报价成交后还
会有成交回报（OnRtnTrade）。OnRtnOrder的OrderType和
OnRtnTrade的TradeType取自交易所回报。
上期所：交易所不返回TradeType和OrderType，所以在收到交
易所回报后，两个字段都是空值。
大商所：报价会转换成两笔普通报单出去，所以TradeType和
OrderType都是0（普通成交）
中金所：交易所返回OrderType为1（报价衍生单），TradeType
为0（普通成交）
郑商所：交易所返回OrderType为1（报价衍生单），TradeType
为0（普通成交）
回到顶部
< 前页 回目录 后页 >


报单回调规则
