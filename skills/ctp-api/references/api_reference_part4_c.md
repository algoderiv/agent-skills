ReqQryMulticastInstrument查询到映射关系。
注意，普通front不提供ReqQryMulticastInstrument查询接口，
调用不会返回有效数据。
综上所述，需要有三个部分才能拼装出完整的行情，分别为增
量行情、行情快照和组播合约映射关系。
回到顶部


◇ 3.方法
下面是推荐方法，仅做参考：
1、订阅CTP行情。
可以连接mdfront组件接收行情，因为CTP端的行情是完整的行
情，而非增量，所以可以作为基准快照，在后续拼装完整行情的时
候使用。
2、订阅期货公司转发的二代行情。
具有解码能力的投资者，可以订阅该行情，并参考交易所二代
行情协议规范做相应解码。解码后会得到增量行情，但是这里要注
意，这个增量行情还不能直接拿来用，因为该行情里没有
InstrumentID，只有合约编号InstrumentNo，而CTP mdfront的行情里
是InstrumentID，两者无法关联。为了解决这个问题，新的mdapi增
加了ReqQryMulticastInstrument接口，该接口会返回InstrumentID和
InstrumentNo的对应关系，有了这个函数便可以拼出完整的行情。
注意，仅新的组播mdfront支持该函数，普通行情front和老mdfront
不支持该函数！完整行情拼装方法如下：
a) 盘前接入
开盘后，CTP推送每tick快照行情，转发行情则推送每tick增量
行情。投资者程序判断同一合约两边的UpdateTime和
UpdateMillisec。如果一致，则将mdfront的这笔tick快照行情作为该
合约的基准快照，后续的增量行情都在此基础上做拼装，得到笔笔
完整行情。
b) 盘中接入
盘中，如果客户接入，则方法同上述a，投资者程序实时判断两
边的UpdateTime和UpdateMillisec，直到遇到一致，则作为基准快


照。
c) 盘中丢行情
交易所的二代行情带行情序号，如果行情序号不连续了，说明
行情丢包了。此时应该停止行情组装工作，重新寻找最新的行情快
照，方法同a。
回到顶部


◇ 4.代码示例
下面代码演示了如何接入mdfront前置，查询组播合约，并订阅
行情。
// mduserhandle.h
#include "ThostFtdcMdApi.h"
#include <stdio.h>
#include <Windows.h>
TThostFtdcInstrumentIDType  g_chInstrumentID;
class CMduserHandler : public CThostFtdcMdSpi
{
    private:
      CThostFtdcMdApi *m_mdApi;
    public:
    void connect()
    {
    //创建并初始化API
        m_mdApi = CThostFtdcMdApi::CreateFtdcMdApi("", true, true);
        m_mdApi->RegisterSpi(this);
        m_mdApi->RegisterFront("tcp://218.28.130.102:41413");
        m_mdApi->Init();
    }
    //登陆
    void login()
    {
        CThostFtdcReqUserLoginField t = {0};
        while (m_mdApi->ReqUserLogin(&t, 1)!=0) Sleep(1000);
    }
    //请求查询组播合约
     void ReqQryMulticastInstrument()
    {


        CThostFtdcQryMulticastInstrumentField a = { 0 };
        a.TopicID = 1001;
        strcpy_s(a.InstrumentID, "cu1905");
        while (m_mdApi->ReqQryMulticastInstrument(&a, 1)!=0) 
Sleep(1000);
    }
    void 
OnRspQryMulticastInstrument(CThostFtdcMulticastInstrumentField 
*pMulticastInstrument, CThostFtdcRspInfoField *pRspInfo, int 
nRequestID, bool bIsLast)
    {
        if (pMulticastInstrument)
        {
            strcpy_s(g_chInstrumentID,pMulticastInstrument-
>InstrumentID);
        }
    }
    // 订阅行情
    void subscribe()
    {
        char **ppInstrument=new char * [50];
        ppInstrumentID[0] = g_chInstrumentID;
        while (m_mdApi->SubscribeMarketData(ppInstrument, 1)!=0) 
Sleep(1000);
    }
    //接收行情
    void OnRtnDepthMarketData(CThostFtdcDepthMarketDataField 
*pDepthMarketData)
    {   
            printf("OnRtnDepthMarketData\n");
    }
};
// main.cpp
#include "mduserhandle.h"


int main(int argc, char* argv[])
{
    CMduserHandler *mduser = new CMduserHandler;
    mduser->connect();
    mduser->login();
    mduser->ReqQryMulticastInstrument();
    mduser->subscribe();
    Sleep(INFINITE);
}
回到顶部
< 前页 回目录 后页 >


fens连接说明
fens机制提供了另一种接入CTP的方式。


◇ 1.优势
通常情况下，用户为了实现交易前置接入的冗余备份，会用
RegisterFront注册多个地址，当所连前置发生故障时，API会自动连
接到其他已注册的可用地址。但是，CTP规定用户同一时间只能在
一个中心有交易权限，基于这个前提，所有注册的地址必须是同一
中心的，否则API在择优挑选地址时如果连接到其他中心，用户是
没有交易权限的；当发生灾备切换时，用户需要从主中心的前置切
换到备中心的前置，此时就不得不中断交易，退出程序，重新注册
备中心的前置地址进行连接。可见，当发生中心切换时，
RegisterFront方式带来的连接恢复成本是很高的。
再比如，一些客户端在打包发布时，往往集成了前置地址列
表。但是，如果期货公司更改了前置地址，客户端就不得不重新打
包发布。
fens地址就很好的解决了RegisterFront的缺点。用户只要接入
fens前置，fens会根据用户所在的中心号自动注册对应中心的地址列
表；当发生中心切换时，用户会自动转到另一个中心的前置上，迅
速恢复交易，整个过程无需干预。
同样的，对于客户端的打包发布，只需配置fens地址，即便期
货公司新增或修改了前置地址，期货公司也不需要通知客户端，客
户端也不需要重新打包发布。
另外，fens还能根据用户所在的网段，自定义返回的地址列
表。
下文将详细介绍fens的一些配置和使用方法。
回到顶部


◇ 2.fens连接规则
下图是fens.xml示例，该配置在期货公司端：
投资者在调用接口RegisterFensUserInfo时需要选择LoginMode参
数。
若选择THOST_FTDC_LM_Trade，且投资者是主中心用户，则
会连接到sysid="1"的这组地址；
若选择THOST_FTDC_LM_Trade，且投资者是备中心用户，则
会连接到sysid="2"的这组地址；
若选择THOST_FTDC_LM_Transfer，且投资者是主中心用户，
则会连接到sysid="1"的这组地址；
若选择THOST_FTDC_LM_Transfer，且投资者是备中心用户，
则会连接到sysid="1"的这组地址。
回到顶部


◇ 3.参数说明
THOST_FTDC_LM_Trade模式下，fens会根据用户所在中心，
自动切换。
THOST_FTDC_LM_Transfer模式下，fens只连接主中心前置

---



... 更多内容请参考 PDF 原文 ...


