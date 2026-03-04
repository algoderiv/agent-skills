# Md Spi - CTP 6.7.8 API

本节包含 1 个主题。

## 目录

    - [CThostFtdcMdSpi](#CThostFtdcMdSpi) (第 304 页)

---

## CThostFtdcMdSpi

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




---

