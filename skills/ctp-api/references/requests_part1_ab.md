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




---

## ReqOrderAction

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




---

## ReqOrderInsert

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
strcpy_s(ord.ExchangeID, “SHFE”);
strcpy_s(ord.InstrumentID, “ag1801”);
strcpy_s(ord.UserID, “00001”);
ord.OrderPriceType = THOST_FTDC_OPT_LimitPrice;//限价
ord.Direction = THOST_FTDC_D_Buy;//买
ord.CombOffsetFlag[0] = THOST_FTDC_OF_Open;//开
ord.CombHedgeFlag[0] = THOST_FTDC_HF_Speculation;//投机
ord.LimitPrice = 100;
ord.VolumeTotalOriginal = 1;
ord.TimeCondition = THOST_FTDC_TC_GFD;///当日有效
ord.VolumeCondition = THOST_FTDC_VC_AV;///任意数量
ord.MinVolume = 1;
ord.ContingentCondition = THOST_FTDC_CC_Immediately;
ord.StopPrice = 0;
ord.ForceCloseReason = THOST_FTDC_FCC_NotForceClose;
ord.IsAutoSuspend = 0;
m_pUserApi->ReqOrderInsert(&ord, nRequestID++);
回到顶部


◇ 5.FAQ
郑商所白糖期权的跨式和宽跨式套利指令，在ctp里如何
实现？
和普通的限价单类似，不同的地方是在填写Instrument
ID的时候
郑商所跨式组合（STD）: STD SR703C5300&SR703P
5300
郑商所宽跨式组合（STG): STG SR703C5300&SR703
P5200
支持FAK和FOK
“已撤单报单被拒绝CZCE:出错:不支持无限制期权组合
订单”是什么原因？
郑商所的组合合约下单，TimeCondition应该用THOS
T_FTDC_TC_IOC（1）。
如果下单后遇到报错“ctp：每秒报单数超过许可数”，是
什么原因？
这是由于报单频率太快被前置流控所致。投资者可询
问期货公司具体的流控设置；期货公司可以自行查看FLE
X"程序化交易频繁报撤单管理"中的设置，以及Front的流
控配置。注意：Front的流控会放到下一秒，不会报错；FL
EX"程序化交易频繁报撤单管理"中的流控会报错。
“CTP:报单错误：不允许重复报单”，是什么原因？
这是表示OrderRef没有递增。
“CTP:无此权限”，是什么原因？


这可能InvestorID填写错误，或者该InvestorID不属于
UserID的组织架构下。
“CTP:交易所未处理请求超过许可数”，是什么原因？
表示报单频率过快，超过了交易所的流控。
“CTP:交易所每秒发送请求数超过许可数”，是什么原
因？
表示报单频率过快，超过了交易所的流控。
投机套保标志能够用来判断报单所使用的交易编码类
型？
ctp支持，但是有些交易所不支持多交易编码。除中金
所有投机、套保、套利三个交易编码外，其他交易所只有
投机交易编码。并且交易编码还有做市商类型，但是投机
套保中没有做市商这种类型。
req请求返回值为0，可能出现回报收不到的情况吗？
可能的，Req返回为0，代表socket.write成功，并不表
示柜台那边已经收到了请求。不保证一定有OnRsp和OnErr
Rtn的情况。
回到顶部
< 前页 回目录 后页 >




---

## ReqParkedOrderAction

ReqParkedOrderAction
预埋撤单录入请求
响应: OnRspQryParkedOrderAction


◇ 1.函数原型
virtual int
ReqParkedOrderAction(CThostFtdcParkedOrderActionField
*pParkedOrderAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pParkedOrderAction：输入预埋单操作
字段类型
字段名称
含
义
值
TThostFtdcBrokerIDType
BrokerID
经
纪
公
司
代
码
必
填
TThostFtdcInvestorIDType
InvestorID
投
资
者
代
码
必
填
TThostFtdcOrderRefType
OrderRef
报
单
引
用
必
填
*1
TThostFtdcExchangeIDType
ExchangeID
交
易
所
代
码
无
TThostFtdcOrderSysIDType
OrderSysID
报
单
必
填


编
号
*2
TThostFtdcUserIDType
UserID
用
户
代
码
无
TThostFtdcInstrumentIDType
InstrumentID
合
约
代
码
必
填
TThostFtdcParkedOrderActionIDType ParkedOrderActionID
预
埋
撤
单
单
编
号
无
TThostFtdcErrorMsgType
ErrorMsg
错
误
信
息
无
TThostFtdcInvestUnitIDType
InvestUnitID
投
资
单
元
代
码
无


TThostFtdcIPAddressType
IPAddress
IP
地
址
无
TThostFtdcMacAddressType
MacAddress
Mac
地
址
无
TThostFtdcOrderActionRefType
OrderActionRef
报
单
操
作
引
用
无
TThostFtdcRequestIDType
RequestID
请
求
编
号
无
TThostFtdcFrontIDType
FrontID
前
置
编
号
必
填
*1
TThostFtdcSessionIDType
SessionID
会
话
编
号
必
填
*1
TThostFtdcVolumeType
VolumeChange
数
量
变
化
无


TThostFtdcErrorIDType
ErrorID
错
误
代
码
无
TThostFtdcActionFlagType
ActionFlag
操
作
标
志
必
填
TThostFtdcUserTypeType
UserType
用
户
类
型
无
TThostFtdcParkedOrderStatusType
Status
预
埋
单
状
态
无
TThostFtdcPriceType
LimitPrice
价
格
无
TThostFtdcOldInstrumentIDType
reserve1
保
留
的
无
效
字
段
否
TThostFtdcOldIPAddressType
reserve2
保
留
否


的
无
效
字
段
必填*1、必填*2：两组选一组必填，能对应要撤的报单。
OrderRef：对应要撤销的那笔报单的报单引用
FrontID: 对应要撤销的那笔报单的前置编号
SessionID: 对应要撤销的那笔报单的会话编号
ExchangeID: 对应要撤销的那笔报单的交易所ID
OrderSysID: 对应要撤销的那笔报单的报单编号
ActionFlag：只支持删除，不支持修改
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
CThostFtdcParkedOrderActionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.ExchangeID, "SHFE");
strcpy_s(a.OrderSysID, "    10061782"); 
strcpy_s(a.UserID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
ActionFlag = THOST_FTDC_AF_Delete;
m_pUserApi->ReqParkedOrderAction(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqParkedOrderInsert

ReqParkedOrderInsert
预埋单录入请求
注意：由于交易所不推送组合合约的开盘信号，而服务器预埋单
依赖交易所合约开盘信号触发，所以服务器预埋单暂不支持下组合合
约。
响应: OnRspParkedOrderInsert


◇ 1.函数原型
virtual int ReqParkedOrderInsert(CThostFtdcParkedOrderField
*pParkedOrder, int nRequestID) = 0;
回到顶部


◇ 2.参数
pParkedOrder：预埋单
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
无
TThostFtdcParkedOrderIDType
ParkedOrderID
预埋
报单
编号
无
TThostFtdcErrorMsgType
ErrorMsg
错误
信息
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
TThostFtdcInvestUnitIDType
InvestUnitID
投资
单元
代码
无
TThostFtdcIPAddressType
IPAddress
IP地
址
无
TThostFtdcMacAddressType
MacAddress
Mac
地址
无
TThostFtdcVolumeType
VolumeTotalOriginal 数量
必
填
TThostFtdcVolumeType
MinVolume
最小
成交
无


量
TThostFtdcBoolType
IsAutoSuspend
自动
挂起
标志
无
TThostFtdcRequestIDType
RequestID
请求
编号
无
TThostFtdcBoolType
UserForceClose
用户
强评
标志
无
TThostFtdcErrorIDType
ErrorID
错误
代码
无
TThostFtdcBoolType
IsSwapOrder
互换
单标
志
无
TThostFtdcOrderPriceTypeType
OrderPriceType
报单
价格
条件
限
价
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
当
日
有
效
TThostFtdcVolumeConditionType
VolumeCondition
成交
量类
型
任
何
数
量


TThostFtdcContingentConditionType ContingentCondition 触发
条件
立
即
TThostFtdcForceCloseReasonType
ForceCloseReason
强平
原因
非
强
平
TThostFtdcUserTypeType
UserType
用户
类型
无
TThostFtdcParkedOrderStatusType
Status
预埋
单状
态
无
TThostFtdcPriceType
LimitPrice
价格
必
填
TThostFtdcPriceType
StopPrice
止损
价
无
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
OrderRef：OrderRef是本地会话全局唯一编号，必须保持递
增；可由用户维护，也可由系统自动填写。一定为数字。
OrderPriceType：确定输入的报单类型，比如限价单则填写
THOST_FTDC_OPT_LimitPrice、市价单则填写
THOST_FTDC_OPT_AnyPrice。


Direction：确定买卖方向
CombOffsetFlag：确定开平标志。注:上期所、能源交易所有平
今指令，下平仓指令和平昨指令相同
CombHedgeFlag：确定投机套保标志 例:投机
THOST_FTDC_BHF_Speculation
TimeCondition：确定报单有效期类型 例:立即完成，否则撤销
THOST_FTDC_TC_IOC
VolumeCondition：确定成交量类型
ContingentCondition：确定触发条件
StopPrice：止损价，用于条件单的触发价格
ForceCloseReason：一般填写
THOST_FTDC_FCC_NotForceClose 非强平
IsSwapOrder：用户互换单的标志，只有互换单需要填写，是互
换单的则赋值为1
CurrencyID：不填写默认为CNY
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
CThostFtdcParkedOrderField a = { 0 };
strcpy(a.BrokerID, "9999");
strcpy(a.InvestorID, "1000001");
strcpy(a.InstrumentID, "rb1809");
strcpy(a.UserID, "1000001");
strcpy(a.ExchangeID, "SHFE");
a.OrderPriceType = THOST_FTDC_OPT_LimitPrice;
a.Direction = THOST_FTDC_D_Buy; 
strcpy(a.CombOffsetFlag, "0"); 
strcpy(a.CombHedgeFlag, "1");
a.LimitPrice = 400;
a.VolumeTotalOriginal = 1;
a.TimeCondition = THOST_FTDC_TC_GFD;
strcpy(a.GTDDate, "");
a.VolumeCondition = THOST_FTDC_VC_CV;
a.MinVolume = 0;
a.ContingentCondition = THOST_FTDC_CC_Immediately;
a.StopPrice = 0;
a.ForceCloseReason = THOST_FTDC_FCC_NotForceClose;
a.IsAutoSuspend = 0;
m_pUserApi->ReqParkedOrderInsert(&a, nRequestID++);
回到顶部


◇ 5.FAQ
“CTP:预埋单:不支持的触发类型。”，是什么原因？
后台版本自6.7.2开始不再支持报入预埋条件单、预埋
预埋单。
回到顶部
< 前页 回目录 后页 >


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




---

## ReqQryCombAction

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
