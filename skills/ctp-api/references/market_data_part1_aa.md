# Market Data - CTP 6.7.8 API

本节包含 16 个主题。

## 目录

  - [行情接口](#行情接口) (第 187 页)
      - [SubscribeForQuoteRsp](#SubscribeForQuoteRsp) (第 280 页)
      - [SubscribeMarketData](#SubscribeMarketData) (第 286 页)
      - [UnSubscribeForQuoteRsp](#UnSubscribeForQuoteRsp) (第 292 页)
      - [UnSubscribeMarketData](#UnSubscribeMarketData) (第 298 页)
      - [OnErrRtnForQuoteInsert](#OnErrRtnForQuoteInsert) (第 460 页)
      - [OnErrRtnQuoteAction](#OnErrRtnQuoteAction) (第 509 页)
      - [OnErrRtnQuoteInsert](#OnErrRtnQuoteInsert) (第 516 页)
      - [ReqForQuoteInsert](#ReqForQuoteInsert) (第 1571 页)
      - [ReqQryDepthMarketData](#ReqQryDepthMarketData) (第 1706 页)
      - [ReqQryForQuote](#ReqQryForQuote) (第 1748 页)
      - [ReqQryQuote](#ReqQryQuote) (第 1892 页)
      - [ReqQuoteInsert](#ReqQuoteInsert) (第 2001 页)
    - [协议方式接收二代组播行情](#协议方式接收二代组播行情) (第 2331 页)
    - [各交易所行情区别](#各交易所行情区别) (第 2345 页)
    - [行情流控](#行情流控) (第 2454 页)

---

## 行情接口

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
pszFlowPath：常量字符指针，用于指定一个文件目录来存贮交
易托管系统发布消息的状态。默认值代表当前目录。
bIsUsingUdp：是否使用UDP行情
bIsMulticast：是否使用组播行情
组播行情只能在内网中使用，需要咨询所连接的系统是否支
持组播行情。
各类型行情字段组合如下：
　
bIsUsingUdp
bIsMulticast
TCP行情前置
false
false
UDP行情前置
true
false
组播行情

---

## SubscribeForQuoteRsp

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




---

## SubscribeMarketData

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




---

## UnSubscribeForQuoteRsp

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




---

## UnSubscribeMarketData

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


CThostFtdcMdSpi
■ 6.7.8_API接口说明
└△ 行情接口
　└◆ CThostFtdcMdSpi
CthostFtdcMdSpi类提供了行情相关的回调接口，用户需要继承该
类并重载这些接口，以获取响应数据。


◇ 1.接口
class CThostFtdcMdSpi
{
public:
    ///当客户端与交易后台建立起通信连接时（还未登录前），该方
法被调用。
    virtual void OnFrontConnected(){};
    ///当客户端与交易后台通信连接断开时，该方法被调用。当发生
这个情况后，API会自动重新连接，客户端可不做处理。
    ///@param nReason 错误原因
    ///        0x1001 网络读失败
    ///        0x1002 网络写失败
    ///        0x2001 接收心跳超时
    ///        0x2002 发送心跳失败
    ///        0x2003 收到错误报文
    virtual void OnFrontDisconnected(int nReason){};
    ///心跳超时警告。当长时间未收到报文时，该方法被调用。
    ///@param nTimeLapse 距离上次接收报文的时间
    virtual void OnHeartBeatWarning(int nTimeLapse){};
    ///登录请求响应
    virtual void OnRspUserLogin(CThostFtdcRspUserLoginField 
*pRspUserLogin, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///登出请求响应
    virtual void OnRspUserLogout(CThostFtdcUserLogoutField 
*pUserLogout, CThostFtdcRspInfoField *pRspInfo, int nRequestID, 
bool bIsLast) {};
    ///请求查询组播合约响应
    virtual void 
OnRspQryMulticastInstrument(CThostFtdcMulticastInstrumentField 
*pMulticastInstrument, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};


    ///错误应答
    virtual void OnRspError(CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///订阅行情应答
    virtual void 
OnRspSubMarketData(CThostFtdcSpecificInstrumentField 
*pSpecificInstrument, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///取消订阅行情应答
    virtual void 
OnRspUnSubMarketData(CThostFtdcSpecificInstrumentField 
*pSpecificInstrument, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///订阅询价应答
    virtual void 
OnRspSubForQuoteRsp(CThostFtdcSpecificInstrumentField 
*pSpecificInstrument, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///取消订阅询价应答
    virtual void 
OnRspUnSubForQuoteRsp(CThostFtdcSpecificInstrumentField 
*pSpecificInstrument, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast) {};
    ///深度行情通知
    virtual void 
OnRtnDepthMarketData(CThostFtdcDepthMarketDataField 
*pDepthMarketData) {};
    ///询价通知
    virtual void OnRtnForQuoteRsp(CThostFtdcForQuoteRspField 
*pForQuoteRsp) {};
};
回到顶部


◇ 2.示例代码
// CMduserHandler继承CThostFtdcMdSpi
class CMduserHandler : public CThostFtdcMdSpi    
{ 
public:
      //重载，接收行情
       void OnRtnDepthMarketData(CThostFtdcDepthMarketDataField 
*pDepthMarketData)
       {
                     printf("OnRtnDepthMarketData\n");
       }
}
请参阅：
    OnFrontConnected
    OnFrontDisconnected
    OnHeartBeatWarning
    OnRspError
    OnRspQryMulticastInstrument
    OnRspSubForQuoteRsp
    OnRspSubMarketData
    OnRspUnSubForQuoteRsp
    OnRspUnSubMarketData
    OnRspUserLogin
    OnRspUserLogout
    OnRtnDepthMarketData


    OnRtnForQuoteRsp
回到顶部
< 前页 回目录 后页 >


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




---

## OnErrRtnForQuoteInsert

OnErrRtnForQuoteInsert
询价录入错误回报，当执行ReqForQuoteInsert后有字段填写不对
之类的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void
OnErrRtnForQuoteInsert(CThostFtdcInputForQuoteField
*pInputForQuote, CThostFtdcRspInfoField *pRspInfo) {};
回到顶部


◇ 2.参数
pInputForQuote：输入的询价
struct CThostFtdcInputForQuoteField
{
    ///经纪公司代码
    TThostFtdcBrokerIDType  BrokerID;
    ///投资者代码
    TThostFtdcInvestorIDType    InvestorID;
    ///保留的无效字段
    TThostFtdcOldInstrumentIDType   reserve1;
    ///询价引用
    TThostFtdcOrderRefType  ForQuoteRef;
    ///用户代码
    TThostFtdcUserIDType    UserID;
    ///交易所代码
    TThostFtdcExchangeIDType    ExchangeID;
    ///投资单元代码
    TThostFtdcInvestUnitIDType  InvestUnitID;
    ///保留的无效字段
    TThostFtdcOldIPAddressType  reserve2;
    ///Mac地址
    TThostFtdcMacAddressType    MacAddress;
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
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.FAQ
无
回到顶部
< 前页 回目录 后页 >


OnErrRtnFutureToBankByFuture
期货发起期货资金转银行错误回报，当执行
ReqFromFutureToBankByFuture后有字段填写不对之类的CTP报错则通
过此接口返回


◇ 1.函数原型
virtual void
OnErrRtnFutureToBankByFuture(CThostFtdcReqTransferField
*pReqTransfer, CThostFtdcRspInfoField *pRspInfo) {};
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
TradeCode: 202002期货发起期货资金转银行
BankID：返回银行在期货公司的代码
BankBranchID：填0000
BrokerBranchID：一般为空


CustomerName：客户姓名
BankPassWord：返回银行的密码，都是“*”密码不显示，
Password：返回期货账户的密码，都是“*”密码不显示
ErrorMsg：返回转账成功与否的状态
TradeAmount：转账金额
pRspInfo：响应信息
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


OnErrRtnOptionSelfCloseAction
期权自对冲操作错误回报，当执行ReqOptionSelfCloseAction后有
字段填写不对之类的CTP报错则通过此接口返回


◇ 1.函数原型
virtual void
OnErrRtnOptionSelfCloseAction(CThostFtdcOptionSelfCloseActionFiel
d *pOptionSelfCloseAction, CThostFtdcRspInfoField *pRspInfo) {};
回到顶部


◇ 2.参数
pOptionSelfCloseAction：期权自对冲操作
struct CThostFtdcOptionSelfCloseActionField
{
///经纪公司代码
TThostFtdcBrokerIDType  BrokerID;
///投资者代码
TThostFtdcInvestorIDType    InvestorID;
///期权自对冲操作引用
TThostFtdcOrderActionRefType    OptionSelfCloseActionRef;
///期权自对冲引用
TThostFtdcOrderRefType  OptionSelfCloseRef;
///请求编号
TThostFtdcRequestIDType RequestID;
///前置编号
TThostFtdcFrontIDType   FrontID;
///会话编号
TThostFtdcSessionIDType SessionID;
///交易所代码
TThostFtdcExchangeIDType    ExchangeID;
///期权自对冲操作编号
TThostFtdcOrderSysIDType    OptionSelfCloseSysID;
///操作标志
TThostFtdcActionFlagType    ActionFlag;
///操作日期
TThostFtdcDateType  ActionDate;
///操作时间
TThostFtdcTimeType  ActionTime;
///交易所交易员代码
TThostFtdcTraderIDType  TraderID;
///安装编号


TThostFtdcInstallIDType InstallID;
///本地期权自对冲编号
TThostFtdcOrderLocalIDType  OptionSelfCloseLocalID;
///操作本地编号
TThostFtdcOrderLocalIDType  ActionLocalID;
///会员代码
TThostFtdcParticipantIDType ParticipantID;
///客户代码
