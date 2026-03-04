# Trader Api - CTP 6.7.8 API

本节包含 2 个主题。

## 目录

    - [CThostFtdcTraderApi](#CThostFtdcTraderApi) (第 1434 页)
      - [CreateFtdcTraderApi](#CreateFtdcTraderApi) (第 1457 页)

---

## CThostFtdcTraderApi

CThostFtdcTraderApi
■ 6.7.8_API接口说明
└△ 交易接口
　└◆ CThostFtdcTraderApi
CThostFtdcTraderApi类提供了交易api的初始化、登录、报单和查
询等功能。


◇ 1.接口
class TRADER_API_EXPORT CThostFtdcTraderApi
{
public:
    ///创建TraderApi
    ///@param pszFlowPath 存贮订阅信息文件的目录，默认为当前目
录
    ///@return 创建出的UserApi
    static CThostFtdcTraderApi *CreateFtdcTraderApi(const char 
*pszFlowPath = "");
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
    ///获取已连接的前置的信息
    /// @param pFrontInfo：输入输出参数，用于存储获取到的前置
信息，不能为空
    /// @remark 连接成功后，可获取正确的前置地址信息
    /// @remark 登录成功后，可获取正确的前置流控信息
    virtual void GetFrontInfo(CThostFtdcFrontInfoField* pFrontInfo) 


=0;
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
    virtual void RegisterSpi(CThostFtdcTraderSpi *pSpi) = 0;
    ///订阅私有流。
    ///@param nResumeType 私有流重传方式  
    ///        THOST_TERT_RESTART:从本交易日开始重传
    ///        THOST_TERT_RESUME:从上次收到的续传
    ///        THOST_TERT_QUICK:只传送登录后私有流的内容
    ///@remark 该方法要在Init方法前调用。若不调用则不会收到私
有流的数据。
    virtual void SubscribePrivateTopic(THOST_TE_RESUME_TYPE 
nResumeType) = 0;
    ///订阅公共流。
    ///@param nResumeType 公共流重传方式  


    ///        THOST_TERT_RESTART:从本交易日开始重传
    ///        THOST_TERT_RESUME:从上次收到的续传
    ///        THOST_TERT_QUICK:只传送登录后公共流的内容
    ///        THOST_TERT_NONE:取消订阅公共流
    ///@remark 该方法要在Init方法前调用。若不调用则不会收到公
共流的数据。
    virtual void SubscribePublicTopic(THOST_TE_RESUME_TYPE 
nResumeType) = 0;
    ///客户端认证请求
    virtual int ReqAuthenticate(CThostFtdcReqAuthenticateField 
*pReqAuthenticateField, int nRequestID) = 0;
    ///注册用户终端信息，用于中继服务器多连接模式
    ///需要在终端认证成功后，用户登录前调用该接口
    virtual int RegisterUserSystemInfo(CThostFtdcUserSystemInfoField 
*pUserSystemInfo) = 0;
    ///上报用户终端信息，用于中继服务器操作员登录模式
    ///操作员登录后，可以多次调用该接口上报客户信息
    virtual int SubmitUserSystemInfo(CThostFtdcUserSystemInfoField 
*pUserSystemInfo) = 0;
    ///用户登录请求
    virtual int ReqUserLogin(CThostFtdcReqUserLoginField 
*pReqUserLoginField, int nRequestID) = 0;
    ///登出请求
    virtual int ReqUserLogout(CThostFtdcUserLogoutField 
*pUserLogout, int nRequestID) = 0;
    ///用户口令更新请求
    virtual int 
ReqUserPasswordUpdate(CThostFtdcUserPasswordUpdateField 
*pUserPasswordUpdate, int nRequestID) = 0;
    ///资金账户口令更新请求
    virtual int 
ReqTradingAccountPasswordUpdate(CThostFtdcTradingAccountPassw
ordUpdateField *pTradingAccountPasswordUpdate, int nRequestID) = 
0;


    ///查询用户当前支持的认证模式
    virtual int 
ReqUserAuthMethod(CThostFtdcReqUserAuthMethodField 
*pReqUserAuthMethod, int nRequestID) = 0;
    ///用户发出获取图形验证码请求
    virtual int 
ReqGenUserCaptcha(CThostFtdcReqGenUserCaptchaField 
*pReqGenUserCaptcha, int nRequestID) = 0;
    ///用户发出获取短信验证码请求
    virtual int ReqGenUserText(CThostFtdcReqGenUserTextField 
*pReqGenUserText, int nRequestID) = 0;
    ///用户发出带有图片验证码的登陆请求
    virtual int 
ReqUserLoginWithCaptcha(CThostFtdcReqUserLoginWithCaptchaFiel
d *pReqUserLoginWithCaptcha, int nRequestID) = 0;
    ///用户发出带有短信验证码的登陆请求
    virtual int 
ReqUserLoginWithText(CThostFtdcReqUserLoginWithTextField 
*pReqUserLoginWithText, int nRequestID) = 0;
    ///用户发出带有动态口令的登陆请求
    virtual int 
ReqUserLoginWithOTP(CThostFtdcReqUserLoginWithOTPField 
*pReqUserLoginWithOTP, int nRequestID) = 0;
    ///报单录入请求
    virtual int ReqOrderInsert(CThostFtdcInputOrderField *pInputOrder, 
int nRequestID) = 0;
    ///预埋单录入请求
    virtual int ReqParkedOrderInsert(CThostFtdcParkedOrderField 
*pParkedOrder, int nRequestID) = 0;
    ///预埋撤单录入请求
    virtual int 
ReqParkedOrderAction(CThostFtdcParkedOrderActionField 
*pParkedOrderAction, int nRequestID) = 0;
    ///报单操作请求


    virtual int ReqOrderAction(CThostFtdcInputOrderActionField 
*pInputOrderAction, int nRequestID) = 0;
    ///查询最大报单数量请求
    virtual int 
ReqQryMaxOrderVolume(CThostFtdcQryMaxOrderVolumeField 
*pQryMaxOrderVolume, int nRequestID) = 0;
    ///投资者结算结果确认
    virtual int 
ReqSettlementInfoConfirm(CThostFtdcSettlementInfoConfirmField 
*pSettlementInfoConfirm, int nRequestID) = 0;
    ///请求删除预埋单
    virtual int 
ReqRemoveParkedOrder(CThostFtdcRemoveParkedOrderField 
*pRemoveParkedOrder, int nRequestID) = 0;
    ///请求删除预埋撤单
    virtual int 
ReqRemoveParkedOrderAction(CThostFtdcRemoveParkedOrderActio
nField *pRemoveParkedOrderAction, int nRequestID) = 0;
    ///执行宣告录入请求
    virtual int ReqExecOrderInsert(CThostFtdcInputExecOrderField 
*pInputExecOrder, int nRequestID) = 0;
    ///执行宣告操作请求
    virtual int 
ReqExecOrderAction(CThostFtdcInputExecOrderActionField 
*pInputExecOrderAction, int nRequestID) = 0;
    ///询价录入请求
    virtual int ReqForQuoteInsert(CThostFtdcInputForQuoteField 
*pInputForQuote, int nRequestID) = 0;
    ///报价录入请求
    virtual int ReqQuoteInsert(CThostFtdcInputQuoteField 
*pInputQuote, int nRequestID) = 0;
    ///报价操作请求
    virtual int ReqQuoteAction(CThostFtdcInputQuoteActionField 
*pInputQuoteAction, int nRequestID) = 0;


    ///批量报单操作请求
    virtual int 
ReqBatchOrderAction(CThostFtdcInputBatchOrderActionField 
*pInputBatchOrderAction, int nRequestID) = 0;
    ///期权自对冲录入请求
    virtual int 
ReqOptionSelfCloseInsert(CThostFtdcInputOptionSelfCloseField 
*pInputOptionSelfClose, int nRequestID) = 0;
    ///期权自对冲操作请求
    virtual int 
ReqOptionSelfCloseAction(CThostFtdcInputOptionSelfCloseActionFie
ld *pInputOptionSelfCloseAction, int nRequestID) = 0;
    ///申请组合录入请求
    virtual int ReqCombActionInsert(CThostFtdcInputCombActionField 
*pInputCombAction, int nRequestID) = 0;
    ///请求查询报单
    virtual int ReqQryOrder(CThostFtdcQryOrderField *pQryOrder, int 
nRequestID) = 0;
    ///请求查询成交
    virtual int ReqQryTrade(CThostFtdcQryTradeField *pQryTrade, int 
nRequestID) = 0;
    ///请求查询投资者持仓
    virtual int 
ReqQryInvestorPosition(CThostFtdcQryInvestorPositionField 
*pQryInvestorPosition, int nRequestID) = 0;
    ///请求查询资金账户
    virtual int 
ReqQryTradingAccount(CThostFtdcQryTradingAccountField 
*pQryTradingAccount, int nRequestID) = 0;
    ///请求查询投资者
    virtual int ReqQryInvestor(CThostFtdcQryInvestorField 
*pQryInvestor, int nRequestID) = 0;
    ///请求查询交易编码
    virtual int ReqQryTradingCode(CThostFtdcQryTradingCodeField 


*pQryTradingCode, int nRequestID) = 0;
    ///请求查询合约保证金率
    virtual int 
ReqQryInstrumentMarginRate(CThostFtdcQryInstrumentMarginRateFi
eld *pQryInstrumentMarginRate, int nRequestID) = 0;
    ///请求查询合约手续费率
    virtual int 
ReqQryInstrumentCommissionRate(CThostFtdcQryInstrumentCommis
sionRateField *pQryInstrumentCommissionRate, int nRequestID) = 0;
    ///请求查询交易所
    virtual int ReqQryExchange(CThostFtdcQryExchangeField 
*pQryExchange, int nRequestID) = 0;
    ///请求查询产品
    virtual int ReqQryProduct(CThostFtdcQryProductField 
*pQryProduct, int nRequestID) = 0;
    ///请求查询合约
    virtual int ReqQryInstrument(CThostFtdcQryInstrumentField 
*pQryInstrument, int nRequestID) = 0;
    ///请求查询行情
    virtual int 
ReqQryDepthMarketData(CThostFtdcQryDepthMarketDataField 
*pQryDepthMarketData, int nRequestID) = 0;
    ///请求查询投资者结算结果
    virtual int ReqQrySettlementInfo(CThostFtdcQrySettlementInfoField 
*pQrySettle

---

## CreateFtdcTraderApi

CreateFtdcTraderApi
创建TraderApi实例。如果创建多个api实例，则每个实例的flow目
录都要区分开，否则可能会导致报单回报丢失。


◇ 1.函数原型
static CThostFtdcTraderApi *CreateFtdcTraderApi(const char
*pszFlowPath = "");
回到顶部


◇ 2.参数
pszFlowPath：常量字符指针，用于指定一个文件目录来存贮交
易托管系统发布消息的状态。默认值代表当前目录。
回到顶部


◇ 3.返回
无
回到顶部


◇ 4.调用示例
//初始化api
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("flow\\01\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
pUserApi->SubscribePrivateTopic(THOST_TERT_QUICK);
pUserApi->SubscribePublicTopic(THOST_TERT_QUICK);
pUserApi->RegisterFront("tcp://127.0.0.1:41205");
pUserApi->Init();
//创建第二个api实例，要区分开flow目录
CThostFtdcTraderApi *pUserApi2 = 
CThostFtdcTraderApi::CreateFtdcTraderApi("flow\\02\\");
回到顶部


◇ 5.FAQ
“RuntimeError:can not open CFlow file in line 279 of file
....\source\userapi\ThostFtdcUserApiImplBase.cpp” 报错
是什么意思？
程序运行之前，flow目录必须提前创建好，否则会报
错。
“RuntimeError:can not open CFlow file in line 338 of file
....\source\userapi\ThostFtdcUserApiImplBase.cpp” 报错
是什么意思？
有core文件生成，调试后发现断点在CThostUserFlow::
OpenFile中，可能是ulimit参数“open files”太小导致不能开
启更多线程。
回到顶部
< 前页 回目录 后页 >


GetApiVersion
获取API的版本信息


◇ 1.函数原型
virtual const char *GetApiVersion() = 0;
回到顶部


◇ 2.参数
无
回到顶部


◇ 3.返回
返回具体的版本号，如（v6.3.11_20180109 14:59:39）
回到顶部


◇ 4.调用示例
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
LOG(pUserApi->GetApiVersion());
pUserApi->SubscribePrivateTopic(THOST_TERT_QUICK);
pUserApi->SubscribePublicTopic(THOST_TERT_QUICK);
pUserApi->RegisterFront(“tcp://127.0.0.1:51205”);
pUserApi->Init();
回到顶部


◇ 5.FAQ
无
回到顶部
< 前页 回目录 后页 >


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


GetFrontInfo
获取已连接的前置的信息。包含前置地址、查询流控参数、FTD
流控参数。连接成功后，可获取正确的前置地址信息，登录成功后，
可获取正确的前置查询流控和FTD流控信息。


◇ 1.函数原型
virtual void GetFrontInfo(CThostFtdcFrontInfoField* pFrontInfo)
=0;
回到顶部




---

