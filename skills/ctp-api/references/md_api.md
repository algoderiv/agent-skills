# Md Api - CTP 6.7.8 API

本节包含 2 个主题。

## 目录

    - [CThostFtdcMdApi](#CThostFtdcMdApi) (第 198 页)
      - [CreateFtdcMdApi](#CreateFtdcMdApi) (第 204 页)

---

## CThostFtdcMdApi

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




---

## CreateFtdcMdApi

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
组播行情前置
true
true
连接mdfront时，在ini中有ThostChannelModel、
ThostUsingMulticast两项用于配置对客户端的连接模式，分三种情
况：
　
ThostChannelModel ThostUsingMulticast 终端配置的模
式
TCP模
式
tcp
　
TCP
UDP模
式
udp
no
TCP、UDP
组播模
式
udp
yes
组播


回到顶部


◇ 3.返回
返回一个CThostFtdcMdApi实例。
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
“RuntimeError:can not open CFlow file in line 279 of file
....\source\userapi\ThostFtdcUserApiImplBase.cpp”程序
一运行就报这个错是为什么？
程序运行之前，flow目录必须提前创建好，否则会报
错。
回到顶部
< 前页 回目录 后页 >


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




---

