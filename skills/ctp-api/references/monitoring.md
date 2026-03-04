# Monitoring

This section contains 1 topics.

## Table of Contents

  - 看穿式监管数据采集说明 (page 156)

---

## 看穿式监管数据采集说明

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
Step 2  中继认证成功后，使用操作员发起登录
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
        cout << "认证失败，" << "ErrorID=" << pRspInfo->ErrorID 
<< "  ,ErrMsg=" << pRspInfo->ErrorMsg << endl;
}
int CUser::ReqUserLogin()
{
    printf("====ReqUserLogin====,用户登录中...\n\n");
    CThostFtdcReqUserLoginField reqUserLogin;
    memset(&reqUserLogin, 0, sizeof(reqUserLogin));
    strcpy_s(reqUserLogin.BrokerID, "8000");
    strcpy(reqUserLogin.UserID, "8000_admin");
    strcpy(reqUserLogin.Password, "1");
    strcpy(reqUserLogin.TradingDay, "");
    strcpy(reqUserLogin.ClientIPAddress, "1.1.1.1");
    reqUserLogin.ClientIPPort = 11;
    return m_pUserApi->ReqUserLogin(&reqUserLogin, 
++RequestID);
}
Step 3  此时有终端登录中继，并将采集信息发送给中继
char pSystemInfo[344];
int len;
CTP_GetSystemInfo(pSystemInfo, len);
cout << "CTP_GetSystemInfo once" << endl;
Step 4  中继接收到采集信息后，发起终端信息的上报
void CUser::SubSystemInfo()
{
    //char pSystemInfo[344];


    //int len;
    ///CTP_GetSystemInfo(pSystemInfo, len);
    /////终端信息由终端发送到中继，并提交信息
    cout << "SubSystemInfo 1" << endl;
    CThostFtdcUserSystemInfoField field;
    memset(&field, 0, sizeof(field));
    strcpy(field.BrokerID, "8000");
    strcpy(field.UserID, "0018881");
    //strcpy(field.ClientSystemInfo, pSystemInfo); 不能用 因为不是
字符串
    memcpy(field.ClientSystemInfo, pSystemInfo, len);
    field.ClientSystemInfoLen = len;
    strcpy(field.ClientPublicIP, "198.114.114.124");
    field.ClientIPPort = 65535;
    strcpy(field.ClientLoginTime, "11:28:28");
    strcpy(field.ClientAppID, "Q7");
    int retx = m_pUserApi->SubmitUserSystemInfo(&field);
    cout << "ret = " << retx << endl;
     //为另一个用户提交采集信息
    CThostFtdcUserSystemInfoField field1;
    memset(&field1, 0, sizeof(field1));
    strcpy(field1.BrokerID, "8000");
    strcpy(field1.UserID, "0018882");
    memcpy(field1.ClientSystemInfo, pSystemInfo, len);
    field1.ClientSystemInfoLen = len;
    strcpy(field1.ClientPublicIP, "198.4.4.120");
    field1.ClientIPPort = 65532;
    strcpy(field1.ClientLoginTime, "11:28:29");
    strcpy(field1.ClientAppID, "Q7");
    m_pUserApi->SubmitUserSystemInfo(&field1);
}
请参阅：


    常见FAQ
    CTP-GetSystemInfo
    CTP-GetDataCollectApiVersion
回到顶部
< 前页 回目录 后页 >




---

