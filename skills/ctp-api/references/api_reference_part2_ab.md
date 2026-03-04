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


OnRspExecOrderAction
执行宣告操作请求响应，当执行ReqExecOrderAction后，该方法
被调用


◇ 1.函数原型
virtual void
OnRspExecOrderAction(CThostFtdcInputExecOrderActionField
*pInputExecOrderAction, CThostFtdcRspInfoField *pRspInfo, int
nRequestID, bool bIsLast) {};
回到顶部




---

## GetTradingDay

GetTradingDay
获得当前交易日。只有当登陆成功后才会取到正确的值。


◇ 1.函数原型
virtual const char *GetTradingDay() = 0;
回到顶部


◇ 2.参数
无
回到顶部


◇ 3.返回
返回一个指向日期信息字符串的常量指针。
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
pUserApi->SubscribePrivateTopic(THOST_TERT_QUICK);
pUserApi->SubscribePublicTopic(THOST_TERT_QUICK);
pUserApi->RegisterFront(“tcp://127.0.0.1:51205”);
pUserApi->Init();
WaitForSingleObject(g_LoginSig, INFINITE); //等待登陆成功后
printf(pUserApi->GetTradingDay()); //获取交易日
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## GetFrontInfo

GetFrontInfo
获取已连接的前置的信息。包含前置地址、查询流控参数、FTD
流控参数。连接成功后，可获取正确的前置地址信息，登录成功后，
可获取正确的前置查询流控和FTD流控信息。


◇ 1.函数原型
virtual void GetFrontInfo(CThostFtdcFrontInfoField* pFrontInfo)
=0;
回到顶部


◇ 2.参数
pFrontInfo:前置信息
字段类型
字段名称
含义
值
TThostFtdcAddressType
FrontAddr
前置地址
无
TThostFtdcQueryFreqType
QryFreq
查询流控
无
TThostFtdcQueryFreqType
FTDPkgFreq
FTD流控
无
回到顶部


◇ 3.返回
返回一个前置信息。
回到顶部


◇ 4.调用示例
CThostFtdcFrontInfoField g_chpFrontInfo = {};
pUserApi->GetFrontInfo(&g_chpFrontInfo);
printf("%s\n",g_chpFrontInfo.FrontAddr);
printf("%d\n",g_chpFrontInfo.FTDPkgFreq);
printf("%d\n", g_chpFrontInfo.QryFreq);
回到顶部


◇ 5.FAQ
回到顶部
< 前页 回目录 后页 >




---

## Init

Init
使客户端开始与交易托管系统建立连接，连接成功后可以进行登
陆。
非线程安全，多线程使用请加锁。


◇ 1.函数原型
virtual void Init() = 0;
回到顶部


◇ 2.参数
无
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
pUserApi->SubscribePrivateTopic(THOST_TERT_QUICK);
pUserApi->SubscribePublicTopic(THOST_TERT_QUICK);
pUserApi->RegisterFront(“tcp://127.0.0.1:51205”);
pUserApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## Join

Join
客户端等待一个接口实例线程的结束。


◇ 1.函数原型
virtual int Join() = 0;
回到顶部


◇ 2.参数
无
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## RegisterFensUserInfo

RegisterFensUserInfo
注册名字服务器用户信息，调用RegisterNameServer前需要先使用
RegisterFensUserInfo设置登录模式。
详见fens连接说明


◇ 1.函数原型
virtual void RegisterFensUserInfo(CThostFtdcFensUserInfoField *
pFensUserInfo) = 0;
回到顶部


◇ 2.参数
CThostFtdcFensUserInfoField：Fens用户信息
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代码
必填
TThostFtdcUserIDType
UserID
用户代码
必填
TThostFtdcLoginModeType
LoginMode
登录模式
必填
LoginMode：填写THOST_FTDC_LM_Trade
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
printf(pUserApi->GetApiVersion());
pUserApi->SubscribePrivateTopic(THOST_TERT_QUICK);
pUserApi->SubscribePublicTopic(THOST_TERT_QUICK);
CThostFtdcFensUserInfoField pFensUserInfo = { 0 };
strcpy_s(pFensUserInfo.BrokerID, "9999");
strcpy_s(pFensUserInfo.UserID, "1000001");
pFensUserInfo.LoginMode = THOST_FTDC_LM_Trade;
pUserApi->RegisterFensUserInfo(&pFensUserInfo);
pUserApi-> RegisterNameServer("tcp://127.0.0.1:41205");
pUserApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## RegisterFront

RegisterFront
设置交易托管系统的网络通讯地址，交易托管系统拥有多个通信
地址，但用户只需要选择一个通信地址。
也可以通过多次调用的方式注册不同的前置地址实现冗余。当交
易系统断开连接时会自动从注册的地址池中择优选择一个建立连接。
择优方式为以最先建立TCP连接的地址为连接地址。


◇ 1.函数原型
virtual void RegisterFront(char *pszFrontAddress) = 0;
回到顶部


◇ 2.参数
pszFrontAddress：指向后台服务器地址的指针。
服务器地址的格式为：“protocol://ipaddress:port”，
如：”tcp://127.0.0.1:17001”。“tcp”代表传输协议，“127.0.0.1”代表
服务器地址。”17001”代表交易端口号。
SSL前置格式：ssl://192.168.0.1:41205
TCP前置IPv4格式：tcp://192.168.0.1:41205
TCP前置IPv6格式：tcp6://fe80::20f8:aa9b:7d59:887d:35001
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow \\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
pUserApi->SubscribePrivateTopic(THOST_TERT_RESUME);
pUserApi->SubscribePublicTopic(THOST_TERT_RESUME);
//注册多个前置地址
pUserApi->RegisterFront(“tcp://192.168.1.10:51205”);
pUserApi->RegisterFront(“tcp://192.168.1.11:51205”);
pUserApi->RegisterFront(“tcp://192.168.1.12:51205”);
pUserApi->Init();
回到顶部


◇ 5.FAQ
“CTP:无此功能”是什么错？
端口如果填写错误，例如交易和行情写反，则会报。
回到顶部
< 前页 回目录 后页 >




---

## RegisterNameServer

RegisterNameServer
设置名字服务器网络地址。RegisterNameServer优先于
RegisterFront。
调用前需要先使用RegisterFensUserInfo设置登录模式。
详见fens连接说明


◇ 1.函数原型
virtual void RegisterNameServer(char *pszNsAddress) = 0;
回到顶部


◇ 2.参数
pszNsAddress:指向后台服务器地址的指针。
服务器地址的格式为：“protocol://ipaddress:port”，
如：”tcp://127.0.0.1:17001”。“tcp”代表传输协议，“127.0.0.1”代表
服务器地址。”17001”代表服务器端口号。
SSL前置格式：ssl://192.168.0.1:41205
TCP前置IPv4格式：tcp://192.168.0.1:41205
TCP前置IPv6格式：tcp6://fe80::20f8:aa9b:7d59:887d:35001
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
printf(pUserApi->GetApiVersion());
pUserApi->SubscribePrivateTopic(THOST_TERT_QUICK);
pUserApi->SubscribePublicTopic(THOST_TERT_QUICK);
CThostFtdcFensUserInfoField pFensUserInfo = { 0 };
strcpy_s(pFensUserInfo.BrokerID, "9999");
strcpy_s(pFensUserInfo.UserID, "1000001");
pFensUserInfo.LoginMode = THOST_FTDC_LM_Trade;
pUserApi->RegisterFensUserInfo(&pFensUserInfo);
pUserApi-> RegisterNameServer (“tcp://127.0.0.1:41205”);
pUserApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## RegisterSpi

RegisterSpi
注册一个派生自CThostFtdcTraderSpi 接口类的实例，该实例将完
成事件处理。


◇ 1.函数原型
virtual void RegisterSpi(CThostFtdcTraderSpi *pSpi) = 0;
回到顶部


◇ 2.参数
pSpi：实现了CThostFtdcTraderSpi接口的实例指针。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\api_liu\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
pUserApi->SubscribePrivateTopic(THOST_TERT_QUICK);
pUserApi->SubscribePublicTopic(THOST_TERT_QUICK);
pUserApi->RegisterFront(“tcp://127.0.0.1:51205”);
pUserApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## RegisterUserSystemInfo

RegisterUserSystemInfo
注册用户终端信息，用于中继服务器多连接模式
需要在终端认证成功后，用户登录前调用该接口


◇ 1.函数原型
virtual int
RegisterUserSystemInfo(CThostFtdcUserSystemInfoField
*pUserSystemInfo) = 0;
回到顶部


◇ 2.参数
CThostFtdcUserSystemInfoField ：用户系统信息
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
TThostFtdcUserIDType
UserID
用户
代码
必
填
TThostFtdcClientSystemInfoType
ClientSystemInfo
用户
端系
统内
部信
息
必
填
TThostFtdcIPAddressType
ClientPublicIP
终端IP
端口
必
填
TThostFtdcTimeType
ClientLoginTime
登录
成功
时间
必
填
TThostFtdcAppIDType
ClientAppID
App代
码
必
填
TThostFtdcSystemInfoLenType
ClientSystemInfoLen
用户
端系
统内
部信
息长
度
必
填


TThostFtdcIPPortType
ClientIPPort
终端IP
端口
必
填
TThostFtdcOldIPAddressType
reserve1
保留
的无
效字
段
否
TThostFtdcClientLoginRemarkType ClientLoginRemark
客户
登录
备注2
否
ClientSystemInfoLen：存储的为加密后的用户终端系统内部信
息的长度
ClientSystemInfo：存储的为加密后的用户终端系统内部信息。
ClientPublicIP：存储的为用户终端IP，由中继服务器采集和填
写
ClientLoginTime：存储的为用户登录中继时间，由中继服务器
采集和填写
ClientAppid：存储的为用户终端的appid，由中继服务器采集和
填写
回到顶部


◇ 3.返回
0 正确
-1 字段长度不对
-2 非CTP采集的终端信息
-3 当前终端类型非多对多中继
-5 字段中存在非法字符或者长度超限
-6 采集结果字段错误
-7 采集库的版本类型和生产库的不一致
回到顶部


◇ 4.调用示例
//多对多中继终端使用流程
Step 1  终端侧采集信息 向中继发起登录 并将终端信息发送给中继
char pSystemInfo[344];
int len;
CTP_GetSystemInfo(pSystemInfo, len);
Step 2  中继收到终端的登录请求，发起终端认证
ReqAuthenticate();
Step 3  终端认证成功后，中继上报用户信息
RegisterUserSystemInfo()
{
    char pSystemInfo[344];
    int len;
    ////将从终端得到的信息赋值给下面结构体
    CThostFtdcUserSystemInfoField field;
    memset(&field, 0, sizeof(field));
    strcpy(field.BrokerID, "9999");
    strcpy(field.UserID, "00001");
    memcpy(field.ClientSystemInfo, pSystemInfo, len);
    field.ClientSystemInfoLen = len;
    strcpy(field.ClientPublicIP, "127.0.0.1");
    field.ClientIPPort = 65535;
    strcpy(field.ClientLoginTime, "11:28:28");
    strcpy(field.ClientAppID, "aaa");
    m_pUserApi->RegisterUserSystemInfo(&field);
}


Step 4  中继上报成功后，发起登录
ReqUserLogin();
Step 5  中继登录成功
回到顶部


◇ 5.FAQ
采集信息上报时候总是提示operation not permitted，这
是为什么？
如果提示operation not permitted，可能是AppID类型错
误。例如，直连模式的AppID，却错误调用了SubmitUserS
ystemInfo。
此错误不会通过特定接口返回，只在标准输出中提
示，例如直接在屏幕上打印出来。
不上报采集信息，会影响登录吗？
不影响登录，CTP不做控制，但这样不符合监管要
求。
多对多中继代理模式的AppID怎么填？是填RelayAppID
还是ClientAppID？
认证时用relayappid，上报时填客户端的appid。
使用中继模式，已经认证成功了。用工具查看不到采集
获得的信息，但是在代码里看到的pSystemInfo不为空
的。这可能是什么原因？
采集不落库，基本上都是采集的数据在转储时被截断
了。
一定要使用memcpy，不要把采集的数据当成字符
串，他不是字符串。
还有一种常见错误，ClientSystemInfoLen不按照采集
库的返回值填写，而是自己用strlen()之类的函数去获取。
回到顶部


< 前页 回目录 后页 >




---

## Release

Release
删除接口对象本身
不再使用本接口对象时,调用该函数删除接口对象
非线程安全，多线程使用请加锁。
建议使用logout函数登出，等自动重连后再重新登录，以实现api
实例重复使用。不建议直接release api实例。


◇ 1.函数原型
virtual void Release() = 0;
回到顶部


◇ 2.参数
无
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
template <class TUserApi>
void CUserApiEnv<TUserApi>::UnInitialUserApi()
{
    // 释放UserApi
    if (m_pUserApi)
    {
        m_pUserApi->Release();
        m_pUserApi = NULL;
    }
    // 释放UserSpi实例
    if (m_pUserSpiImpl)
    {
        delete m_pUserSpiImpl;
        m_pUserSpiImpl = NULL;
    }
}
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## ReqAuthenticate

ReqAuthenticate
客户端认证请求，对应响应OnRspAuthenticate。如果交易系统开
启了强制终端认证，则必须认证通过后才能发起登陆；如果未开启，
则不需要认证即可登陆，此时如果主动去认证，不管成功或失败，也
不影响后续登陆。


◇ 1.函数原型
virtual int ReqAuthenticate(CThostFtdcReqAuthenticateField
*pReqAuthenticateField, int nRequestID) = 0;
回到顶部


◇ 2.参数
pReqAuthenticateField：客户端认证请求
字段类型
字段名称
含义
值
TThostFtdcBrokerIDType
BrokerID
经纪公司代码
必填
TThostFtdcUserIDType
UserID
用户代码
必填
TThostFtdcProductInfoType UserProductInfo 用户端产品信息无
TThostFtdcAuthCodeType
AuthCode
认证码
必填
TThostFtdcAppIDType
AppID
App代码
必填
UserProductInfo：客户端的产品信息，如软件开发商、版本号
等。
CTP后台用户事件中的用户登录事件所显示的用户端产品信
息取自ReqAuthentication接口里的UserProductInfo，而非
ReqUserLogin里的。
AuthCode：认证码需要向期货公司申请得到
AppID：必填项，不然认证失败，如果没有则要向期货公司申
请，申请的AppID必须遵循监控中心规范格式
UserID：必填项，用户代码。
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
CThostFtdcReqAuthenticateField a = { 0 };
strcpy_s(a.BrokerID, "9999");
strcpy_s(a.UserID, "1000001");
strcpy_s(a.UserProductInfo, "mytest");
strcpy_s(a.AuthCode, "MLX0LEA4L4UPUCBF"); 
strcpy_s(a.AppID, "mytest");
m_pUserApi->ReqAuthenticate(&a, nRequestID++);
回到顶部


◇ 5.FAQ
每次重连都需要做一遍客户端认证吗？
重新连接都需要重新认证一下。
非穿透式版本，开通了强制认证，需要申请一个认证码
的。穿透式版本，也需要一个穿透式的认证码。如果想
同时开通非穿透式强制认证和穿透式认证，这样就要两
个认证码；但API里是只需要一个认证码，那请问这种情
况怎么解决？
这种场景是不存在的，因为穿透式和非穿透式版本的
api互不兼容，所以不存在用一个api去同时做新旧前置的
认证。
在使用“看穿式监管信息采集评测工具”时候，发现获取
到的记录数和数据库中信息的条目数不匹配。这是为什
么？
数据库中的记录为乱码的话就不显示。
采集信息库是否是线程安全的？能多个线程同时调用
吗？
不是线程安全的，不能同时调用。
ErrorID=4043, ErrorMsg-CTP:用户与客户端授权验证失
败 请问这个报错的原因是什么？
认证使用的appid没有授权给当前投资者账号使用。需
要柜台重新指定下。
回到顶部


< 前页 回目录 后页 >


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


ReqExecOrderInsert
执行宣告录入请求、详见期货期权的行权、自对冲
错误响应: OnErrRtnExecOrderInsert，OnRspExecOrderInsert
正确响应: OnRtnExecOrder





