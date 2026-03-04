*pRspInfo, int nRequestID, bool bIsLast) {};


◇ 3.4. API与前置传输过程中的数据压缩
支持API与前置传输过程中的数据压缩，该变更对API使用者
来说无影响。CTP后台系统在V6.6.1_P1版本支持。
回到顶部
< 前页 回目录 后页 >




---

## 6.6.5版本更新说明

6.6.5版本更新说明
版本号：v6.6.5_20210924_traderapi
后台版本：v6.6.5
补充变更说明：（后台系统兼容线上旧版本API接入，请各终端
厂商根据自身情况进行开发）


◇ 1. API变动


◇ 1.1. 期权自对冲回报
OnRtnOptionSelfClose中ActiveUserID字段处
理方式更新。
期权自对冲回报OnRtnOptionSelfClose（私有流）报文中
ActiveUserID[操作用户代码]字段值处理如下：
1. 委托回报报文中此字段值均为空，无论是否是首次委托
其请求报文中此字段值也置为空；
2. 撤单回报报文中此字段值有值，同撤单请求报文。


◇ 1.2. 交易TraderAPI增加一个错误码。
<error id ="CFMMC_NO_CONNECTION" value="155"
prompt="CTP:未连接监控中心">
举例：当调用查询保监中心的结算单ReqQrySettlementInfo接
口时，查询到m_cfmmcofferInstallID==0时，即该席位未连接监控
中心的cfmmcoffer，响应报文中返回该错误代码，给客户清晰的
报错提示。


◇ 1.3. 交易TraderAPI登录回报报文
OnRspUserLogin，增加返回柜台的版本号
SysVersion。
///用户登录应答
struct CThostFtdcRspUserLoginField
{
    ///交易日
    String  TradingDay;
    ///登录成功时间
    String  LoginTime;
    ///经纪公司代码
    String  BrokerID;
    ///用户代码
    String  UserID;
    ///交易系统名称
    String  SystemName;
    ///前置编号
    int FrontID;
    ///会话编号
    int SessionID;
    ///最大报单引用
    String  MaxOrderRef;
    ///上期所时间
    String  SHFETime;
    ///大商所时间
    String  DCETime;
    ///郑商所时间
    String  CZCETime;
    ///中金所时间
    String  FFEXTime;


    ///能源中心时间
    String  INETime;
    //后台版本信息
    String  SysVersion;
};
举例：客户登录成功后，调用OnRspUserLogin接口，返回登
录成功的回报报文，报文中SysVersion即柜台版本号，如V6.6.5。
回到顶部
< 前页 回目录 后页 >




---

## 6.6.7版本更新说明

6.6.7版本更新说明
版本号：v6.6.7_20220613
后台版本：V6.6.7
补充变更说明：（后台系统兼容线上旧版本API接入，请各终端
厂商根据自身情况进行开发）


◇ 1.API变动


◇ 1.1.新增CTP_GetDataCollectApiVersion接
口
采集库新增CTP_GetDataCollectApiVersion接口获取采集库版
本号：
const char * CTP_GetDataCollectApiVersion(void);


◇ 1.2.SubmitUserSystemInfo增加错误返回
SubmitUserSystemInfo函数增加错误返回值：-7，对应错误信
息为“api and collect api version dismatch ”
采集库版本与API版本不匹配时输出该错误码（举例采集库
为评测版，API为生产版）


◇ 1.3.CTP_GetSystemInfo增加返回值错误码
DATA_COLLECT_API_EXPORT int CTP_GetSystemInfo(char*
pSystemInfo, int& nLen)在调用采集信息上报的时候，增加检查密
文长度是否正常，不正常直接返回错误。长度大于等于264则放
过。获取AES加密和RSA加密的终端信息pSystemInfo的空间需要
调用者自己分配至少270个字节。
Windows返回值定义：
    返回值 & （0x01 << 0） 不为0 表示终端类型未采集到
    返回值 & （0x01 << 1） 不为0 表示 信息采集时间获取异常
    返回值 & （0x01 << 2） 不为0 表示ip 获取失败  （采集多个相
同类型信息的场景有一个采集到 即表示采集成功）
    返回值 & （0x01 << 3） 不为0 表示mac 获取失败
    返回值 & （0x01 << 4） 不为0 表示 设备名 获取失败
    返回值 & （0x01 << 5） 不为0 表示 操作系统版本 获取失败
    返回值 & （0x01 << 6） 不为0 表示 硬盘序列号 获取失败
    返回值 & （0x01 << 7） 不为0 表示 CPU序列号 获取失败
    返回值 & （0x01 << 8） 不为0 表示 BIOS 获取失败
    返回值 & （0x01 << 9） 不为0 表示 系统盘分区信息 获取失败
Linux返回值定义：     
    返回值 & （0x01 << 0） 不为0 表示终端类型未采集到
    返回值 & （0x01 << 1） 不为0 表示 信息采集时间获取异常
    返回值 & （0x01 << 2） 不为0 表示ip 获取失败  （采集多个相
同类型信息的场景有一个采集到 即表示采集成功）
    返回值 & （0x01 << 3） 不为0 表示mac 获取失败
    返回值 & （0x01 << 4） 不为0 表示 设备名 获取失败
    返回值 & （0x01 << 5） 不为0 表示 操作系统版本 获取失败
    返回值 & （0x01 << 6） 不为0 表示 硬盘序列号 获取失败
    返回值 & （0x01 << 7） 不为0 表示 CPU序列号 获取失败
    返回值 & （0x01 << 8） 不为0 表示 BIOS 获取失败


◇ 1.4.新增命令获取云服务器硬盘序列号信息
新增system("wmic diskdrive get serialnumber")命令获取CPU、
硬盘序列号等信息。


◇ 1.5.启用OnHeartBeatWarning心跳超时回
调
当60s没有收到心跳时，支持回调该函数，客户端可提前知道
网络断开等场景（前提是客户端需要实现该接口）。


◇ 1.6.OnRspQryInvestor和
OnRspQryProduct接口新增字段
OnRspQryInvestor和OnRspQryProduct接口新增频繁报撤单和
日内开仓限制相关字段。具体限制的阈值请咨询期货公司柜台设
置。
需要柜台版本6.6.6版本及以上支持。
回到顶部
< 前页 回目录 后页 >




---

## 6.6.8版本更新说明

6.6.8版本更新说明
版本号：v6.6.8
后台版本：V6.6.8
补充变更说明：（后台系统兼容线上旧版本API接入，请各终端
厂商根据自身情况进行开发）


◇ 1.API变动
用户登录应答CThostFtdcRspUserLoginField增加广期所时间
///用户登录应答
struct CThostFtdcRspUserLoginField
{
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///登录成功时间
    TThostFtdcTimeType  LoginTime;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///交易系统名称
    TThostFtdcSystemNameType    SystemName;
    ///前置编号
    TThostFtdcFrontIDType   FrontID;
    ///会话编号
    TThostFtdcSessionIDType SessionID;
    ///最大报单引用
    TThostFtdcOrderRefType  MaxOrderRef;
    ///上期所时间
    TThostFtdcTimeType  SHFETime;
    ///大商所时间
    TThostFtdcTimeType  DCETime;
    ///郑商所时间
    TThostFtdcTimeType  CZCETime;
    ///中金所时间
    TThostFtdcTimeType  FFEXTime;
    ///能源中心时间
    TThostFtdcTimeType  INETime;


    ///后台版本信息
    TThostFtdcSysVersionType    SysVersion;
    ///广期所时间
    TThostFtdcTimeType  GFEXTime;
};
回到顶部
< 前页 回目录 后页 >




---

## 6.6.9版本更新说明

6.6.9版本更新说明
版本号：v6.6.9_20220914_traderapi
后台版本：V6.6.9
注意：此版本为支持郑商所SPBM组保业务而变更，新增8个交易
查询接口。
变更说明：（请各终端厂商根据自身情况进行开发）


◇ 1.API变动


◇ 1.1.新增接口：SPBM期货合约参数查询
（查询对冲参数X、Y、Z）
（1）查询请求
///SPBM期货合约参数查询
virtual int ReqQrySPBMFutureParameter(CThostFtdcQrySPB
MFutureParameterField *pQrySPBMFutureParameter, int nRequestI
D) = 0;
struct CThostFtdcQrySPBMFutureParameterField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
};
（2）查询响应
///SPBM期货合约参数查询响应
virtual void OnRspQrySPBMFutureParameter(CThostFtdcSPB
MFutureParameterField *pSPBMFutureParameter, CThostFtdcRspIn
foField *pRspInfo, int nRequestID, bool bIsLast) {};
struct CThostFtdcSPBMFutureParameterField
{
    ///交易日


    TThostFtdcDateType  TradingDay;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
    ///期货合约因子
    TThostFtdcVolumeMultipleType    Cvf;
    ///阶段标识
    TThostFtdcTimeRangeType TimeRange;
    ///品种保证金标准
    TThostFtdcRatioType MarginRate;
    ///期货合约内部对锁仓费率折扣比例
    TThostFtdcRatioType LockRateX;
    ///提高保证金标准
    TThostFtdcRatioType AddOnRate;
    ///昨结算价
    TThostFtdcPriceType PreSettlementPrice;
};


◇ 1.2.新增接口：SPBM期权合约参数查询
（1）查询请求
///SPBM期权合约参数查询
virtual int ReqQrySPBMOptionParameter(CThostFtdcQrySPB
MOptionParameterField *pQrySPBMOptionParameter, int nRequestI
D) = 0;
struct CThostFtdcQrySPBMOptionParameterField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
};
（2）查询响应
///SPBM期权合约参数查询响应
virtual void OnRspQrySPBMOptionParameter(CThostFtdcSPB
MOptionParameterField *pSPBMOptionParameter, CThostFtdcRspI
nfoField *pRspInfo, int nRequestID, bool bIsLast) {};
struct CThostFtdcSPBMOptionParameterField
{
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///交易所代码


    TThostFtdcExchangeIDType    ExchangeID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
    ///期权合约因子
    TThostFtdcVolumeMultipleType    Cvf;
    ///期权冲抵价格
    TThostFtdcPriceType DownPrice;
    ///Delta值
    TThostFtdcDeltaType Delta;
    ///卖方期权风险转换最低值
    TThostFtdcDeltaType SlimiDelta;
    ///昨结算价
    TThostFtdcPriceType PreSettlementPrice;
};


◇ 1.3.新增接口：SPBM品种内对锁仓折扣参
数查询
（1）查询请求
///SPBM品种内对锁仓折扣参数查询
virtual int ReqQrySPBMIntraParameter(CThostFtdcQrySPBMI
ntraParameterField *pQrySPBMIntraParameter, int nRequestID) = 0;
struct CThostFtdcQrySPBMIntraParameterField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
};
（2）查询响应
///SPBM品种内对锁仓折扣参数查询响应
virtual void OnRspQrySPBMIntraParameter(CThostFtdcSPBMI
ntraParameterField *pSPBMIntraParameter, CThostFtdcRspInfoFiel
d *pRspInfo, int nRequestID, bool bIsLast) {};
///SPBM品种内对锁仓折扣参数
struct CThostFtdcSPBMIntraParameterField
{
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///交易所代码


    TThostFtdcExchangeIDType    ExchangeID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
    ///品种内合约间对锁仓费率折扣比例
    TThostFtdcRatioType IntraRateY;
};


◇ 1.4.新增接口：SPBM跨品种抵扣参数查询
（1）查询请求
///SPBM跨品种抵扣参数查询
virtual int ReqQrySPBMInterParameter(CThostFtdcQrySPBMI
nterParameterField *pQrySPBMInterParameter, int nRequestID) = 0;
struct CThostFtdcQrySPBMInterParameterField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///第一腿构成品种
    TThostFtdcInstrumentIDType  Leg1ProdFamilyCode;
    ///第二腿构成品种
    TThostFtdcInstrumentIDType  Leg2ProdFamilyCode;
};
（2）查询响应
///SPBM跨品种抵扣参数查询响应
virtual void OnRspQrySPBMInterParameter(CThostFtdcSPBMI
nterParameterField *pSPBMInterParameter, CThostFtdcRspInfoFiel
d *pRspInfo, int nRequestID, bool bIsLast) {};
///SPBM跨品种抵扣参数
struct CThostFtdcSPBMInterParameterField
{
    ///交易日
    TThostFtdcDateType  TradingDay;
    ///交易所代码


    TThostFtdcExchangeIDType    ExchangeID;
    ///优先级
    TThostFtdcSpreadIdType  SpreadId;
    ///品种间对锁仓费率折扣比例
    TThostFtdcRatioType InterRateZ;
    ///第一腿构成品种
    TThostFtdcInstrumentIDType  Leg1ProdFamilyCode;
    ///第二腿构成品种
    TThostFtdcInstrumentIDType  Leg2ProdFamilyCode;
};


◇ 1.5.新增接口：SPBM组合保证金套餐查询
（1）查询请求
///SPBM组合保证金套餐查询
virtual int ReqQrySPBMPortfDefinition(CThostFtdcQrySPBM
PortfDefinitionField *pQrySPBMPortfDefinition, int nRequestID) =
0;
struct CThostFtdcQrySPBMPortfDefinitionField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///组合保证金套餐代码
    TThostFtdcPortfolioDefIDType    PortfolioDefID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
};
（2）查询响应
///SPBM组合保证金套餐查询响应
virtual void OnRspQrySPBMPortfDefinition(CThostFtdcSPBM
PortfDefinitionField *pSPBMPortfDefinition, CThostFtdcRspInfoFie
ld *pRspInfo, int nRequestID, bool bIsLast) {};
///组合保证金套餐
struct CThostFtdcSPBMPortfDefinitionField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;


    ///组合保证金套餐代码
    TThostFtdcPortfolioDefIDType    PortfolioDefID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
    ///是否启用SPBM
    TThostFtdcBoolType  IsSPBM;
};


◇ 1.6.新增接口：投资者SPBM套餐选择查询
（操作员可以查所有投资者，投资者只能查
自己的）
（1）查询请求
///投资者SPBM套餐选择查询
virtual int ReqQrySPBMInvestorPortfDef(CThostFtdcQrySPB
MInvestorPortfDefField *pQrySPBMInvestorPortfDef, int nRequestI
D) = 0;
struct CThostFtdcQrySPBMInvestorPortfDefField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
};
（2）查询响应
///投资者SPBM套餐选择查询响应
virtual void OnRspQrySPBMInvestorPortfDef(CThostFtdcSPB
MInvestorPortfDefField *pSPBMInvestorPortfDef, CThostFtdcRspI
nfoField *pRspInfo, int nRequestID, bool bIsLast) {};


///投资者套餐选择
struct CThostFtdcSPBMInvestorPortfDefField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///组合保证金套餐代码
    TThostFtdcPortfolioDefIDType    PortfolioDefID;
};


◇ 1.7.新增接口：投资者SPBM组合保证金系
数查询
（1）查询请求
///投资者新型组合保证金系数查询
virtual int ReqQryInvestorPortfMarginRatio(CThostFtdcQryInv
estorPortfMarginRatioField *pQryInvestorPortfMarginRatio, int nRe
questID) = 0;
struct CThostFtdcQryInvestorPortfMarginRatioField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
};
（2）查询响应
///投资者新型组合保证金系数查询响应
virtual void OnRspQryInvestorPortfMarginRatio(CThostFtdcIn
vestorPortfMarginRatioField *pInvestorPortfMarginRatio, CThostFt
dcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
///投资者新型组合保证金系数
struct CThostFtdcInvestorPortfMarginRatioField
{


    ///投资者范围
    TThostFtdcInvestorRangeType InvestorRange;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///会员对投资者收取的保证金和交易所对投资者收取的保证金
的比例
    TThostFtdcRatioType MarginRatio;
};


◇ 1.8.新增接口：投资者产品SPBM明细查询
（1）查询请求
///投资者产品SPBM明细查询
virtual int ReqQryInvestorProdSPBMDetail(CThostFtdcQryInv
estorProdSPBMDetailField *pQryInvestorProdSPBMDetail, int nRe
questID) = 0;
struct CThostFtdcQryInvestorProdSPBMDetailField
{
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///品种代码
    TThostFtdcInstrumentIDType  ProdFamilyCode;
};
（2）查询响应
///投资者产品SPBM明细查询响应
virtual void OnRspQryInvestorProdSPBMDetail(CThostFtdcInv
estorProdSPBMDetailField *pInvestorProdSPBMDetail, CThostFtdc
RspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
///投资者产品SPBM明细
struct CThostFtdcInvestorProdSPBMDetailField
{


    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDTyp

---

## 6.7.0版本更新说明

6.7.0版本更新说明
版本号：v6.7.0
后台版本：V6.7.0
补充变更说明：（后台系统兼容线上旧版本API接入，请各终端
厂商根据自身情况进行开发）


◇ 1.API变动
此版本未新增接口，支持查询流量压缩lz4算法。
优化API的openssl编译和查询合约响应中ExchangeID乱码问题
回到顶部


◇ 2.兼容性说明：（请各终端厂商根据自身情
况进行开发）
1. 新版本API连接新front前置：无需配置，使用Lz4压缩算
法，查询流量压缩效果显著。
2. 旧版本API可以连接新front前置：使用原压缩算法。
3. 新版本API可以连接旧front前置：使用原压缩算法。
回到顶部
< 前页 回目录 后页 >




---

## 6.7.1版本更新说明

6.7.1版本更新说明
版本号：v6.7.1_20230613_traderapi
后台版本：V6.7.1
注意：此版本为支持上期所SPMM新组保业务而变更，新增4个交
易查询接口。为支持大商所GIS属性交易指令，报价接口
ReqQuoteInsert新增TimeCondition字段。GIS属性对应
THOST_FTDC_TC_GFS枚举值。该业务交易所尚未上线,待交易所上
线后可进行申报。
变更说明：（请各终端厂商根据自身情况进行开发）


◇ 1.API变动


◇ 1.1.新增接口：投资者商品组SPMM记录查
询
（1）查询请求
///投资者商品组SPMM记录查询
virtual int ReqQryInvestorCommoditySPMMMargin(CThostFtd
cQryInvestorCommoditySPMMMarginField *pQryInvestorCommod
itySPMMMargin, int nRequestID) = 0;
（2）查询响应
///投资者商品组SPMM记录查询响应
virtual void OnRspQryInvestorCommoditySPMMMargin(CTho
stFtdcInvestorCommoditySPMMMarginField *pInvestorCommodity
SPMMMargin, CThostFtdcRspInfoField *pRspInfo, int nRequestID,
bool bIsLast) {};


◇ 1.2.新增接口：投资者商品群SPMM记录查
询
（1）查询请求
///投资者商品群SPMM记录查询
virtual int ReqQryInvestorCommodityGroupSPMMMargin(CTh
ostFtdcQryInvestorCommodityGroupSPMMMarginField *pQryInve
storCommodityGroupSPMMMargin, int nRequestID) = 0;
（2）查询响应
///投资者商品群SPMM记录查询响应
virtual void OnRspQryInvestorCommodityGroupSPMMMargin
(CThostFtdcInvestorCommodityGroupSPMMMarginField *pInvesto
rCommodityGroupSPMMMargin, CThostFtdcRspInfoField *pRspIn
fo, int nRequestID, bool bIsLast) {};


◇ 1.3.新增接口：SPMM合约参数查询
（1）查询请求
///SPMM合约参数查询
virtual int ReqQrySPMMInstParam(CThostFtdcQrySPMMInstP
aramField *pQrySPMMInstParam, int nRequestID) = 0;
（2）查询响应
///SPMM合约参数查询响应
virtual void OnRspQrySPMMInstParam(CThostFtdcSPMMInst
ParamField *pSPMMInstParam, CThostFtdcRspInfoField *pRspInf
o, int nRequestID, bool bIsLast) {};


◇ 1.4.新增接口：SPMM产品参数查询
（1）查询请求
///SPMM产品参数查询
virtual int ReqQrySPMMProductParam(CThostFtdcQrySPMM
ProductParamField *pQrySPMMProductParam, int nRequestID) = 0;
（2）查询响应
///SPMM产品参数查询响应
virtual void OnRspQrySPMMProductParam(CThostFtdcSPMM
ProductParamField *pSPMMProductParam, CThostFtdcRspInfoFiel
d *pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.5.报价接口新增字段
为支持大商所GIS属性交易指令，报价接口ReqQuoteInsert新
增TimeCondition字段。GIS属性对应THOST_FTDC_TC_GFS枚举
值。该业务交易所尚未上线,待交易所上线后可进行申报。
回到顶部
< 前页 回目录 后页 >




---

## 6.7.2版本更新说明

6.7.2版本更新说明
版本号：v6.7.2_20230913_traderapi
后台版本：V6.7.2
注意：此版本为支持郑商所附加保证金算法调整、中金所新组保
RCAMS和大商所新组保Rule业务而变更，新增12个交易查询接口，请
各终端厂商根据自身情况进行开发。该版本修改了心跳打印逻辑，不
会自动打印了。
变更说明：（请各终端厂商根据自身情况进行开发）


◇ 1.API变动


◇ 1.1.新增接口：SPBM附加跨品种抵扣参数
查询。
（1）查询请求
///SPBM附加跨品种抵扣参数查询
virtual int ReqQrySPBMAddOnInterParameter(CThostFtdcQry
SPBMAddOnInterParameterField *pQrySPBMAddOnInterParamete
r, int nRequestID) = 0;
（2）查询响应
///SPBM附加跨品种抵扣参数查询响应
virtual void OnRspQrySPBMAddOnInterParameter(CThostFtdc
SPBMAddOnInterParameterField *pSPBMAddOnInterParameter, C
ThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.2.新增接口：RCAMS产品组合信息查
询。
（1）查询请求
///RCAMS产品组合信息查询
virtual int ReqQryRCAMSCombProductInfo(CThostFtdcQryR
CAMSCombProductInfoField *pQryRCAMSCombProductInfo, int
nRequestID) = 0;
（2）查询响应
///RCAMS产品组合信息查询响应
virtual void OnRspQryRCAMSCombProductInfo(CThostFtdcR
CAMSCombProductInfoField *pRCAMSCombProductInfo, CThost
FtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.3.新增接口：RCAMS同合约风险对冲参
数查询。
（1）查询请求
///RCAMS同合约风险对冲参数查询
virtual int ReqQryRCAMSInstrParameter(CThostFtdcQryRCA
MSInstrParameterField *pQryRCAMSInstrParameter, int nRequestI
D) = 0;
（2）查询响应
///RCAMS同合约风险对冲参数查询响应
virtual void OnRspQryRCAMSInstrParameter(CThostFtdcRCA
MSInstrParameterField *pRCAMSInstrParameter, CThostFtdcRspInf
oField *pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.4.新增接口：RCAMS品种内风险对冲参
数查询。
（1）查询请求
///RCAMS品种内风险对冲参数查询
virtual int ReqQryRCAMSIntraParameter(CThostFtdcQryRCA
MSIntraParameterField *pQryRCAMSIntraParameter, int nRequestI
D) = 0;
（2）查询响应
///RCAMS品种内风险对冲参数查询响应
virtual void OnRspQryRCAMSIntraParameter(CThostFtdcRCA
MSIntraParameterField *pRCAMSIntraParameter, CThostFtdcRspIn
foField *pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.5.新增接口：RCAMS跨品种风险折抵参
数查询。
（1）查询请求
///RCAMS跨品种风险折抵参数查询
virtual int ReqQryRCAMSInterParameter(CThostFtdcQryRCA
MSInterParameterField *pQryRCAMSInterParameter, int nRequestI
D) = 0;
（2）查询响应
///RCAMS跨品种风险折抵参数查询响应
virtual void OnRspQryRCAMSInterParameter(CThostFtdcRCA
MSInterParameterField *pRCAMSInterParameter, CThostFtdcRspIn
foField *pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.6.新增接口：RCAMS空头期权风险调整
参数查询。
（1）查询请求
///RCAMS空头期权风险调整参数查询
virtual int ReqQryRCAMSShortOptAdjustParam(CThostFtdcQr
yRCAMSShortOptAdjustParamField *pQryRCAMSShortOptAdjust
Param, int nRequestID) = 0;
（2）查询响应
///RCAMS空头期权风险调整参数查询响应
virtual void OnRspQryRCAMSShortOptAdjustParam(CThostFt
dcRCAMSShortOptAdjustParamField *pRCAMSShortOptAdjustPar
am, CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLa
st) {};


◇ 1.7.新增接口：RCAMS策略组合持仓查
询。
（1）查询请求
///RCAMS策略组合持仓查询
virtual int ReqQryRCAMSInvestorCombPosition(CThostFtdcQ
ryRCAMSInvestorCombPositionField *pQryRCAMSInvestorComb
Position, int nRequestID) = 0;
（2）查询响应
///RCAMS策略组合持仓查询响应
virtual void OnRspQryRCAMSInvestorCombPosition(CThostFt
dcRCAMSInvestorCombPositionField *pRCAMSInvestorCombPosi
tion, CThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLa
st) {};


◇ 1.8.新增接口：投资者品种RCAMS保证金
查询。
（1）查询请求
///投资者品种RCAMS保证金查询
virtual int ReqQryInvestorProdRCAMSMargin(CThostFtdcQryI
nvestorProdRCAMSMarginField *pQryInvestorProdRCAMSMargi
n, int nRequestID) = 0;
（2）查询响应
///投资者品种RCAMS保证金查询响应
virtual void OnRspQryInvestorProdRCAMSMargin(CThostFtdc
InvestorProdRCAMSMarginField *pInvestorProdRCAMSMargin, C
ThostFtdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.9.新增接口：RULE合约保证金参数查
询。
（1）查询请求
///RULE合约保证金参数查询
virtual int ReqQryRULEInstrParameter(CThostFtdcQryRULEIn
strParameterField *pQryRULEInstrParameter, int nRequestID) = 0;
（2）查询响应
///RULE合约保证金参数查询响应
virtual void OnRspQryRULEInstrParameter(CThostFtdcRULEI
nstrParameterField *pRULEInstrParameter, CThostFtdcRspInfoField
*pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.10.新增接口：RULE品种内对锁仓折扣
参数查询。
（1）查询请求
///RULE品种内对锁仓折扣参数查询
virtual int ReqQryRULEIntraParameter(CThostFtdcQryRULEI
ntraParameterField *pQryRULEIntraParameter, int nRequestID) = 0;
（2）查询响应
///RULE品种内对锁仓折扣参数查询响应
virtual void OnRspQryRULEIntraParameter(CThostFtdcRULEI
ntraParameterField *pRULEIntraParameter, CThostFtdcRspInfoField
*pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.11.新增接口：RULE跨品种抵扣参数查
询。
（1）查询请求
///RULE跨品种抵扣参数查询
virtual int ReqQryRULEInterParameter(CThostFtdcQryRULEI
nterParameterField *pQryRULEInterParameter, int nRequestID) = 0;
（2）查询响应
///RULE跨品种抵扣参数查询响应
virtual void OnRspQryRULEInterParameter(CThostFtdcRULEI
nterParameterField *pRULEInterParameter, CThostFtdcRspInfoField
*pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.12.新增接口：投资者产品RULE保证金
查询。
（1）查询请求
///投资者产品RULE保证金查询
virtual int ReqQryInvestorProdRULEMargin(CThostFtdcQryIn
vestorProdRULEMarginField *pQryInvestorProdRULEMargin, int n
RequestID) = 0;
（2）查询响应
///投资者产品RULE保证金查询响应
virtual void OnRspQryInvestorProdRULEMargin(CThostFtdcIn
vestorProdRULEMarginField *pInvestorProdRULEMargin, CThostF
tdcRspInfoField *pRspInfo, int nRequestID, bool bIsLast) {};
回到顶部
< 前页 回目录 后页 >




---

## 6.7.7版本更新说明

6.7.7版本更新说明
版本号：v6.7.7_20240607 15:51:36.7347
后台版本：V6.7.7
变更说明：（请各终端厂商根据自身情况进行开发）


◇ 1.API变动


◇ 1.1.新增接口：投资者新型组合保证金开关
查询
（1）查询请求
///投资者新型组合保证金开关查询
virtual int ReqQryInvestorPortfSetting(CThostFtdcQryInvestorP
ortfSettingField *pQryInvestorPortfSetting, int nRequestID) = 0;
（2）查询响应
///投资者新型组合保证金开关查询响应
virtual void OnRspQryInvestorPortfSetting(CThostFtdcInvestor
PortfSettingField *pInvestorPortfSetting, CThostFtdcRspInfoField *
pRspInfo, int nRequestID, bool bIsLast) {};


◇ 1.2.新增接口：获取已连接的前置的信息
（1）查询请求
///获取已连接得前置的信息
virtual void GetFrontInfo(CThostFtdcFrontInfoField* pFrontInf
o) =0;
返回前置地址、查询流控参数、FTD流控参数。连接成功
后，可获取正确的前置地址信息，登录成功后，可获取正确的
前置查询流控和FTD流控信息。


◇ 1.3.查询行情接口
ReqQryDepthMarketData 新增入参
ProductClass
///查询行情
struct CThostFtdcQryDepthMarketDataField
{
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///合约代码
    TThostFtdcInstrumentIDType  InstrumentID;
    ///产品类型
    TThostFtdcProductClassType  ProductClass;
};


◇ 1.4.TFtdcInstrumentStatusType合约交易状
态类型新增枚举值
说明：为与上期所API合约状态保持一致，合约交易状态类
型新增枚举值
///交易业务处理
#define THOST_FTDC_IS_TransactionProcessing '7'


◇ 1.5.报单、报价录入请求响应ErrorMsg新
增报单欠缺资金。
OnRspOrderInsert报单录入请求响应和OnRspQuoteInsert报价
录入请求响应ErrorMsg新增报单欠缺资金。
当报单、报价资金不足时，返回的ErrorMsg中包含了欠缺资
金，即：资金不足，约缺少资金[******]


◇ 1.6.如下四个接口新增入参
ReqOrderInsert（报单录入请求）、ReqOrderAction（报单操
作请求）、ReqQuoteInsert（报价录入请求）、ReqQuoteAction
（报价操作请求）
新增入参如下：
///报单回显字段，OrderMemo字段可供终端厂商标记订单使用，
CTP不做处理，即终端填写什么CTP就返回什么
TThostFtdcOrderMemoType OrderMemo;
///session上请求计数 api自动维护
TThostFtdcSequenceNo12Type  SessionReqSeq;
回到顶部
< 前页 回目录 后页 >





