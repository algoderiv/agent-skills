# Api Reference - CTP 6.7.8 API

本节包含 107 个主题。


## ReqOptionSelfCloseInsert

ReqOptionSelfCloseInsert
期权自对冲录入请求、详见期货期权的行权、自对冲
错误响应: OnErrRtnOptionSelfCloseInsert，
OnRspOptionSelfCloseInsert
正确响应: OnRtnOptionSelfClose


◇ 1.函数原型
virtual int
ReqOptionSelfCloseInsert(CThostFtdcInputOptionSelfCloseField
*pInputOptionSelfClose, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputOptionSelfClose：输入的期权自对冲
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪
公司
代码
必填
TThostFtdcInvestorIDType
InvestorID
投资
者代
码
必填
TThostFtdcInstrumentIDType
InstrumentID
合约
代码必填
TThostFtdcOrderRefType
OptionSelfCloseRef
期权
自对
冲引
用
无
TThostFtdcUserIDType
UserID
用户
代码无
TThostFtdcBusinessUnitType
BusinessUnit
业务
单元无
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
必填
TThostFtdcInvestUnitIDType
InvestUnitID
投资
单元
代码
无


TThostFtdcAccountIDType
AccountID
投资
者帐
号
无
TThostFtdcCurrencyIDType
CurrencyID
币种
代码无
TThostFtdcClientIDType
ClientID
客户
代码无
TThostFtdcIPAddressType
IPAddress
IP地
址
无
TThostFtdcMacAddressType
MacAddress
Mac
地址无
TThostFtdcVolumeType
Volume
数量必填
TThostFtdcRequestIDType
RequestID
请求
编号无
TThostFtdcHedgeFlagType
HedgeFlag
投机
套保
标志
必填
TThostFtdcOptSelfCloseFlagType OptSelfCloseFlag
期权
行权
的头
寸是
否自
对冲
1自对
冲期权
仓位、
2保留
期权仓
位、3
自对冲
卖方履
约后的
期货仓
位


TThostFtdcOldInstrumentIDType
reserve1
保留
的无
效字
段
否
TThostFtdcOldIPAddressType
reserve2
保留
的无
效字
段
否
OptionSelfCloseRef：需要纯数字递增，不填则ctp自动填写
IPAddress：手工填写本机IP地址，不自动获取。填写规则如
下：ipv4原样填写，ipv6要转成非零压缩地址，即原始地址，同时
要去掉冒号，eg：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
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
CThostFtdcInputOptionSelfCloseField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.UserID, "1000001");
a.Volume = 1;
a.HedgeFlag = THOST_FTDC_HF_Speculation;
a.OptSelfCloseFlag = THOST_FTDC_OSCF_CloseSelfOptionPosition;
strcpy_s(a.ExchangeID, "SHFE");
strcpy_s(a.AccountID, "1000001");
strcpy_s(a.CurrencyID, "CNY");
m_pUserApi->ReqOptionSelfCloseInsert(&a, nRequestID++);
回到顶部


◇ 5.FAQ
为什么报入的OptionSelfCloseRef和返回的值不同？
根据交易所规则，自对冲报单由首次报入的报单属性
确定，之后不论是否对该报单进行任何操作（包括撤单和
重新报单），报单的OptionSelfCloseRef都不变。
回到顶部
< 前页 回目录 后页 >


ReqOrderAction
报单操作请求
错误响应: OnRspOrderAction，OnErrRtnOrderAction
正确响应：OnRtnOrder


◇ 1.函数原型
virtual int ReqOrderAction(CThostFtdcInputOrderActionField
*pInputOrderAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputOrderAction：输入报单操作
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司
代码
必填
TThostFtdcInvestorIDType
InvestorID
投资者代
码
必填
TThostFtdcOrderRefType
OrderRef
报单引用
必填
*1
TThostFtdcExchangeIDType
ExchangeID
交易所代
码
必填
TThostFtdcOrderSysIDType
OrderSysID
报单编号
必填
*2
TThostFtdcUserIDType
UserID
用户代码
必填
TThostFtdcInstrumentIDType
InstrumentID
合约代码
必填
TThostFtdcInvestUnitIDType
InvestUnitID
投资单元
代码
无
TThostFtdcIPAddressType
IPAddress
IP地址
无
TThostFtdcMacAddressType
MacAddress
Mac地址
无
TThostFtdcOrderActionRefType
OrderActionRef 报单操作
引用
必
填，
递增
TThostFtdcRequestIDType
RequestID
请求编号
无
TThostFtdcFrontIDType
FrontID
前置编号
必填
*1


TThostFtdcSessionIDType
SessionID
会话编号
必填
*1
TThostFtdcVolumeType
VolumeChange
数量变化
无
TThostFtdcActionFlagType
ActionFlag
操作标志
必填
TThostFtdcPriceType
LimitPrice
价格
无
TThostFtdcOldInstrumentIDType reserve1
保留的无
效字段
无
TThostFtdcOldIPAddressType
reserve2
保留的无
效字段
无
TThostFtdcOldIPAddressType
reserve2
保留的无
效字段
无
TThostFtdcOrderMemoType
OrderMemo
报单回显
字段
无
TThostFtdcSequenceNo12Type
SessionReqSeq
session上
请求计数
api自动维
护
无
必填*1、必填*2：两组选一组必填，能对应要撤的报单。
OrderRef：对应要撤销的那笔报单的报单引用
FrontID: 对应要撤销的那笔报单的前置编号
SessionID: 对应要撤销的那笔报单的会话编号
ExchangeID: 对应要撤销的那笔报单的交易所ID
OrderSysID: 对应要撤销的那笔报单的报单编号
ActionFlag：只支持删除，不支持修改、激活、挂起。对于由其
他柜台做的激活或挂起的报单，ctp系统能正常接收。


IPAddress：手工填写本机IP地址，不自动获取。填写规则如
下：ipv4原样填写，ipv6要转成非零压缩地址，即原始地址，同时
要去掉冒号，eg：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
OrderMemo:报单回显字段，OrderMemo字段可供终端厂商标记
订单使用，CTP不做处理，即终端填写什么CTP就返回什么
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
//第一种方法，使用OrderSysID撤单（推荐使用）
CThostFtdcInputOrderActionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.UserID, "1000001");
strcpy_s(a.OrderSysID, "         131");  //对应要撤报单的OrderSysID
strcpy_s(a.ExchangeID, "SHFE");
strcpy_s(a.InstrumentID, "rb1809");
ActionFlag = THOST_FTDC_AF_Delete;
m_pUserApi->ReqOrderAction(&a, nRequestID++);
//第二种方法，使用FrontID+SessionID+OrderRef撤单
CThostFtdcInputOrderActionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.UserID, "1000001");
a.FrontID = 1;   //对应要撤报单的FrontID
a.SessionID = -788541;   //对应要撤报单的sessionid
strcpy_s(a.OrderRef, "        3");   //对应要撤报单的OrderRef
strcpy_s(a.ExchangeID, "SHFE");
strcpy_s(a.InstrumentID, "rb1809");
ActionFlag = THOST_FTDC_AF_Delete;
m_pUserApi->ReqOrderAction(&a, nRequestID++);
回到顶部


◇ 5.FAQ
ctp支持撤销未知单吗？
目前665版本以上柜台不允许撤郑商所的未知单，其
他交易所都是可以撤未知单的。
回到顶部
< 前页 回目录 后页 >


ReqOrderInsert
报单录入请求，录入错误时对应响应OnRspOrderInsert、
OnErrRtnOrderInsert，正确时对应回报OnRtnOrder、OnRtnTrade。
可以录入限价单、市价单、条件单等交易所支持的指令，撤单时
使用ReqOrderAction。
不支持预埋单录入，预埋单请使用ReqParkedOrderInsert。


◇ 1.函数原型
virtual int ReqOrderInsert(CThostFtdcInputOrderField
*pInputOrder, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputOrder：输入报单
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪
公司
代码
必
填
TThostFtdcInvestorIDType
InvestorID
投资
者代
码
必
填
TThostFtdcInstrumentIDType
InstrumentID
合约
代码
必
填
TThostFtdcOrderRefType
OrderRef
报单
引用
可
自
定
义
或
不
填
TThostFtdcUserIDType
UserID
用户
代码
无
TThostFtdcCombOffsetFlagType
CombOffsetFlag
开平
标志
必
填
TThostFtdcCombHedgeFlagType
CombHedgeFlag
投机
套保
标志
必
填


TThostFtdcDateType
GTDDate
GTD
日期
无
TThostFtdcBusinessUnitType
BusinessUnit
业务
单元
无
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
必
填
TThostFtdcInvestUnitIDType
InvestUnitID
投资
单元
代码
无
TThostFtdcAccountIDType
AccountID
投资
者帐
号
无
TThostFtdcCurrencyIDType
CurrencyID
币种
代码
无
TThostFtdcClientIDType
ClientID
客户
代码
无
TThostFtdcIPAddressType
IPAddress
IP地址无
TThostFtdcMacAddressType
MacAddress
Mac地
址
无
TThostFtdcVolumeType
VolumeTotalOriginal 数量
必
填
TThostFtdcVolumeType
MinVolume
最小
成交
量
无
TThostFtdcBoolType
IsAutoSuspend
自动
挂起
标志
必
填
0


TThostFtdcRequestIDType
RequestID
请求
编号
无
TThostFtdcBoolType
UserForceClose
用户
强平
标志
无
TThostFtdcBoolType
IsSwapOrder
互换
单标
志
必
填
TThostFtdcOrderPriceTypeType
OrderPriceType
报单
价格
条件
必
填
TThostFtdcDirectionType
Direction
买卖
方向
必
填
TThostFtdcTimeConditionType
TimeCondition
有效
期类
型
必
填
TThostFtdcVolumeConditionType
VolumeCondition
成交
量类
型
必
填
TThostFtdcContingentConditionType ContingentCondition 触发
条件
必
填
TThostFtdcForceCloseReasonType
ForceCloseReason
强平
原因
必
填
TThostFtdcPriceType
LimitPrice
价格
必
填
TThostFtdcPriceType
StopPrice
止损
价
必
填


TThostFtdcOldInstrumentIDType
reserve1
保留
的无
效字
段
无
TThostFtdcOldIPAddressType
reserve2
保留
的无
效字
段
无
TThostFtdcOrderMemoType
OrderMemo
报单
回显
字段
无
TThostFtdcSequenceNo12Type
SessionReqSeq
session
上请
求计
数 api
自动
维护
无
OrderPriceType：确定输入的报单类型，比如限价单则填写
THOST_FTDC_OPT_LimitPrice、市价单则填写
THOST_FTDC_OPT_AnyPrice。
Direction：确定买卖方向
CombOffsetFlag：确定开平标志。上期所和能源交易所支持平
昨和平今指令，分别对应昨仓和今仓；如果下平仓指令，则等同于
平昨指令。大商所、广期所、郑商所、中金所按交易所规则只能报
入平仓指令，如果报入平昨平今指令ctp会转成平仓指令报出。
CombHedgeFlag：确定投机套保标志，注：郑商所平仓只能报
入“投机”属性 例:投机 THOST_FTDC_BHF_Speculation


TimeCondition：确定报单有效期类型，目前只支持GFD和
IOC，其他都不支持 例:立即完成，否则撤销
THOST_FTDC_TC_IOC
VolumeCondition：确定成交量类型
ContingentCondition：确定触发条件
StopPrice：止损价，用于条件单的触发价格
ForceCloseReason：普通用户下单填写
THOST_FTDC_FCC_NotForceClose 非强平
IsSwapOrder：互换单填1
CurrencyID：不填写默认为CNY
OrderRef：OrderRef是本地会话全局唯一编号，必须保持递
增；可由用户维护，也可由系统自动填写。一定为数字。
IPAddress：手工填写本机IP地址，不自动获取。填写规则如
下：ipv4原样填写，ipv6要转成非零压缩地址，即原始地址，同时
要去掉冒号，eg：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
MacAddress：手工填写本机Mac地址，不自动获取。
BusinessUnit：保留字段。
LimitPrice：郑商所报单的价格会被交易所按照最小变动价位截
取。(eg:报入1500.9，最小变动价位是1，会被交易所截取为1500。)
其他交易所则直接返回错误。
OrderMemo:报单回显字段，OrderMemo字段可供终端厂商标记
订单使用，CTP不做处理，即终端填写什么CTP就返回什么
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
//限价单:
CThostFtdcInputOrderField ord = { 0 };
strcpy_s(ord.BrokerID, “0000”);
strcpy_s(ord.InvestorID, “00001”); 
strcpy_s

---

## ReqQryBrokerTradingAlgos

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




---

## ReqQryBrokerTradingParams

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




---

## ReqQryCombInstrumentGuard

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




---

## ReqQryContractBank

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

## ReqQryEWarrantOffset

ReqQryEWarrantOffset
请求查询仓单折抵信息
响应: OnRspQryEWarrantOffset


◇ 1.函数原型
virtual int
ReqQryEWarrantOffset(CThostFtdcQryEWarrantOffsetField
*pQryEWarrantOffset, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryEWarrantOffset：查询仓单折抵信息
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
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
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




---

## ReqQryExchange

ReqQryExchange
请求查询交易所
响应: OnRspQryExchange


◇ 1.函数原型
virtual int ReqQryExchange(CThostFtdcQryExchangeField
*pQryExchange, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExchange：查询交易所
字段类型
字段名称
含义
是否可作为过
滤条件
TThostFtdcExchangeIDType ExchangeID 交易所
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
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryExchangeMarginRate

ReqQryExchangeMarginRate
请求查询交易所保证金率
响应: OnRspQryExchangeMarginRate


◇ 1.函数原型
virtual int
ReqQryExchangeMarginRate(CThostFtdcQryExchangeMarginRateField
*pQryExchangeMarginRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExchangeMarginRate：查询交易所保证金率
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
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcHedgeFlagType
HedgeFlag
投机套
保标志
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

## ReqQryExchangeMarginRateAdjust

ReqQryExchangeMarginRateAdjust
请求查询交易所调整保证金率
响应: OnRspQryExchangeMarginRateAdjust


◇ 1.函数原型
virtual int
ReqQryExchangeMarginRateAdjust(CThostFtdcQryExchangeMarginRat
eAdjustField *pQryExchangeMarginRateAdjust, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExchangeMarginRateAdjust：查询交易所调整保证金率
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
TThostFtdcInstrumentIDType
InstrumentID 合约代
码
是
TThostFtdcHedgeFlagType
HedgeFlag
投机套
保标志
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




---

## ReqQryExchangeRate

ReqQryExchangeRate
请求查询汇率
响应：OnRspQryExchangeRate


◇ 1.函数原型
virtual int
ReqQryExchangeRate(CThostFtdcQryExchangeRateField
*pQryExchangeRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExchangeRate：查询汇率
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
TThostFtdcCurrencyIDType FromCurrencyID 源币种
是
TThostFtdcCurrencyIDType ToCurrencyID
目标币
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


ReqQryExecOrder
请求查询执行宣告
响应: OnRspQryExecOrder


◇ 1.函数原型
virtual int ReqQryExecOrder(CThostFtdcQryExecOrderField
*pQryExecOrder, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryExecOrder：执行宣告查询
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
InstrumentID
合约代
码
是
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
是
TThostFtdcExecOrderSysIDType ExecOrderSysID 报单编
号
是
TThostFtdcTimeType
InsertTimeStart
开始时
间
是
TThostFtdcTimeType
InsertTimeEnd
结束时
间
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
CThostFtdcQryExecOrderField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryExecOrder(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqQryForQuote
请求查询询价
响应：OnRspQryForQuote


◇ 1.函数原型
virtual int ReqQryForQuote(CThostFtdcQryForQuoteField
*pQryForQuote, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryForQuote：询价查询
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
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqQryInstrument

ReqQryInstrument
请求查询合约，填空可以查询到所有合约。
响应:OnRspQryInstrument


◇ 1.函数原型
virtual int ReqQryInstrument(CThostFtdcQryInstrumentField
*pQryInstrument, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInstrument：查询合约
字段类型
字段名称
含义
是否可
作为过
滤条件
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
TThostFtdcExchangeInstIDType
ExchangeInstID
合约在
交易所
的代码
是
TThostFtdcInstrumentIDType
ProductID
产品代
码
是
TThostFtdcOldInstrumentIDType
reserve1
保留的
无效字
段
否
TThostFtdcOldExchangeInstIDType reserve2
保留的
无效字
段
否
TThostFtdcOldInstrumentIDType
reserve3
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
CThostFtdcQryInstrumentField a = { 0 };
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqQryInstrument(&a, nRequestID++);
回到顶部


◇ 5.FAQ
为何查询不到郑商所跨式和宽跨式套利合约？
郑商所没有推跨式和宽跨式套利合约，所以查询不到
此类合约。
为何查询中金所的EFP（期转现）合约，
InstrumentName为空？
交易所就没有推送instrumentname。
回到顶部
< 前页 回目录 后页 >




---

## ReqQryInstrumentCommissionRate

ReqQryInstrumentCommissionRate
请求查询合约手续费率，对应响应
OnRspQryInstrumentCommissionRate。如果InstrumentID填空，则返回
持仓对应的合约手续费率。
目前无法通过一次查询得到所有合约手续费率，如果要查询所
有，则需要通过多次查询得到。


◇ 1.函数原型
virtual int
ReqQryInstrumentCommissionRate(CThostFtdcQryInstrumentCommissi
onRateField *pQryInstrumentCommissionRate, int nRequestID) = 0;
回到顶部


◇ 2.参数
pQryInstrumentCommissionRate：查询手续费率
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
