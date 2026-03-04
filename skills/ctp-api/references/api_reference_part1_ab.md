取交易日。只有在登录后才能获取到正确的交易日。
回到顶部
< 前页 回目录 后页 >




---

## Init

Init
初始化运行环境,只有调用后,接口才开始发起前置的连接请求。


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
CThostFtdcMdApi  *pUserMdApi = 
CThostFtdcMdApi::CreateFtdcMdApi();
CSimpleMdHandler ash(pUserMdApi);
pUserMdApi->RegisterSpi(&ash);
pUserMdApi->RegisterFront(“tcp://127.0.0.1:41205”);
pUserMdApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## Join

Join
客户端等待接口线程结束运行。当调用该函数时会阻塞当前线
程，直到api线程退出。


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
pFensUserInfo：Fens用户信息
struct CThostFtdcFensUserInfoField
{
///经纪公司代码
TThostFtdcBrokerIDType BrokerID;
///用户代码
TThostFtdcUserIDType UserID;
///登录模式
TThostFtdcLoginModeType LoginMode;
};  
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcMdApi  *pUserMdApi = 
CThostFtdcMdApi::CreateFtdcMdApi();
CSimpleMdHandler ash(pUserMdApi);
pUserMdApi->RegisterSpi(&ash);
CThostFtdcFensUserInfoField pFensUserInfo = { 0 };
strcpy_s(pFensUserInfo.BrokerID, "9999");
strcpy_s(pFensUserInfo.UserID, "00001");
pFensUserInfo.LoginMode = THOST_FTDC_LM_Trade;
pUserMdApi->RegisterFensUserInfo(&pFensUserInfo);
pUserMdApi-> RegisterNameServer ("tcp://127.0.0.1:41213");
pUserMdApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## RegisterFront

RegisterFront
设置交易托管系统的网络通讯地址，交易托管系统拥有多个通信
地址，用户可以注册一个或多个地址。如果注册多个地址则使用最先
建立TCP连接的地址。


◇ 1.函数原型
virtual void RegisterFront(char *pszFrontAddress) = 0;
回到顶部


◇ 2.参数
pszFrontAddress：指向后台服务器地址的指针。
服务器地址的格式
为：“protocol://ipaddress:port”如：”tcp://127.0.0.1:17001”。“tcp”代
表传输协议，“127.0.0.1”代表服务器地址。”17001”代表行情端口
号。
SSL前置格式：ssl://192.168.0.1:41205
TCP前置IPv4格式：tcp://192.168.0.1:41205
TCP前置IPv6格式：tcp6://fe80::20f8:aa9b:7d59:887d:35001
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcMdApi  *pUserMdApi = 
CThostFtdcMdApi::CreateFtdcMdApi();
CSimpleMdHandler ash(pUserMdApi);
pUserMdApi->RegisterSpi(&ash);
//此处注册多个前置
pUserMdApi->RegisterFront(“tcp://192.168.0.1:41213”); 
pUserMdApi->RegisterFront(“tcp://192.168.0.2:41213”);
pUserMdApi->Init();
回到顶部


◇ 5.FAQ
如果我注册了多个前置，会选择一个最优的前置进行连
接吗？
会以最先建立TCP连接的地址作为当前连接地址进行
连接。
回到顶部
< 前页 回目录 后页 >




---

## RegisterNameServer

RegisterNameServer
设置名字服务器网络地址。RegisterNameServer优先于
RegisterFront。
调用前需要先使用RegisterFensUserInfo设置登录模式。
如果CTP系统启用了fens前置，则可以使用该接口连接fens前置地
址。
fens的好处是fens地址对应的后端地址是一个前置地址池，前置
地址的增删改都对用户透明，用户不需要调整自己的接入地址。当
API使用fens地址接入时，fens前置会返回一个地址池，随后API择
优选择一个地址进行接入。
详见fens连接说明


◇ 1.函数原型
virtual void RegisterNameServer(char *pszNsAddress) = 0;
回到顶部


◇ 2.参数
pszNsAddress:指向后台服务器地址的指针。
服务器地址的格式为：“protocol://ipaddress:port”。
如：“tcp://127.0.0.1:17001”。“tcp”代表传输协议，“127.0.0.1”代表
服务器地址。“17001”代表服务器端口号。
SSL前置格式：ssl://192.168.0.1:41205
TCP前置IPv4格式：tcp://192.168.0.1:41205
TCP前置IPv6格式：tcp6://fe80::20f8:aa9b:7d59:887d:35001
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
CThostFtdcMdApi  *pUserMdApi = 
CThostFtdcMdApi::CreateFtdcMdApi();
CSimpleMdHandler ash(pUserMdApi);
pUserMdApi->RegisterSpi(&ash);
CThostFtdcFensUserInfoField pFensUserInfo = { 0 };
strcpy_s(pFensUserInfo.BrokerID, g_chBrokerID);
strcpy_s(pFensUserInfo.UserID, g_chUserID);
pFensUserInfo.LoginMode = THOST_FTDC_LM_Trade;
pUserMdApi->RegisterFensUserInfo(&pFensUserInfo, 
nRequestID++);
pUserMdApi->RegisterNameServer (“tcp://127.0.0.1:41205”);
pUserMdApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## RegisterSpi

RegisterSpi
注册一个派生自CThostFtdcMdSpi接口类的实例，该实例将完成事
件处理。


◇ 1.函数原型
virtual void RegisterSpi(CThostFtdcMdSpi *pSpi) = 0;
回到顶部


◇ 2.参数
pSpi：实现了CThostFtdcMdSpi接口的实例指针。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## Release

Release
不再使用本接口对象时,调用该函数删除接口对象


◇ 1.函数原型
virtual void Release() = 0;
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

## ReqQryMulticastInstrument

ReqQryMulticastInstrument
请求查询组播合约，对应响应OnRspQryMulticastInstrument
目前只有上期所、原油交易所有组播行情
上期所topicid：1001(一档行情)，1000(五档行情)
原油交易所topicid：5001(一档行情)，5000(五档行情)
注意：该函数只有连接支持交易所组播行情的mdfront才能获得
完整功能，连接其他行情前置则不能使用该函数。
详细说明见二代行情接入


◇ 1.函数原型
virtual int
ReqQryMulticastInstrument(CThostFtdcQryMulticastInstrumentField
*pQryMulticastInstrument, int nRequestID) = 0;
回到顶部


◇ 2.参数
QryMulticastInstrument：请求查询组播合约
    struct CThostFtdcQryMulticastInstrumentField
    {
        ///主题号
        TThostFtdcInstallIDType TopicID;
        ///合约代码
        TThostFtdcInstrumentIDType  InstrumentID;
    };
TopicID：对应交易所组播行情主题号。
回到顶部


◇ 3.返回
0，代表成功。
-1，表示网络连接失败；
-2，表示未处理请求超过许可数；
-3，表示每秒发送请求数超过许可数。
回到顶部


◇ 4.调用示例
CThostFtdcQryMulticastInstrumentField a = { 0 };
a.TopicID = 1001;//对应上期所的组播行情topic
//a.TopicID = 5001;//对应原油交易所的组播行情topic
strcpy_s(g_chInstrumentID,"cu1906");
m_pUserMdApi->ReqQryMulticastInstrument(&a, 1);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


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




---

## ReqUserLogout

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


◇ 4.调用示例
char **ppInstrumentID = new char*[50];
ppInstrumentID[0] = “sc1801”;
m_pUserMdApi->SubscribeForQuoteRsp(ppInstrumentID, 1);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


UnSubscribeMarketData
退订行情，对应响应OnRspUnSubMarketData。


◇ 1.函数原型
virtual int UnSubscribeMarketData(char *ppInstrumentID[], int
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
ppInstrumentID[0] = "T1712";
m_pUserMdApi->UnSubscribeMarketData(ppInstrumentID, 1);
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >




---

## OnFrontConnected

OnFrontConnected
当客户端与交易托管系统建立起通信连接时（还未登录前），该
方法被调用。本方法在完成初始化后调用，可以在其中完成用户登录
任务。


◇ 1.函数原型
virtual void OnFrontConnected(){};
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

## OnFrontDisconnected

OnFrontDisconnected
当客户端与交易托管系统通信连接断开时，该方法被调用。当发
生这个情况后，API会自动重新连接，客户端可不做处理。自动重连
地址，可能是原来注册的地址，也可能是系统支持的其它可用的通信
地址，它由程序自动选择。
注:重连之后需要重新认证、登录


◇ 1.函数原型
virtual void OnFrontDisconnected(int nReason){};
回到顶部


◇ 2.参数
nReason：连接断开原因，为10进制值，因此需要转成16进制后
再参照下列代码：
0x1001 网络读失败
0x1002 网络写失败
0x2001 接收心跳超时
0x2002 发送心跳失败
0x2003 收到错误报文
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
API每次断线都会自动重连，可以设置为不重连或者指定
重连时间间隔吗？
断线重连时间不可自定义，并且也无法通过参数设置
来主动取消断线重连机制。
回到顶部
< 前页 回目录 后页 >




---

## OnHeartBeatWarning

OnHeartBeatWarning
心跳超时警告。当长时间未收到报文时，该方法被调用。目前该
方法暂不启用。


◇ 1.函数原型
virtual void OnHeartBeatWarning(int nTimeLapse){};
回到顶部


◇ 2.参数
nTimeLapse：距离上次接收报文的时间
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
无
回到顶部
< 前页 回目录 后页 >


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

## 交易接口

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
90.     


91.           CThostFtdcSettlementInfoConfirmField t = {0};
92.     
93.           strcpy_s(t.BrokerID, "1701");
94.     
95.           strcpy_s(t.InvestorID, "00001");
96.     
97.           while (m_ptraderapi->ReqSettlementInfoConfirm(&t, 
2)!=0)  Sleep(1000);
98.     
99.       }
100.        
101.         
102.        
103.          //报单
104.        
105.          void orderinsert()
106.        
107.          {
108.        
109.              CThostFtdcInputOrderField t = { 0 };
110.        
111.              strcpy_s(t.BrokerID, "9999");
112.        
113.              strcpy_s(t.InvestorID, "00001");
114.        
115.              strcpy_s(t.UserID, "00001");
116.        
117.              t.Direction = THOST_FTDC_D_Buy;
118.        
119.              t.CombOffsetFlag[0] = THOST_FTDC_OF_Open;
120.        
121.              t.CombHedgeFlag[0] = 
THOST_FTDC_HF_Speculation;
122.        


123.              t.ContingentCondition = 
THOST_FTDC_CC_Immediately;
124.    
125.              strcpy_s(t.InstrumentID, "sc1807");
126.        
127.              t.ForceCloseReason = 
THOST_FTDC_FCC_NotForceClose;
128.        
129.              t.LimitPrice = 490;
130.        
131.              t.OrderPriceType = THOST_FTDC_OPT_LimitPrice;
132.        
133.              t.VolumeCondition = THOST_FTDC_VC_AV;
134.        
135.              t.TimeCondition = THOST_FTDC_TC_GFD;
136.        
137.              t.VolumeTotalOriginal = 1;
138.        
139.              strcpy_s(t.OrderRef, "0000001");
140.    
141.              strcpy_s(t.ExchangeID, "INE");
142.        
143.              while (m_ptraderapi->ReqOrderInsert(&t, 3) != 0) 
Sleep(1000);
144.        
145.          }
146.        
147.          //查询合约
148.        
149.          void qryInstrument()
150.        
151.          {
152.        
153.              CThostFtdcQryInstrumentField t = { 0 };


154.        
155.              strcpy_s(t.ExchangeID, "SHFE");
156.        
157.              strcpy_s(t.InstrumentID, "zn1803");
158.        
159.              while (m_ptraderapi->ReqQryInstrument(&t, 4) != 0) 
Sleep(1000);
160.        
161.          }
162.        
163.         
164.        
165.        //前置连接响应
166.        
167.          void OnFrontConnected()
168.        
169.          {
170.        
171.              printf("OnFrontConnected\n");
172.        
173.          }
174.        
175.         
176.        
177.          //登陆请求响应
178.        
179.          void OnRspUserLogin(CThostFtdcRspUserLoginField 
*pRspUserLogin, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
180.        
181.          {
182.        
183.              printf("OnRspUserLogin\n");
184.        


185.          }
186.        
187.         
188.        
189.          //结算单确认响应
190.        
191.          void 
OnRspSettlementInfoConfirm(CThostFtdcSettlementInfoConfirmFie
ld *pSettlementInfoConfirm, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
192.        
193.          {
194.        
195.              printf("OnRspSettlementInfoConfirm\n");
196.        
197.          }
198.        
199.         
200.        
201.          //查询合约响应
202.        
203.          void OnRspQryInstrument(CThostFtdcInstrumentField 
*pInstrument, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast)
204.        
205.          {
206.        
207.              printf("OnRspQryInstrument\n");
208.        
209.          }
210.         
211.        
212.          //报单通知
213.        


214.          void OnRtnOrder(CThostFtdcOrderField *pOrder)
215.        
216.          {
217.        
218.              printf("OnRtnOrder\n");
219.        
220.          }
221.        
222.          
223.        
224.          //成交通知
225.        
226.          void OnRtnTrade(CThostFtdcTradeField *pTrade)
227.        
228.          {
229.        
230.              printf("OnRtnTrade\n");
231.        
232.          }
233.        
234.         
235.        
236.          //报单录入请求响应
237.        
238.          void OnRspOrderInsert(CThostFtdcInputOrderField 
*pInputOrder, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast)
239.        
240.          {
241.        
242.              printf("OnRspOrderInsert\n");
243.        
244.          }
245.        


246.          
247.        
248.          //错误应答
249.        
250.          void OnRspError(CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
251.        
252.          {
253.        
254.              printf("OnRspError\n");
255.        
256.          }
257.        
258.        };
259.        
260.        //以下是main.cpp文件 
261.        
262.        #include "tradehandler.h"
263.        
264.        int main(int argc, char *argv[])
265.        
266.        {
267.        
