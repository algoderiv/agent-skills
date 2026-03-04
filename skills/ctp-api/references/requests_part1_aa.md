# Requests - CTP 6.7.8 API

本节包含 26 个主题。

## 目录

      - [ReqUserLogin](#ReqUserLogin) (第 266 页)
      - [ReqBatchOrderAction](#ReqBatchOrderAction) (第 1538 页)
      - [ReqCombActionInsert](#ReqCombActionInsert) (第 1545 页)
      - [ReqExecOrderAction](#ReqExecOrderAction) (第 1552 页)
      - [ReqExecOrderInsert](#ReqExecOrderInsert) (第 1560 页)
      - [ReqOptionSelfCloseAction](#ReqOptionSelfCloseAction) (第 1608 页)
      - [ReqOrderAction](#ReqOrderAction) (第 1625 页)
      - [ReqOrderInsert](#ReqOrderInsert) (第 1633 页)
      - [ReqParkedOrderAction](#ReqParkedOrderAction) (第 1644 页)
      - [ReqParkedOrderInsert](#ReqParkedOrderInsert) (第 1654 页)
      - [ReqQryCombAction](#ReqQryCombAction) (第 1688 页)
      - [ReqQryExecOrder](#ReqQryExecOrder) (第 1742 页)
      - [ReqQryInstrumentOrderCommRate](#ReqQryInstrumentOrderCommRate) (第 1773 页)
      - [ReqQryOrder](#ReqQryOrder) (第 1855 页)
      - [ReqQryParkedOrder](#ReqQryParkedOrder) (第 1862 页)
      - [ReqQryParkedOrderAction](#ReqQryParkedOrderAction) (第 1868 页)
      - [ReqQueryBankAccountMoneyByFuture](#ReqQueryBankAccountMoneyByFuture) (第 1972 页)
      - [ReqQueryCFMMCTradingAccountToken](#ReqQueryCFMMCTradingAccountToken) (第 1980 页)
      - [ReqQryMaxOrderVolume](#ReqQryMaxOrderVolume) (第 1986 页)
      - [ReqQuoteAction](#ReqQuoteAction) (第 1993 页)
      - [ReqRemoveParkedOrder](#ReqRemoveParkedOrder) (第 2010 页)
      - [ReqRemoveParkedOrderAction](#ReqRemoveParkedOrderAction) (第 2016 页)
      - [ReqUserLogin](#ReqUserLogin) (第 2041 页)
      - [ReqUserLoginWithCaptcha](#ReqUserLoginWithCaptcha) (第 2049 页)
      - [ReqUserLoginWithOTP](#ReqUserLoginWithOTP) (第 2056 页)
      - [ReqUserLoginWithText](#ReqUserLoginWithText) (第 2063 页)

---

## ReqUserLogin

ReqUserLogin
用户登录请求，对应响应OnRspUserLogin。目前行情登陆不校验
账号密码。


◇ 1.特别说明
自CTP交易系统升级6.6.2版本后，后台支持对用户登录行情前
置进行身份校验。
若启用该功能后，登录行情前置时要求当前交易日该IP已成功
登录过交易系统，且发起登录行情的请求中必须正确填写BrokerID
和UserID，与登录交易的信息保持一致。
不填、填错或该IP未成功登录过交易系统，则校验不通过，会
提示“CTP:不合法登录”；
若不启用，则无需验证，可直接发起登录。
回到顶部


◇ 2.函数原型
virtual int ReqUserLogin(CThostFtdcReqUserLoginField
*pReqUserLoginField, int nRequestID) = 0;
回到顶部


◇ 3.参数
pReqUserLoginField：用户登录请求
struct CThostFtdcReqUserLoginField
{
    ///交易日
    TThostFtdcDateType TradingDay;
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///用户代码
    TThostFtdcUserIDType UserID;
    ///密码
    TThostFtdcPasswordType Password;
    ///用户端产品信息
    TThostFtdcProductInfoType UserProductInfo;
    ///接口端产品信息
    TThostFtdcProductInfoType InterfaceProductInfo;
    ///协议信息
    TThostFtdcProtocolInfoType ProtocolInfo;
    ///Mac地址
    TThostFtdcMacAddressType MacAddress;
    ///动态密码
    TThostFtdcPasswordType OneTimePassword;
    ///终端IP地址
    TThostFtdcIPAddressType ClientIPAddress;
    ///登录备注
    TThostFtdcLoginRemarkType LoginRemark;
};
BrokerID:开启行情身份校验功能后，该字段必需正确填写


UserID：操作员代码，后续请求中的investorid需要属于该操作
员的组织架构下；开启行情身份校验功能后，该字段必需正确填写
UserProductInfo：客户端的产品信息，如软件开发商、版本号
等，
CTP后台用户事件中的用户登录事件所显示的用户端产品信
息取自ReqAuthentication接口里的UserProductInfo，而非
ReqUserLogin里的。
LoginRemark：可以写登录备注，能够被交易系统的日志查询
到。
IPAddress：系统自动获取，填写无效。
nRequestID：请求ID，对应响应里的nRequestID，无递增规
则，由用户自行维护。
回到顶部


◇ 4.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 5.调用示例
CThostFtdcReqUserLoginField reqUserLogin = {0};
m_pUserMdApi->ReqUserLogin(&reqUserLogin, nRequestID++);
回到顶部


◇ 6.FAQ
无
回到顶部
< 前页 回目录 后页 >


ReqUserLogout
登出请求，对应响应OnRspUserLogout。暂不支持。


◇ 1.函数原型
virtual int ReqUserLogout(CThostFtdcUserLogoutField
*pUserLogout, int nRequestID) = 0;
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
CThostFtdcUserLogoutField a = { 0 };
m_pUserApi->ReqUserLogout(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


SubscribeForQuoteRsp
订阅询价，对应响应OnRspSubForQuoteRsp；订阅成功后推送
OnRtnForQuoteRsp。
询价相关业务请参考：做市商询价和报价


◇ 1.函数原型
virtual int SubscribeForQuoteRsp(char *ppInstrumentID[], int
nCount) = 0;
回到顶部


◇ 2.参数
ppInstrumentID：合约ID
nCount：要订阅/退订行情的合约个数
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
char **ppInstrumentID = new char*[50]; 
ppInstrumentID[0] = “sc1801”;
int result = m_pUserMdApi->SubscribeForQuoteRsp(ppInstrumentID, 
1);
回到顶部


◇ 5.FAQ
回到顶部
< 前页 回目录 后页 >


SubscribeMarketData
订阅行情，对应响应OnRspSubMarketData；订阅成功后，通过
OnRtnDepthMarketData推送行情信息。
订阅全市场合约需要把全市场合约代码都赋值给
ppInstrumentID，填空不能订阅全市场合约。
目前OnRtnDepthMarketData响应的数量会比请求合约的数量
少，且OnRtnDepthMarketData响应中会有多次bIsLast=true，此为已
知问题，但不影响实际订阅的合约数量。


◇ 1.函数原型
virtual int SubscribeMarketData(char *ppInstrumentID[], int
nCount) = 0;
回到顶部


◇ 2.参数
ppInstrumentID：合约数组
nCount：合约数组的数量
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
char **ppInstrumentID = new char*[50];
ppInstrumentID[0] = "T1712";
m_pUserMdApi->SubscribeMarketData(ppInstrumentID, 1);
回到顶部


◇ 5.FAQ
能否订阅重收全天的行情？
不行，只推送最新的行情。
订阅全部合约包含期货和期权所有合约后，发生
OnSessionDisconnected（4097）的报错，是什么原因？
行情前置有个缓冲区限制，一瞬间发送太多超出缓冲
区后就会有触发自我保护机制把session断开，可以尝试分
批订阅，比如每订阅1000个延迟1秒。
回到顶部
< 前页 回目录 后页 >


UnSubscribeForQuoteRsp
退订询价，对应响应OnRspUnSubForQuoteRsp


◇ 1.函数原型
virtual int UnSubscribeForQuoteRsp(char *ppInstrumentID[], int
nCount) = 0;
回到顶部


◇ 2.参数
ppInstrumentID：合约ID
nCount：要订阅/退订行情的合约个数
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部




---

## ReqBatchOrderAction

ReqBatchOrderAction
批量报单操作请求
错误回报: OnErrRtnBatchOrderAction，OnRspBatchOrderAction
正确回报：OnRtnOrder


◇ 1.函数原型
virtual int
ReqBatchOrderAction(CThostFtdcInputBatchOrderActionField
*pInputBatchOrderAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputBatchOrderAction：输入批量报单操作
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代
码
无
TThostFtdcInvestorIDType
InvestorID
投资者代码
必
填
TThostFtdcExchangeIDType
ExchangeID
交易所代码
无
TThostFtdcUserIDType
UserID
用户代码
无
TThostFtdcInvestUnitIDType
InvestUnitID
投资单元代
码
无
TThostFtdcIPAddressType
IPAddress
IP地址
无
TThostFtdcMacAddressType
MacAddress
Mac地址
无
TThostFtdcOrderActionRefType OrderActionRef 报单操作引
用
无
TThostFtdcRequestIDType
RequestID
请求编号
无
TThostFtdcFrontIDType
FrontID
前置编号
无
TThostFtdcSessionIDType
SessionID
会话编号
无
TThostFtdcOldIPAddressType
reserve1
保留的无效
字段
否
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
无
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqCombActionInsert

ReqCombActionInsert
申请组合录入请求
错误响应: OnErrRtnCombActionInsert，OnRspCombActionInsert
正确响应: OnRtnCombAction
详细说明见大商所组保


◇ 1.函数原型
virtual int
ReqCombActionInsert(CThostFtdcInputCombActionField
*pInputCombAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputCombAction：输入的申请组合
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公
司代码
必填
TThostFtdcInvestorIDType
InvestorID
投资者
代码
必填
TThostFtdcInstrumentIDType
InstrumentID
合约代
码
必填
TThostFtdcOrderRefType
CombActionRef 组合引
用
选填
TThostFtdcUserIDType
UserID
用户代
码
无
TThostFtdcExchangeIDType
ExchangeID
交易所
代码
必填
TThostFtdcIPAddressType
IPAddress
IP地址
无
TThostFtdcMacAddressType
MacAddress
Mac地
址
无
TThostFtdcInvestUnitIDType
InvestUnitID
投资单
元代码
无
TThostFtdcVolumeType
Volume
数量
必填
TThostFtdcFrontIDType
FrontID
前置编
号
无
TThostFtdcSessionIDType
SessionID
会话编
号
无


TThostFtdcDirectionType
Direction
买卖方
向
必填
TThostFtdcCombDirectionType
CombDirection
组合指
令方向
申请组
合或者
申请拆
分
TThostFtdcHedgeFlagType
HedgeFlag
投机套
保标志
必填
TThostFtdcOldInstrumentIDType reserve1
保留的
无效字
段
否
TThostFtdcOldIPAddressType
reserve2
保留的
无效字
段
否
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
    CThostFtdcInputCombActionField a = { 0 };
    strcpy_s(a.BrokerID, “9999”);
    strcpy_s(a.InvestorID, “00001”);
    strcpy_s(a.InstrumentID, “STG c1909-P-1680&c1909-C-2020”);
    strcpy_s(a.CombActionRef, "1");
    strcpy_s(a.UserID, “00001”);
    a.Direction = THOST_FTDC_D_Sell;
    a.Volume = 1;
    a.CombDirection = THOST_FTDC_CMDR_Comb;
    a.HedgeFlag = THOST_FTDC_HF_Speculation;
    strcpy_s(a.ExchangeID, “DCE”);
    m_pUserApi->ReqCombActionInsert(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqExecOrderAction

ReqExecOrderAction
执行宣告操作请求、详见期货期权的行权、自对冲
错误响应: OnErrRtnExecOrderAction，OnRspExecOrderAction
正确响应: OnRtnExecOrder


◇ 1.函数原型
virtual int
ReqExecOrderAction(CThostFtdcInputExecOrderActionField
*pInputExecOrderAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputExecOrderAction：输入执行宣告操作
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
TThostFtdcOrderRefType
ExecOrderRef
报单
引用
无
TThostFtdcExchangeIDType
ExchangeID
交易
所代
码
必填
TThostFtdcExecOrderSysIDType ExecOrderSysID
执行
宣告
操作
编号
与执
行宣
告记
录该
编号
一致
TThostFtdcUserIDType
UserID
用户
代码
无
TThostFtdcInstrumentIDType
InstrumentID
合约
代码
无
TThostFtdcInvestUnitIDType
InvestUnitID
投资
单元
无


代码
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
TThostFtdcOrderActionRefType
ExecOrderActionRef
报单
操作
引用
无
TThostFtdcRequestIDType
RequestID
请求
编号
无
TThostFtdcFrontIDType
FrontID
前置
编号
无
TThostFtdcSessionIDType
SessionID
会话
编号
无
TThostFtdcActionFlagType
ActionFlag
操作
标志
删除
TThostFtdcOldInstrumentIDType reserve1
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
CThostFtdcInputExecOrderActionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
a.ExecOrderActionRef = 1;
strcpy_s(a.ExecOrderRef, "00000003");
a.FrontID = 1;
a.SessionID = -7844256;
strcpy_s(a.ExchangeID, "SHFE");
strcpy_s(a.ExecOrderSysID, "         285");
a.ActionFlag = THOST_FTDC_AF_Delete;//删除
strcpy_s(a.UserID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
m_pUserApi->ReqExecOrderAction(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqExecOrderInsert

ReqExecOrderInsert
执行宣告录入请求、详见期货期权的行权、自对冲
错误响应: OnErrRtnExecOrderInsert，OnRspExecOrderInsert
正确响应: OnRtnExecOrder


◇ 1.函数原型
virtual int ReqExecOrderInsert(CThostFtdcInputExecOrderField
*pInputExecOrder, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputExecOrder：输入的执行宣告
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
必填
TThostFtdcInvestorIDType
InvestorID
投
资
者
代
码
必填
TThostFtdcInstrumentIDType
InstrumentID
合
约
代
码
必填
TThostFtdcOrderRefType
ExecOrderRef
执
行
宣
告
引
用
选填


TThostFtdcUserIDType
UserID
用
户
代
码
无
TThostFtdcBusinessUnitType
BusinessUnit
业
务
单
元
无
TThostFtdcExchangeIDType
ExchangeID
交
易
所
代
码
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
TThostFtdcAccountIDType
AccountID
投
资
者
帐
号
无
TThostFtdcCurrencyIDType
CurrencyID
币
种
代
码
无


TThostFtdcClientIDType
ClientID
客
户
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
TThostFtdcVolumeType
Volume
数
量
必填
TThostFtdcRequestIDType
RequestID
请
求
编
号
无
TThostFtdcOffsetFlagType
OffsetFlag
开
平
标
志
必填
TThostFtdcHedgeFlagType
HedgeFlag
投
机
套
保
标
志
投机
或套
保
TThostFtdcActionTypeType
ActionType
执
行
必填


类
型
TThostFtdcPosiDirectionType
PosiDirection
保
留
头
寸
申
请
的
持
仓
方
向
多头
TThostFtdcExecOrderPositionFlagType ReservePositionFlag 期
权
行
权
后
是
否
保
留
期
货
头
寸
的
标
记,
该字
段已
废
弃，
调用
时不
能为
空


该
字
段
已
废
弃
TThostFtdcExecOrderCloseFlagType
CloseFlag
期
权
行
权
后
生
成
的
头
寸
是
否
自
动
平
仓
必填
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
的
无
效
字
段
否
ExecOrderRef：需要纯数字递增，不填则ctp自动填写
IPAddress：手工填写本机IP地址，不自动获取。填写规则如下：
ipv4原样填写，ipv6要转成非零压缩地址，即原始地址，同时要去掉
冒号，eg：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
nRequestID：请求ID，对应响应里的nRequestID，无递增规则，
由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcInputExecOrderField OrderInsert = { 0 };
strcpy_s(OrderInsert.BrokerID, "9999");
strcpy_s(OrderInsert.InvestorID, "1000001");
strcpy_s(OrderInsert.InstrumentID, "rb1809");
strcpy_s(OrderInsert.ExchangeID, "SHFE");
strcpy_s(OrderInsert.ExecOrderRef, "00001");
strcpy_s(OrderInsert.UserID, "1000001");
OrderInsert.Volume = 1;
OrderInsert.RequestID = 1; 
OrderInsert.OffsetFlag = THOST_FTDC_OF_Close;//开平标志
OrderInsert.HedgeFlag = THOST_FTDC_HF_Speculation;//投机套保标
志
OrderInsert.ActionType = THOST_FTDC_ACTP_Exec;//执行类型类型
OrderInsert.PosiDirection = THOST_FTDC_PD_Long;//持仓多空方向
类型
OrderInsert.ReservePositionFlag = THOST_FTDC_EOPF_Reserve;//期
权行权后是否保留期货头寸的标记类型
OrderInsert.CloseFlag = THOST_FTDC_EOCF_NotToClose;//期权行
权后生成的头寸是否自动平仓类型
m_pUserApi->ReqExecOrderInsert(&OrderInsert, nRequestID++);
回到顶部


◇ 5.FAQ
盘中通过api进行中金所行权，报错“CTP:不支持的功
能”为什么？
中金所不支持通过api行权，只能盘后通过会服提交行
权申请。
盘中发出大商所的“取消到期日自动行权”指令后(Volume
字段填0)后，为什么收到的响应字段和请求字段有所区
别？
多次发出相同合约的该指令，CTP会将第一次请求的响
应结果返回给api端，即每次请求收到的响应都是第一次请
求的结果。
各家交易所的行权指令有什么不同？
中金所不支持api发起行权
郑商所closeflag必须为nottoclose
上期、能源、大商所不限制
回到顶部
< 前页 回目录 后页 >


ReqForQuoteInsert
询价录入请求
错误响应: OnErrRtnForQuoteInsert，OnRspForQuoteInsert
正确响应: OnRtnForQuoteRsp


◇ 1.函数原型
virtual int ReqForQuoteInsert(CThostFtdcInputForQuoteField
*pInputForQuote, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputForQuote：输入的询价
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
TThostFtdcInstrumentIDType
InstrumentID 合约代码
期权合
约名称
TThostFtdcOrderRefType
ForQuoteRef 询价引用
选填
TThostFtdcUserIDType
UserID
用户代码
无
TThostFtdcExchangeIDType
ExchangeID
交易所代
码
无
TThostFtdcInvestUnitIDType
InvestUnitID 投资单元
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
TThostFtdcOldInstrumentIDType reserve1
保留的无
效字段
否
ForQuoteRef：需要纯数字递增，不填则ctp自动填写
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
CThostFtdcInputForQuoteField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
strcpy_s(a.UserID, "1000001");
strcpy_s(a.ExchangeID, "SHFE");
m_pUserApi->ReqForQuoteInsert(&a, nRequestID++);
回到顶部


◇ 5.FAQ
询价时报：“没有该合约的做市商”？
询价合约不对。
回到顶部
< 前页 回目录 后页 >


ReqFromBankToFutureByFuture
期货发起银行资金转期货请求
错误响应: OnRspFromBankToFutureByFuture
正确响应: OnRtnFromBankToFutureByFuture


◇ 1.函数原型
virtual int
ReqFromBankToFutureByFuture(CThostFtdcReqTransferField
*pReqTransfer, int nRequestID) = 0;
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
TradeCode：202001 期货发起银行资金转期货。
BankBranchID：为0000
BrokerBranchID：为空


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
CThostFtdcReqTransferField a = { 0 };
strcpy_s(a.TradeCode, "202001");///业务功能码
strcpy_s(a.BankID, "1");
strcpy_s(a.BankBranchID, "0000");///期商代码
strcpy_s(a.BrokerID, "9999"); 
a.LastFragment = THOST_FTDC_LF_Yes;///最后分片标志 '0'=是最
后分片
a.IdCardType = THOST_FTDC_ICT_IDCard;///证件类型
a.CustType = THOST_FTDC_CUSTT_Person;///客户类型
strcpy_s(a.BankAccount, "621485212110187");
strcpy_s(a.AccountID, "1000001");///投资者帐号
strcpy_s(a.Password, "123456");///期货密码--资金密码
a.InstallID = 1;///安装编号
a.FutureSerial = 0;///期货公司流水号
a.VerifyCertNoFlag = THOST_FTDC_YNI_No;///验证客户证件号码
标志
strcpy_s(a.CurrencyID, "CNY");///币种代码
a.TradeAmount = output_num;///转帐金额
a.FutureFetchAmount = 0;///期货可取金额
a.CustFee = 0;///应收客户费用
a.BrokerFee = 0;///应收期货公司费用
a.SecuPwdFlag = THOST_FTDC_BPWDF_BlankCheck;///期货资金
密码核对标志
a.RequestID = 0;///请求编号
a.TID = 0;///交易ID
m_pUserApi->ReqFromBankToFutureByFuture(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


Re

---

## ReqOptionSelfCloseAction

ReqOptionSelfCloseAction
期权自对冲操作请求、详见期货期权的行权、自对冲
错误响应: OnErrRtnOptionSelfCloseAction，
OnRspOptionSelfCloseAction
正确响应: OnRtnOptionSelfClose


◇ 1.函数原型
virtual int
ReqOptionSelfCloseAction(CThostFtdcInputOptionSelfCloseActionField
*pInputOptionSelfCloseAction, int nRequestID) = 0;
回到顶部


◇ 2.参数
pInputOptionSelfCloseAction：输入期权自对冲操作
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
必填
TThostFtdcInvestorIDType
InvestorID
投
资
者
代
码
必填
TThostFtdcOrderRefType
OptionSelfCloseRef
期
权
自
对
冲
引
用
无
TThostFtdcExchangeIDType
ExchangeID
交
易
所
必填


代
码
TThostFtdcOrderSysIDType
OptionSelfCloseSysID
期
权
自
对
冲
编
号
必填
*1，
需要
对应
要撤
的报
单
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
OptionSelfCloseActionRef
期
权
自
对
冲
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
无
TThostFtdcSessionIDType
SessionID
会
话
编
号
无
TThostFtdcActionFlagType
ActionFlag
操
作
标
志
必填
TThostFtdcOldInstrumentIDType reserve1
保
留
的
无
否


效
字
段
TThostFtdcOldIPAddressType
reserve2
保
留
的
无
效
字
段
否
OptionSelfCloseRef：对应要撤销的期权自对冲的引用
FrontID：对应要撤销的期权自对冲的前置编号
SessionID：对应要撤销的期权自对冲的会话编号
ExchangeID：对应要撤销的期权自对冲的交易所编号
ActionFlag：支持删除，不支持修改
InstrumentID：对应要撤销的期权自对冲的合约代码
IPAddress：手工填写本机IP地址，不自动获取。填写规则如下：
ipv4原样填写，ipv6要转成非零压缩地址，即原始地址，同时要去掉
冒号，eg：AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH
nRequestID：请求ID，对应响应里的nRequestID，无递增规则，
由用户自行维护。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcInputOptionSelfCloseActionField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.InvestorID, "1000001"); 
strcpy_s(a.OptionSelfCloseRef, "000000258");//期权自对冲引用
a.FrontID = 1;
a.SessionID = 6442531;
strcpy_s(a.ExchangeID, "SHFE");
a.ActionFlag = THOST_FTDC_AF_Delete;
strcpy_s(a.UserID, "1000001");
strcpy_s(a.InstrumentID, "rb1809");
m_pUserApi->ReqOptionSelfCloseAction(&a, nRequestID++);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


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
