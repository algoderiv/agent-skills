---

## 6.7.8版本更新说明

6.7.8版本更新说明
版本号：v6.7.8_20240918 14:34:34.8082
后台版本：V6.7.8
变更说明：（请各终端厂商根据自身情况进行开发）


◇ 1.API变动


◇ 1.1.新增接口：查询投资者申报费阶梯收取
记录
（1）查询请求
///请求查询投资者申报费阶梯收取记录
virtual int ReqQryInvestorInfoCommRec(CThostFtdcQryInvest
orInfoCommRecField *pQryInvestorInfoCommRec,int nRequestID)
=0;
///查询投资者申报费阶梯收取记录
struct CThostFtdcQryInvestorInfoCommRecField
{
    ///投资者代码
    TThostFtdcExchangeIDType InvestorID;
    ///商品代码
    TThostFtdcInstrumentIDType InstrumentID;
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
};
（2）查询响应
///请求查询投资者申报费阶梯收取记录响应
virtual void OnRspQryInvestorInfoCommRec(CThostFtdcInves
torInfoCommRecField *pInvestorInfoCommRec,CThostFtdcRspInfo
Field *pRspInfo, int nRequestID, bool bIsLast) {};
///投资者申报费阶梯收取记录
CThostFtdcInvestorInfoCommRecField


{
    ///交易所代码
    TThostFtdcExchangeIDType ExchangeID;
    ///经纪公司代码
    TThostFtdcBrokerIDType BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType InvestorID;
    ///商品代码
    TThostFtdcInstrumentIDType InstrumentID;
    ///报单总笔数
    TThostFtdcVolumeType OrderCount;
    ///撤单总笔数
    TThostFtdcVolumeType OrderActionCount;
    ///询价总次数
    TThostFtdcVolumeType ForQuoteCnt;
    ///信息量总量
    TThostFtdcVolumeType InfoCnt;
    ///申报费
    TThostFtdcMoneyType InfoComm;
    ///是否期权系列
    TThostFtdcBoolType IsOptSeries;
    ///品种代码
    ThostFtdcProductIDType ProductID;
}
请求查询时3个入参都支持为空，查询期权系列的申报费时通
过填写标的的商品代码查询。同一个投资者、同一个商品代码可
能会返回两条记录：
一条记录为期货合约的申报费收取记录，IsOptSeries字段返
回为0；
一条记录为以此期货合约为标的的系列期权的申报费收取记
录，IsOptSeries字段返回为1。


回到顶部
< 前页 回目录 后页 >


看穿式监管数据采集说明
■ 6.7.8_API接口说明
└◆ 看穿式监管数据采集说明


◇ 1.定义
术语
术语说明
采集API链接
库
负责采集终端信息的动态链接库(windows版的
WinDataCollect.dll，linux版的LinuxDataCollect.so，
安卓版的InfoCollection_single_release.aar)，只有连
接中继服务器的终端需要调用，直连模式或中继服
务器无需调用
直连类型终
端
直接连接到CTP交易系统的客户交易终端
中继类型终
端
先连接到中继服务器,中继服务器再调用TraderAPI连
接到CTP交易系统的客户交易终端
多对多类型
中继服务器
为每个客户终端，建立CTP API实例，每个用户独占
一个交易API实例的中继服务器
一对多类型
中继服务器
为多个客户终端，建立一个CTP API实例，使用操作
员为多个客户进行交易的中继服务器
AppID
每个期货终端软件需要向期货公司申请自己的
AppID
RelayAppID
中继服务器软件需要向期货公司申请自己的
RelayAppID
AppID和RelayAppID必须符合监控中心的格式要求。见监控
中心官网技术规范。
直连类型终端无需加载采集链接库，登录时会自动采集并上
报终端信息。
中继类型服务器无需加载采集链接库，仅负责接收客户端的
采集信息并上报CTP。


中继类型终端需要加载采集链接库，并将采集到的信息发送
给中继服务器。
回到顶部


◇ 2.说明
期货公司确认终端软件集成了正确的数据采集模块后，为该
AppID的终端软件分配授权码。终端软件需要保护好自己的AppID
和授权码，防止被其他软件盗用。CTP对终端的认证流程如下：
直连终端认证流程
对于直接连接期货公司交易柜台的终端，期货公司确认终端软
件集成了正确的数据采集模块后。给该终端软件（根据AppID）分
配一个授权码。这个授权码和AppID是绑定的，当终端试图登录期
货公司交易软件的时候，交易后台会验证该终端是否持有合法的
AppID和授权码。
中继和中继下属终端的认证流程
对于使用中继服务器连接期货公司交易柜台的终端，期货公司
确认终端软件集成了正确的数据采集模块和确认中继可以正常报送
终端信息后。期货公司给该终端软件（根据AppID）分配一个授权
码，给中继服务器（根据RelayAppID） 分配一个授权码。当终端登
录中继服务器时，中继服务器负责验证终端的合法性；当中继服务
器登录期货公司交易软件的时候，交易后台会验证该中继服务器是
否持有合法的RelayAppID和授权码。
注:信息采集过程中，遇到某些信息没有采集到，无法判断是否
正常。此时可以参考监控中心规范文档里的函数手工调用，判断是
否是权限问题或者相应操作系统命令组件没有部署。如果正常情况
下手工调用能采集到，而采集函数采集不到，请联系上期技术。
回到顶部


◇ 3.权限要求
采集库需要u+s权限
回到顶部


◇ 4.相关接口
///客户端认证请求
virtual int ReqAuthenticate(CThostFtdcReqAuthenticateField 
*pReqAuthenticateField, int nRequestID) = 0;
///客户端认证响应
virtual void OnRspAuthenticate(CThostFtdcRspAuthenticateField 
*pRspAuthenticateField, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
///上报用户终端信息，用于中继服务器操作员登录模式
///操作员登录后，可以多次调用该接口上报客户信息
virtual int SubmitUserSystemInfo(CThostFtdcUserSystemInfoField 
*pUserSystemInfo) = 0;
///注册用户终端信息，用于中继服务器多连接模式
///需要在终端认证成功后，用户登录前调用该接口
virtual int RegisterUserSystemInfo(CThostFtdcUserSystemInfoField 
*pUserSystemInfo) = 0;
注意，采集库不是线程安全的。多线程调用采集库时要加
锁，如果是直连模式则要在登录函数上加锁。
回到顶部


◇ 5.示例代码
每个类型的终端调用流程不同，请注意区分！


◇ 5.1.直连终端的采集使用流程示例代码：
Step 1  在API连接后发起认证
void CUser::OnFrontConnected()
{
    cout << "OnFrontConnected." << endl;
    static const char *version = m_pUserApi->GetApiVersion();
    cout << "------当前版本号 ：" << version << " ------" << endl;
    ReqAuthenticate();
}
int CUser::ReqAuthenticate()
{
    CThostFtdcReqAuthenticateField field;
    memset(&field, 0, sizeof(field));
    strcpy(field.BrokerID, "8000");
    strcpy(field.UserID, "001888");
    strcpy(field.AppID, "XY_Q7_V1.0.0");
    strcpy(field.AuthCode, "5A5P4V7AZ5LCFEAK");
    return m_pUserApi->ReqAuthenticate(&field, 5);
}
Step 2  认证成功后发起登录
void CUser::OnRspAuthenticate(CThostFtdcRspAuthenticateField 
*pRspAuthenticateField, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
{
    printf("OnRspAuthenticate\n");
    if (pRspInfo != NULL && pRspInfo->ErrorID == 0)
    {
        printf("认证成功,ErrorID=0x%04x, ErrMsg=%s\n\n", 


pRspInfo->ErrorID, pRspInfo->ErrorMsg);
        ReqUserLogin();
    }
    else
    cout << "认证失败，" << "ErrorID=" << pRspInfo->ErrorID << "  
,ErrMsg=" << pRspInfo->ErrorMsg << endl;
}
int CUser::ReqUserLogin()
{
    printf("====ReqUserLogin====,用户登录中...\n\n");
    CThostFtdcReqUserLoginField reqUserLogin;
    memset(&reqUserLogin, 0, sizeof(reqUserLogin));
    strcpy_s(reqUserLogin.BrokerID, "8000");
    strcpy(reqUserLogin.UserID, "001888");
    strcpy(reqUserLogin.Password, "1");
    strcpy(reqUserLogin.TradingDay, "20150715");
    return m_pUserApi->ReqUserLogin(&reqUserLogin, 
++RequestID);
}


◇ 5.2.多对多中继终端使用流程示例代码：
Step 1  终端侧采集信息，向中继发起登录，并将终端信息发送给
中继
char pSystemInfo[344];
int len;
CTP_GetSystemInfo(pSystemInfo, len);
cout << "CTP_GetSystemInfo once" << endl;
Step 2  中继收到终端的登录请求，新建一个API实例，并发起连
接
void CUser::OnFrontConnected()
{
    cout << "OnFrontConnected." << endl;
    static const char *version = m_pUserApi->GetApiVersion();
    cout << "------当前版本号 ：" << version << " ------" << endl;
    ReqAuthenticate();
}
Step 3  中继连上前置后，发起认证请求
int CUser::ReqAuthenticate()
{
    CThostFtdcReqAuthenticateField field;
    memset(&field, 0, sizeof(field));
    strcpy(field.BrokerID, "8000");
    strcpy(field.UserID, "001888");
    strcpy(field.AppID, "XY_RELAY_V1.0.0");
    strcpy(field.AuthCode, "5A5P4V7AZ5LCFEAK");


    return m_pUserApi->ReqAuthenticate(&field, 5);
}
Step 4  中继认证成功后，注册终端的采集信息
void CUser::OnRspAuthenticate(CThostFtdcRspAuthenticateField 
*pRspAuthenticateField, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
{
    printf("OnRspAuthenticate\n");
    if (pRspInfo != NULL && pRspInfo->ErrorID == 0)
    {
        printf("认证成功,ErrorID=0x%04x, ErrMsg=%s\n\n", 
pRspInfo->ErrorID, pRspInfo->ErrorMsg);
        RegSystemInfo();
        ReqUserLogin();
    }
    else
        cout << "认证失败，" << "ErrorID=" << pRspInfo->ErrorID 
<< "  ,ErrMsg=" << pRspInfo->ErrorMsg << endl;
}
void CUser::RegSystemInfo()
{
    //char pSystemInfo[344];
    //int len;   
    ////将前面从终端得到的信息赋值给下面结构体
    CThostFtdcUserSystemInfoField field;
    memset(&field, 0, sizeof(field));
    strcpy(field.BrokerID, "8000");
    strcpy(field.UserID, "001888");
    //strcpy(field.ClientSystemInfo, pSystemInfo); 不能用 因为不是
字符串
    memcpy(field.ClientSystemInfo, pSystemInfo, len);
    field.ClientSystemInfoLen = len;


    strcpy(field.ClientPublicIP, "198.4.4.124");
    field.ClientIPPort = 65535;
    strcpy(field.ClientLoginTime, "11:28:28");
    strcpy(field.ClientAppID, "Q7");
    int ret = m_pUserApi->RegisterUserSystemInfo(&field);
    cout << "retd = " << ret << endl;
}
Step 5  中继注册好终端的信息后，发起登录
int CUser::ReqUserLogin()
{
    printf("====ReqUserLogin====,用户登录中...\n\n");
    CThostFtdcReqUserLoginField reqUserLogin;
    memset(&reqUserLogin, 0, sizeof(reqUserLogin));
    strcpy_s(reqUserLogin.BrokerID, "8000");
    strcpy(reqUserLogin.UserID, "001888");
    strcpy(reqUserLogin.Password, "1");
    strcpy(reqUserLogin.TradingDay, "20150715");
    return m_pUserApi->ReqUserLogin(&reqUserLogin, 
++RequestID);
}


◇ 5.3.一对多中继终端使用流程示例代码：
Step 1  中继在启动后，在API连接后发起认证
void CUser::OnFrontConnected()
{
    cout << "OnFrontConnected." << endl;
    static const char *version = m_pUserApi->GetApiVersion();
    cout << "------当前版本号 ：" << version << " ------" << endl;
    ReqAuthenticate();
}
int CUser::ReqAuthenticate()
{
    CThostFtdcReqAuthenticateField field;
    memset(&field, 0, sizeof(field));
    strcpy(field.BrokerID, "8000");
    strcpy(field.UserID, "8000_admin");
    strcpy(field.AppID, " XY_RELAYB_V1.0.0");
    strcpy(field.AuthCode, "BLFLTYGD16FZZ91T");
    return m_pUserApi->ReqAuthenticate(&field, 5);
}
Step 2  中继认证成功后，使用操

---

## CTP-GetDataCollectApiVersion

CTP-GetDataCollectApiVersion
获取采集库版本号


◇ 1.函数原型
const char * CTP_GetDataCollectApiVersion(void);
回到顶部


◇ 2.返回
采集库版本号，格式如下：
Sfit + 生产还是测试秘钥(pro/tst) + 秘钥版本 + 编译时间 + 版本
(内部)
回到顶部
< 前页 回目录 后页 >


行情接口
■ 6.7.8_API接口说明
└◆ 行情接口


◇ 1.说明
行情API提供了两个接口，分别为CThostFtdcMdApi和
CThostFtdcMdSpi。这两个接口对FTD协议进行了封装，方便客户端
应用程序的开发。客户端应用程序可以通过CThostFtdcMdApi发出
操作请求，通过继承CThostFtdcMdSpi并重载回调函数来处理后台服
务的响应。
特别注意：CTP系统在早盘系统启动时，会重演夜盘流水，
此时有可能重复推送整个夜盘的行情。如果用户此时连入CTP就
有可能收到这些重复行情，因此建议用户在处理行情时过滤掉重
复行情，以免影响程序逻辑。
自6.6.2P8柜台版本开始frontmd se行情前置带流重启后，不再
推送夜盘行情数据。
回到顶部


◇ 2.代码示例


◇ 2.1.源代码
下面通过一个简单的代码示例，带大家快速了解下行情API
的使用方法。该示例演示了API的初始化、连接前置、登录行情
系统和订阅行情的过程。
01. //以下是mduserhandle.h文件
02. 
03. #include "ThostFtdcMdApi.h"
04. 
05. #include <stdio.h>
06. 
07. #include <Windows.h>
08. 
09.  
10. 
11. class CMduserHandler : public CThostFtdcMdSpi
12. 
13. {
14. 
15. private:
16. 
17.   CThostFtdcMdApi *m_mdApi;
18. 
19.  
20. 
21. public:
22. 
23.   void connect()
24. 
25.   {
26. 


27. //创建并初始化API
28. 
29.       m_mdApi = 
CThostFtdcMdApi::CreateFtdcMdApi(".//flow_md/", true, true);
30. 
31.       m_mdApi->RegisterSpi(this);
32. 
33.       m_mdApi->RegisterFront("tcp://218.28.130.102:41413");
34. 
35.       m_mdApi->Init();
36. 
37.   }
38. 
39.  
40. 
41. //登陆
42. 
43.   void login()
44. 
45.   {
46. 
47.       CThostFtdcReqUserLoginField t = {0};
48. 
49.       while (m_mdApi->ReqUserLogin(&t, 1)!=0) Sleep(1000);
50. 
51.   }
52. 
53.  
54. 
55. // 订阅行情
56. 
57.   void subscribe()
58. 
59.   {


60. 
61.       char **ppInstrument=new char * [50];
62. 
63.       ppInstrument[0] = "IF1809";
64. 
65.       while (m_mdApi->SubscribeMarketData(ppInstrument, 
1)!=0) Sleep(1000);
66. 
67.   }
68. 
69.  
70. 
71.   //接收行情
72. 
73.   void 
OnRtnDepthMarketData(CThostFtdcDepthMarketDataField 
*pDepthMarketData)
74. 
75.   {
76. 
77.           printf("OnRtnDepthMarketData\n");
78. 
79.   }
80. 
81. };
82. 
83.  
84. 
85. // 以下是main.cpp文件
86. 
87. #include "mduserhandle.h"
88. 
89. int main(int argc, char* argv[])
90. 


91. {
92. 
93.   CMduserHandler *mduser = new CMduserHandler;
94. 
95.   mduser->connect();
96. 
97.   mduser->login();
98. 
99.   mduser->subscribe();
100.    
101.  Sleep(INFINITE);
102.    
103. }


◇ 2.2.代码说明
◇ 2.2.1.继承CThostFtdcMdSpi类
代码一开始通过#include "ThostFtdcMdApi.h"，将
thostmduserapi.lib中声明的类和数据类型包括进来，该头文件中
有两个重要的基类：CThostFtdcMdSpi和CThostFtdcMdApi。
CThostFtdcMdSpi类提供了行情相关的回调接口，用户需要
继承该类并重载这些接口，以获取响应数据。
CThostFtdcMdApi类则是提供了行情相关的请求接口，例
如订阅行情请求、订阅询价请求。
第11行我们声明了一个CMduserHandler类，该类正是继承
了CThostFtdcMdSpi类，并且重载了OnRtnDepthMarketData等接
口。通过这种方式，例如当我们API和前置建立连接成功时，
便会触发OnFrontConnected；当我们发起订阅行情请求成功
后，便会触发OnRspSubMarketData。通过这些回调返回的数
据，我们可以得知订阅是否成功，之后交易系统会通过
OnRtnDepthMarketData实时推送行情。
第17行，声明了一个CThostFtdcMdApi类型的变量
m_mdApi，后续我们会实例化它，创建实例后便可以调用
mdapi提供的登录，订阅行情等接口功能。
◇ 2.2.2.初始化
第23行到37行connect()函数里，实现了线程的初始化，步
骤为：


Step 1  创建Api实例（CThostFtdcMdApi）并为其注册对应的回
调接口类的实例（RegisterSpi）。
Step 2  注册名称服务器网络地址（RegisterNameServer）或注
册前置机网络地址（RegisterFront）。
Step 3  初始化Api（Init），使API建立与前置的连接，连接成
功后回调OnFrontConnected。
第29行创建了一个api实例，并将其赋值给m_mdApi，同时
参数".//flow_md/"指明我们api流水文件存放的目录为flow目
录，因此在运行程序前，我们需要手工创建好这个目录！
流水文件以.con文件类型方式存放流水序号和交易日信
息，这些信息对api正常工作有重要意义，不可随意修改和删
除！并且多个api实例不可共享一个流水文件，必须通过不同
文件夹或者不同流水文件名区分开。虽然行情没有私有流，
但是也建议遵循以上规则。
第31行将自己定义的继承了Spi类的CMduserHandler注册给
CThostFtdcMdApi，这样API就能将收到的各种数据通过Spi类
的接口回调给用户。
第33行注册了前置地址，如果有多个地址，可以多次调用
RegisterFront函数传入不同的地址来实现。API会择优挑选一个
地址进行连接。
第35行调用Init()函数开始正式初始化api，也就是说前面的
工作只是准备工作，到了这里api才真正开始工作。此时api会向
之前注册的地址发起与CTP前置的连接。
完成以上步骤后，客户端就已经和ctp的行情前置建立了连
接，后续我们就可以调用各种API接口完成业务需求。
◇ 2.2.3.登录


第43到51行，前置连接成功后能够开始登陆交易系统了，
先初始化登陆结构体，再赋值相应字段。对于行情的登录来说
只需要调用登录接口即可，目前CTP暂时不对行情做密码校
验。之后发送登陆（ReqUserLogin）指令。通过返回值可以判
断是否发送成功，0表示成功，其他则表示失败，具体可以参考
接口ReqUserLogin。发送不成功可以尝试等待一小会重发。
注意这里返回0不表示登录成功，而是仅仅表示api指令
发出去了。该规则同样适用于其他请求接口，建议在实际应
用中做好超时重发机制，以便在网络丢包的情况下能够及时
重发指令。
◇ 2.2.4.订阅行情
第57到67行，调用订阅行情的接口，我们这里订阅了合
约"IF1809"。若需要批量订阅多个合约，则需要循环把合约输
入到ppInstrumentID中去，同时别忘了更改合约数量（第二个参
数）。订阅发出后通过OnRspSubMarketData响应判断是否订阅
成功。
第73到81行，订阅行情后，通过OnRtnDepthMarketData回
调推送实时行情信息。可以在此时实现自身业务逻辑。
但是如果业务逻辑比较耗时，应该在另外一个线程处
理，而不应该卡在此回调里，否则会导致后续的行情堵塞在
API内部，严重情况下会导致断线。
◇ 2.2.5.程序运行流程


第89到103行，是我们的主函数，该函数是业务实现主体。
首先初始化CMduserHandler类。调用connect函数开始连接ctp前
置，然后依次执行登录和订阅行情操作。
请参阅：
 >> CThostFtdcMdApi
 >> CThostFtdcMdSpi
回到顶部
< 前页 回目录 后页 >


CThostFtdcMdApi
■ 6.7.8_API接口说明
└△ 行情接口
　└◆ CThostFtdcMdApi
CthostFtdcMdApi类提供了行情api的初始化、登录、订阅等功能。


◇ 1.接口
class MD_API_EXPORT CThostFtdcMdApi
{
public:
    ///创建MdApi
    ///@param pszFlowPath 存贮订阅信息文件的目录，默认为当前目
录
    ///@return 创建出的UserApi
    ///modify for udp marketdata
    static CThostFtdcMdApi *CreateFtdcMdApi(const char 
*pszFlowPath = "", const bool bIsUsingUdp=false, const bool 
bIsMulticast=false);
    ///获取API的版本信息
    ///@retrun 获取到的版本号
    static const char *GetApiVersion();
    ///删除接口对象本身
    ///@remark 不再使用本接口对象时,调用该函数删除接口对象
    virtual void Release() = 0;
    ///初始化
    ///@remark 初始化运行环境,只有调用后,接口才开始工作
    virtual void Init() = 0;
    ///等待接口线程结束运行
    ///@return 线程退出代码
    virtual int Join() = 0;
    ///获取当前交易日
    ///@retrun 获取到的交易日
    ///@remark 只有登录成功后,才能得到正确的交易日
    virtual const char *GetTradingDay() = 0;
    ///注册前置机网络地址
    ///@param pszFrontAddress：前置机网络地址。
    ///@remark 网络地址的格式为：“protocol://ipaddress:port”，
如：”tcp://127.0.0.1:17001”。 


    ///@remark “tcp”代表传输协议，“127.0.0.1”代表服务器地
址。”17001”代表服务器端口号。
    virtual void RegisterFront(char *pszFrontAddress) = 0;
    ///注册名字服务器网络地址
    ///@param pszNsAddress：名字服务器网络地址。
    ///@remark 网络地址的格式为：“protocol://ipaddress:port”，
如：”tcp://127.0.0.1:12001”。 
    ///@remark “tcp”代表传输协议，“127.0.0.1”代表服务器地
址。”12001”代表服务器端口号。
    ///@remark RegisterNameServer优先于RegisterFront
    virtual void RegisterNameServer(char *pszNsAddress) = 0;
    ///注册名字服务器用户信息
    ///@param pFensUserInfo：用户信息。
    virtual void RegisterFensUserInfo(CThostFtdcFensUserInfoField * 
pFensUserInfo) = 0;
    ///注册回调接口
    ///@param pSpi 派生自回调接口类的实例
    virtual void RegisterSpi(CThostFtdcMdSpi *pSpi) = 0;
    ///订阅行情。
    ///@param ppInstrumentID 合约ID  
    ///@param nCount 要订阅/退订行情的合约个数
    ///@remark 
    virtual int SubscribeMarketData(char *ppInstrumentID[], int nCount) 
= 0;
    ///退订行情。
    ///@param ppInstrumentID 合约ID  
    ///@param nCount 要订阅/退订行情的合约个数
    ///@remark 
    virtual int UnSubscribeMarketData(char *ppInstrumentID[], int 
nCount) = 0;
    ///订阅询价。
    ///@param ppInstrumentID 合约ID  
    ///@param nCount 要订阅/退订行情的合约个数
    ///@remark 


    virtual int SubscribeForQuoteRsp(char *ppInstrumentID[], int 
nCount) = 0;
    ///退订询价。
    ///@param ppInstrumentID 合约ID  
    ///@param nCount 要订阅/退订行情的合约个数
    ///@remark 
    virtual int UnSubscribeForQuoteRsp(char *ppInstrumentID[], int 
nCount) = 0;
    ///用户登录请求
    virtual int ReqUserLogin(CThostFtdcReqUserLoginField 
*pReqUserLoginField, int nRequestID) = 0;
    ///登出请求
    virtual int ReqUserLogout(CThostFtdcUserLogoutField 
*pUserLogout, int nRequestID) = 0;
    ///请求查询组播合约
    virtual int 
ReqQryMulticastInstrument(CThostFtdcQryMulticastInstrumentField 
*pQryMulticastInstrument, int nRequestID) = 0;
protected:
    ~CThostFtdcMdApi(){};
};
回到顶部


◇ 2.示例代码
class CMduserHandler : public CThostFtdcMdSpi
{
private:
    CThostFtdcMdApi *m_mdApi;
public:
    void connect()
    {
        //创建并初始化API，按照以下顺序
        m_mdApi = CThostFtdcMdApi::CreateFtdcMdApi("", true, true);
        m_mdApi->RegisterSpi(this);
        m_mdApi->RegisterFront("tcp://127.0.0.1:41413");
        m_mdApi->Init();
    }
}
请参阅：
    CreateFtdcMdApi
    GetApiVersion
    GetTradingDay
    Init
    Join
    RegisterFensUserInfo
    RegisterFront
    RegisterNameServer
    RegisterSpi
    Release


    ReqQryMulticastInstrument
    ReqUserLogin
    ReqUserLogout
    SubscribeForQuoteRsp
    SubscribeMarketData
    UnSubscribeForQuoteRsp
    UnSubscribeMarketData
回到顶部
< 前页 回目录 后页 >


CreateFtdcMdApi
创建CThostFtdcMdApi实例。


◇ 1.函数原型
static CThostFtdcMdApi *CreateFtdcMdApi(const char
*pszFlowPath = "", const bool bIsUsingUdp=false, const bool
bIsMulticast=false);
回到顶部


◇ 2.参数
pszFlowPath：常量字符指针，用于指定一

---

## GetApiVersion

GetApiVersion
获取API的版本信息


◇ 1.函数原型
virtual const char * GetApiVersion () = 0;
回到顶部


◇ 2.返回
const char * 返回一个指向版本字符串的指针
回到顶部


◇ 3.调用示例
CThostFtdcMdApi  *pUserMdApi = 
CThostFtdcMdApi::CreateFtdcMdApi();
printf("版本号为:%s\n", pUserMdApi->GetApiVersion());
CSimpleMdHandler ash(pUserMdApi);
pUserMdApi->RegisterSpi(&ash);
pUserMdApi->RegisterFront(“tcp://127.0.0.1:41205”);
pUserMdApi->Init();
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >


GetTradingDay
获得当前交易日。只有当登陆成功后才会取到正确的值。


◇ 1.函数原型
virtual const char *GetTradingDay() = 0;
回到顶部


◇ 2.返回
返回一个指向日期信息字符串的常量指针。
回到顶部


◇ 3.调用示例
CThostFtdcMdApi  *pUserMdApi = 
CThostFtdcMdApi::CreateFtdcMdApi();
CSimpleMdHandler ash(pUserMdApi);
pUserMdApi->RegisterSpi(&ash);
pUserMdApi->RegisterFront(“tcp://127.0.0.1:41213”);
pUserMdApi->Init();
//登录成功后
printf("获取当前交易日期:%s\n", pUserMdApi->GetTradingDay());
回到顶部


◇ 4.FAQ
登录前为什么也可以获取到交易日？
登录前调用该函数会去上一次的API流水文件里去获
取交易日。只有在登录后才能获取到正确的交易日。
回到顶部
< 前页 回目录 后页 >


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




---


