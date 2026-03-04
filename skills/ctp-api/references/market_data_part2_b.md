CTGateFtdcSpi。这两个接口对FTD协议进行了封装，方便客户端应
用程序的开发。客户端应用程序可以通过CTGateFtdcApi发出操作请
求，通过继承CTGateFtdcSpi并重载回调函数来处理后台服务的响
应。与TradeApi的使用方法基本相同。
客户端调用网关API至少需要两个线程，一个是应用程序主线
程，一个是网关API工作线程。应用程序与网关通讯是由 API工作线
程驱动的。CTGateFtdcApi提供的接口是线程安全的，可以有多个应
用程序线程同时发出请求。CTGateFtdcSpi提供的接口回调是由API
工作线程驱动，通过实现SPI中的接口方法，可以从网关服务端收取
所需数据。
如果重载的某个回调函数阻塞，则等于阻塞了API工作线程，
API与网关的通讯会停止。因此，在CTGateFtdcSpi派生类的回调函
数中，应迅速返回，此处不建议在回调中发送请求或处理其他代码
逻辑，不要阻塞。
注意以下事项：
最大支持5000个终端连接
在途查询流控为1笔
Release不是线程安全的，多线程使用需要加锁
API请求的输入参数不能为 NULL。
API请求的返回参数，0表示正确，其他表示错误。
回到顶部


◇ 5.TGateApi示例
本次API新增一个查询接口ReqQryTGIpAddrParam，入参
BrokerID、UserID要求必填。
下面通过一个简单的代码示例，带大家快速了解下网关API的
使用方法。该示例演示了API如何接入网关并查询服务地址的过
程。
1   #include "TGateFtdcApi.h"           
2   #include "TGateFtdcApiDataType.h"           
3   #include "TGateFtdcApiStruct.h"         
4   #include <iostream>         
5   #include <string>           
6   #include <Windows.h>            
7               
8   class TGUser : public CTGateFtdcSpi         
9   {           
10  public:         
11      TGUser(){};     
12      ~TGUser(){};        
13              
14      ///服务地址参数查询     
15      void ReqQryTGIpAddrParam()      
16      {       
17          string m_sTGFrontAddr = "tcp://127.0.0.1";  
18          string m_sBrokerid = "0001";    
19          string m_sUserid = "123456789"; 
20          ///创建TGateApi实例 
21          CTGateFtdcApi *m_pTGApi = 
CTGateFtdcApi::CreateTGateFtdcApi();  
22          m_pTGApi->RegisterSpi(this);    
23          m_pTGApi->RegisterTGAddr(const_cast<char *>


(m_sTGFrontAddr.c_str()));   
24          ///调用TGateApi查询服务地址请求   
25          CTGateFtdcQryTGIpAddrParamField* 
pQryTGIpAddrParamField = new 
CTGateFtdcQryTGIpAddrParamField;  
26          memset(pQryTGIpAddrParamField, 0, 
sizeof(pQryTGIpAddrParamField));  
27          ///该查询接口的重要传参   
28          strcpy(pQryTGIpAddrParamField->BrokerID, 
m_sBrokerid.c_str());  
29          strcpy(pQryTGIpAddrParamField->UserID, 
m_sUserid.c_str());  
30          int RequestID = 0;  
31          int rst = m_pTGApi-
>ReqQryTGIpAddrParam(pQryTGIpAddrParamField, RequestID); 
32          Sleep(1000);    
33      }       
34              
35      ///服务地址参数查询响应       
36      void 
OnRspQryTGIpAddrParam(CTGateFtdcTGIpAddrParamField* 
pTGIpAddrParam, CTGateFtdcRspInfoField* pRspInfo, int 
nRequestID, bool bIsLast)            
37      {       
38          string m_sFrontAddr = pTGIpAddrParam->Address;  
39          printf("<OnRspQryTGIpAddrParam>\n");    
40      }       
41  }           
注意：连到非tgate地址，会返回-4 "API验证失败"的报错
回到顶部
< 前页 回目录




---



