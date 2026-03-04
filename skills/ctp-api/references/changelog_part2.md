# Changelog - CTP 6.7.8 API

本节包含 16 个主题。


## GetApiVersion

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


Init
使客户端开始与交易托管系统建立连接，连接成功后可以进行登
陆。
非线程安全，多线程使用请加锁。


◇ 1.函数原型
virtual void Init() = 0;
回到顶部




---


