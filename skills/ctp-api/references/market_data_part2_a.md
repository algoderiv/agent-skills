# Market Data - CTP 6.7.8 API

本节包含 16 个主题。


## 协议方式接收二代组播行情

协议方式接收二代组播行情
本文介绍协议方式下如何通过mdfront接收二代组播行情。


◇ 1.术语介绍
1. 协议方式：指不通过CTP封装的API，而是自行通过二代
行情协议规范解码数据报文，从而获取实时行情的方式。
2. 二代组播行情（下文简称二代行情）：交易所以组播方式
提供的实时五档行情。因为是组播，所以接收端必须在内
部网络并且要加入组播组。
3. mdfront：CTP交易系统中一个接收交易所二代组播行情
的组件。该组件提供查询组播合约接口，可通过API函数
ReqQryMulticastInstrument获取。
回到顶部


◇ 2.说明
交易所不允许投资者直接交易所报盘网去接收二代行情，但期
货公司可以将二代行情转发出来给投资者使用。
二代行情包含了快照查询和增量行情实时推送功能，其中，增
量行情仅包含了变动值，例如价格变动多少、成交量变动多少，只
有通过快照+增量的方式才能得到一笔完整的行。而期货公司只能
转发增量行情，不能转发快照查询。所以投资者除了接收增量行情
外，还需要想办法获取行情快照。
行情快照建议通过mdfront获取，只需订阅行情即可。订阅方法
参见行情接口。
最后，增量行情还有个问题，即报文里的合约是以数字编号命
名，例如编号100。而CTP的行情都是合约名称，例如rb2002，为了
解决合约编号到合约名称的映射关系，mdfront提供了
ReqQryMulticastInstrument查询接口，用户可以通过mdapi的
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
THOST_FTDC_LM_Transfer模式下，fens只连接主中心前置。
回到顶部


◇ 4.代码示例
交易fens使用方法：
CThostFtdcTraderApi *pUserApi = 
CThostFtdcTraderApi::CreateFtdcTraderApi("F:\\flow\\");
CSimpleHandler sh(pUserApi);
pUserApi->RegisterSpi(&sh);
printf(pUserApi->GetApiVersion());
pUserApi->SubscribePrivateTopic(THOST_TERT_QUICK);
pUserApi->SubscribePublicTopic(THOST_TERT_QUICK);
CThostFtdcFensUserInfoField pFensUserInfo = { 0 };
strcpy_s(pFensUserInfo.BrokerID, "9999");
strcpy_s(pFensUserInfo.UserID, "1000001");
pFensUserInfo.LoginMode = THOST_FTDC_LM_Trade;
pUserApi->RegisterFensUserInfo(&pFensUserInfo);
pUserApi-> RegisterNameServer (“tcp://127.0.0.1:41205”);
pUserApi->Init();
行情fens使用方法：
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
< 前页 回目录 后页 >




---

## 各交易所行情区别

各交易所行情区别
目前，各交易所提供的行情存在一定的差异，主要体现在字段数
量、字段计算方法、字段含义上，甚至行情每秒tick数都不尽相同。
CTP对交易所的行情只做转发，但对于有些交易所不发的字段，会做
一定程度的补充，例如郑商所的成交金额，交易所不发这个字段，但
该字段对于行情又十分重要，因此CTP会计算并补填；对于交易所有
值的字段，CTP不会对其做更改，只做转发。
本文针对CTP给出的各所行情，选取一些重要差异，逐一说明，
以帮助用户更快上手，降低开发成本。交易所行情本身的差异不在本
文讨论范围。如需了解行情的订阅方法，请参阅行情接口。


◇ 1.字段差异
ExchangeID（交易所ID）
期货普通前置不推送exchangeid； 期货mdfront推送exchangeid
；个股的普通前置推送exchangeid
Tradingday（交易日）
郑商所行情的Tradingday是自然日，其他交易所的Tradingday都
是交易日。
TrunOver（成交金额）
郑商所行情中，不推送成交金额，CTP会自行计算。对于普通
行情前置front，后台版本6.7.1之前成交金额=成交均价*成交数量；
后台版本6.7.1及之后成交金额=成交均价*成交数量*合约乘数。对
于组播行情前置mdfront，仍保持成交金额=成交均价*成交数量。
（交易所官网公布的成交金额=成交均价*成交数量*合约乘数）
AveragePrice（成交均价）
除了郑商所外，其他交易所都不推送成交均价，CTP的TCP行
情会自行计算并填补，成交均价=成交金额/成交数量。（交易所官
网公布的成交均价=成交金额/成交数量/合约乘数）
回到顶部


◇ 2.每秒tick数
郑商所的行情从六期开始仅支持2Tick/秒，不再支持4Tick/秒
其他交易所的行情只能是2Tick/秒，即500毫秒推一次，一秒两
次。
回到顶部


◇ 3.组合合约行情
只有大商所和郑商所有组合合约行情，其他交易所没有组合合
约行情。
组合行情仅有少数字段有值，例如买卖价量、涨跌停板；大部
分字段都没有值，例如最新价、成交金额、成交量等。
特别提醒：部分终端的组合行情有最新价，是因为终端自己
计算了最新价。并非行情订阅的有问题。
回到顶部


◇ 4.UDP行情和TCP行情区别
UDP行情只做纯转发，因此交易所不发的字段，在UDP行情里
就保持为空转发出去。
TCP行情会补填一些字段。比如郑商所的Tradingday字段和
AveragePrice字段，交易所行情没有这个字段，因此是空的，但是
TCP行情会去补填。
UDP行情和TCP行情的订阅方法，请参阅CreateFtdcMdApi
回到顶部
< 前页 回目录 后页 >


合约状态变化说明
合约状态变化表示的是交易所各合约的交易阶段变化，用户在登
录CTP后，无需订阅便能收到全量的合约状态变化（对应接口
OnRtnInstrumentStatus），该变化在盘中实时推送。其中合约状态推送
到产品级别，期权也是推送到产品级别的。CTP服务器预埋单便是通
过判断合约是否进入集合竞价报单阶段或连续交易阶段来触发的。本
文整理了各交易所的合约状态变化。


◇ 1.各交易所合约状态变化
合约交易状态：
开盘前：0
非交易：1
连续交易：2
集合竞价报单：3
集合竞价价格平衡：4
集合竞价撮合：5
收盘：6
交易业务处理：7


◇ 1.1.SHFE合约状态
推送方式：交易所推送合约级别，ctp推送产品级别
合约开盘前20:55 20:59 21:00 23:00 1:00 2:30 开
ag
1
3
5
2
1
al
1
3
5
2
1
ao
1
3
5
2
1
cu
1
3
5
2
1
ni
1
3
5
2
1
pb
1
3
5
2
1
zn
1
3
5
2
1
sn
1
3
5
2
1
ss
1
3
5
2
1
au
1
3
5
2
1
fu
1
3
5
2
1
hc
1
3
5
2
1
bu
1
3
5
2
1
rb
1
3
5
2
1
ru
1
3
5
2
1
sp
1
3
5
2
1
wr


◇ 1.2.INE合约状态
推送方式：交易所推送合约级别，ctp推送产品级别
合约开盘前20:55 20:59 21:00 23:00 1:00 2:30 8
bc
1
3
5
2
1
3
lu
1
3
5
2
1
3
nr
1
3
5
2
1
3
sc
1
3
5
2
1
3


◇ 1.3.CZCE合约状态
推送方式：交易所推送产品级别，ctp推送产品级别（郑商所
合约状态基于5期环境整理）
产品20:55 20:59 21:00 23:00 8:55 8:59 9:00 10
CF
3
0
2
0
2
0
CY
3
0
2
0
2
0
FG
3
0
2
0
2
0
MA
3
0
2
0
2
0
ME
3
0
2
0
2
0
OI
3
0
2
0
2
0
PF
3
0
2
0
2
0
RM
3
0
2
0
2
0
SA
3
0
2
0
2
0
SR
3
0
2
0
2
0
TA
3
0
2
0
2
0
TC
3
0
2
0
2
0
ZC
3
0
2
0
2
0
CFC
3
0
2
0
2
0
CFP
3
0
2
0
2
0
MAC 3
0
2
0
2
0
MAP 3
0
2
0
2
0
OIC
3
0
2
0
2
0
OIP
3
0
2
0
2
0
RMC 3
0
2
0
2
0


RMP 3
0
2
0
2
0
SRC
3
0
2
0
2
0
SRP
3
0
2
0
2
0
TAC
3
0
2
0
2
0
TAP
3
0
2
0
2
0
ZCC
3
0
2
0
2
0
ZCP
3
0
2
0
2
0
AP
3
0
2
0
CJ
3
0
2
0
ER
3
0
2
0
JR
3
0
2
0
LR
3
0
2
0
PK
3
0
2
0
PM
3
0
2
0
RI
3
0
2
0
RO
3
0
2
0
RS
3
0
2
0
SF
3
0
2
0
SM
3
0
2
0
UR
3
0
2
0
WH
3
0
2
0
WS
3
0
2
0
WT
3
0
2
0
PKC
3
0
2
0
PKP
3
0
2
0


◇ 1.4.DCE合约状态
推送方式：交易所推送产品级别，ctp推送产品级别
产品20:55 20:59 21:00 23:00 8:55 8:59 9:00 10:
a
3
5
2
1
3
5
2
1
a_o
3
5
2
1
3
5
2
1
b
3
5
2
1
3
5
2
1
b_o
3
5
2
1
3
5
2
1
c
3
5
2
1
3
5
2
1
c_o
3
5
2
1
3
5
2
1
cs
3
5
2
1
3
5
2
1
eb
3
5
2
1
3
5
2
1
eb_o 3
5
2
1
3
5
2
1
eg
3
5
2
1
3
5
2
1
eg_o 3
5
2
1
3
5
2
1
i
3
5
2
1
3
5
2
1
i_o
3
5
2
1
3
5
2
1
j
3
5
2
1
3
5
2
1
jm
3
5
2
1
3
5
2
1
l
3
5
2
1
3
5
2
1
l_o
3
5
2
1
3
5
2
1
m
3
5
2
1
3
5
2
1
m_o 3
5
2
1
3
5
2
1
p
3
5
2
1
3
5
2
1
p_o
3
5
2
1
3
5
2
1


pg
3
5
2
1
3
5
2
1
pg_o 3
5
2
1
3
5
2
1
pp
3
5
2
1
3
5
2
1
pp_o 3
5
2
1
3
5
2
1
rr
3
5
2
1
3
5
2
1
v
3
5
2
1
3
5
2
1
v_o
3
5
2
1
3
5
2
1
y
3
5
2
1
3
5
2
1
y_o
3
5
2
1
3
5
2
1
bb
3
5
2
1
fb
3
5
2
1
jd
3
5
2
1
lh
3
5
2
1


◇ 1.5.CFFEX合约状态
推送方式：交易所推送合约级别，ctp推送产品级别
产品
开盘前9:25 9:29 9:30 11:30 13:00 14:57
IC
0
3
5
2
1
2
IF
0
3
5
2
1
2
IH
0
3
5
2
1
2
IM
0
3
5
2
1
2
TL
0
3
5
2
1
2
T
0
3
5
2
1
2
TF
0
3
5
2
1
2
TS
0
3
5
2
1
2
IO期权
0
3
5
2
1
2
3
MO期权0
3
5
2
1
2
3
HO期权0
3
5
2
1
2
3


◇ 1.6.GFEX合约状态
推送方式：交易所推送产品级别，ctp推送产品级别
产品8:55 8:59 9:00 10:15 10:30 11:30 13:30 15:
si
3
5
2
1
2
1
2
6
si_o
3
5
2
1
2
1
2
6
回到顶部


◇ 2.关于EnterTime字段
EnterTime表示的是进入本状态时间，该时间指的是交易所时
间；但郑商所例外，因为郑商所的合约状态变化是通过交易所的行
情报盘推送的，其中没有时间戳，因此CTP会打上排队机时间戳，
也就是说郑商所的合约状态变化报文中的EnterTime表示的是CTP排
队机的时间。
回到顶部
< 前页 回目录 后页 >


报单流控、查询流控和会话数控制
CTP交易系统基于安全和性能考虑，在诸多地方有流量控制，其
中流量控制又分FTD报文流量控制、报单流量控制、查询流量控制
等。而这些流量控制分布在各个不同的地方。此处将会给大家详细介
绍。


◇ 1.报单流控
报单流控是指用户在本交易系统报单（ReqOrderInsert）、撤单
（ReqOrderAction)时每秒内允许的最大笔数。
报单流控限制配置在CTP柜台端【程序化交易频繁报撤单管
理】菜单。
如果超过这个限制API会通过OnRspOrderAction提示：“CTP:下
单频率限制”。
回到顶部


◇ 2.查询流控
查询流控是指用户当前Session在做查询的时候每秒内允许的最
大请求笔数。投资者受流控限制，操作员不受流控限制。
查询流控配置在交易前置组件上，配置项为【QryFreq】。其中
API内置了在途查询流控1笔。
自从穿透式监管版本以后，API在连接交易前置时，会去查询
到前置的查询流控设置（该设置配置在front_se组件）。假设前置配
置了2笔/秒，那么连接该前置的API每秒只能发起2笔查询请求。
但是要注意，不管怎么配置，API都内置了在途流控，在途查
询流控为1笔。即，当前这笔查询请求发出后，在未收完所有的查询
响应前，不能发起下一笔查询请求。
在过去，查询流控是内置在API里，1笔每秒，在途1笔。
如果超过交易前置配置的查询流控，则会触发OnRspError，并
提示：“CTP：查询未就绪，请稍后重试”
如果超过API内置的在途流控，则查询请求的返回值为-2，表示
未处理请求超过许可数。
所有ReqQuery开头查询函数不受流控限制，原因是此类函数
都是通过交易核心处理的，不通过查询核心。
回到顶部


◇ 3.FTD报文流控
FTD报文流控是指用户当前Session在提交API指令的时候每秒
内允许的最大请求笔数。
FTD报文流控配置在交易前置组件上，配置项为
【FTDMaxCommFlux】。
FTD报文流控是一种综合性的流控手段，API的所有接口在跟前
置交互的时候都是使用FTD协议的报文进行通讯的，因此这就意味
着，登录、查询、报单、撤单和报价等等所有请求都在FTD报文流
控的限制范围内。
例如如果配置为6，可以简单理解为用户仅能调用6次API请求
指令，包括登录、查询、报单撤单等等。
如果超过FTD报文流控，则超过的指令会被缓存到前置，直到
下一秒发出。例如用户在一秒内报单10笔，则前6笔会立即发送给核
心，而后4笔则会被缓存在前置，到下一秒才发出。因此对于用户程
序端的感受就是后4笔报单延迟很大，实则是受到了流控。
注意，FTD报文流控不会有错误信息或错误返回。
回到顶部


◇ 4.前置连接数流控
前置连接数流控是指在本交易前置对同一IP每秒允许的最大API
连接请求数。
前置连接数流控配置在交易前置组件上，配置项为
【ConnectFreq】。如果不设置就表示不限制流控。
需要注意的是，API连接请求指的是API从init到
OnFrontConnected的过程，跟登录无关。可以简单理解为一次init就
是一个连接请求。
例如如果配置为20，则一秒内最多有20个session建立跟前置的
连接。
如果超过前置连接数流控则会被主动断开连接，触发
OnFrontDisconnected。因此用户如果发现自己的程序一连接前置就
被断开，则除了版本问题外，有可能是遇到了连接数流控。
想要指定ip不受流控限制的话，可以通过在front_se的bin目录
下面新建一个文件名为whiteiplist的文件，每个ip单独一行。
回到顶部


◇ 5.同一用户最大允许在线会话数
同一用户最大允许在线会话数是指同一个用户（UserID）在本
交易系统中同时登录在线的最大允许会话数。
同一用户最大允许在线会话数配置在柜台或交易核心中。
注意，这个会话数针对的是本交易系统，而非单一前置。
如果超过同一用户最大允许在线会话，则会通过
OnRspUserLogin返回“CTP:用户在线会话超出上限”的错误。
后台交易系统版本为6.6.3及以上，新增分用户会话数设置，支
持按照用户维度设置最大会话数功能。
回到顶部


◇ 6.交易所API流控
交易所API流控指通过交易所API发送报单等请求的每秒最大允
许数。
交易所API流控的阈值设置在交易所端，由交易所API查询获
取，该流控实际控制在交易所API端。
受到交易所流控后会触发OnRtnOrder，报“CTP：交易所每秒发
送请求数超过许可数”或者“CTP:交易所未处理请求超过许可数”。
回到顶部
< 前页 回目录 后页 >


通讯模式
交易员API使用建立在TCP协议之上FTD协议与交易托管系统进行
通讯，交易托管系统负责投资者的交易业务处理。


◇ 1.通讯模式
FTD 协议中的所有通讯都基于某个通讯模式。通讯模式实际上
就是通讯双方协同工作的方式。
FTD涉及的通讯模式共有三种：
对话通讯模式
对话通讯模式是指由会员端主动发起的通讯请求。该请求被交
易所端接收和处理，并给予响应。例如报单、查询等。这种通讯模
式与普通的客户/服务器模式相同。
私有通讯模式
私有通讯模式是指交易所端主动，向某个特定的会员发出的信
息。例如成交回报等。
广播通讯模式（公有流）
广播通讯模式又称公有流，是指交易所端主动，向市场中的所
有会员都发出相同的信息。例如公告、市场公共信息等。
通讯模式和网络的连接不一定存在简单的一对一的关系。也就
是说，一个网络连接中可能传送多种不同通讯模式的报文，一种通
讯模式的报文也可以在多个不同的连接中传送。
无论哪种通讯模式，其通讯过程都如下图所示
回到顶部


◇ 2.数据流
交易托管系统支持对话通讯模式、私有通讯模式、广播通讯模
式：
对话通讯模式下支持对话数据流和查询数据流：
对话数据流是一个双向数据流，交易托管系统发送交易请求，
交易系统反馈应答。交易系统不维护对话流的状态。系统故障时，
对话数据流会重置，通讯途中的数据可能会丢失。
查询数据流是一个双向数据流，交易托管系统发送查询请求，
交易系统反馈应答。交易系统不维护查询流的状态。系统故障时，
查询数据流会重置，通讯途中的数据可能会丢失。
私有通讯模式下支持私有数据流：
私有流是一个单向数据流，由交易系统发向交易托管系统，用
于传送交易员私有的通知和回报信息。私有流是一个可靠的数据
流，交易系统维护每个交易托管系统的私有流，在一个交易日内，
交易托管系统断线后恢复连接时，可以请求交易系统发送指定序号
之后的私有流数据。私有数据流向交易托管系统提供报单状态报
告、成交回报更等信息。
广播通讯模式下支持公共数据流：
公共数据流是一个单向数据流，由交易系统发向交易托管系
统，用于发送市场公共信息；公共数据流也是一个可靠的数据流，
交易系统维护整个系统的公共数据流，在一个交易日内，交易托管
系统断线恢复连接时，可以请求交易系统发送指定序号之后的公共
数据流数据。


回到顶部


◇ 3.业务与接口对照
业务类
型
业务
请求接口
响应接口
数据流
登录
登录
CThostFtdcTraderApi::
ReqUserLogin
CThostFtdcTraderSpi::
OnRspUserLogin
对话流
登出
CThostFtdcTraderApi::
ReqUserLogout
CThostFtdcTraderSpi::
OnRspUserLogout
对话流
修改用户口令
CThostFtdcTraderApi::
ReqUserPasswordUpdate
CThostFtdcTraderSpi::
OnRspUserPasswordUpdate
对话流
交易
报单录入
CThostFtdcTraderApi::
ReqOrderInsert
CThostFtdcTraderSpi::
OnRspOrderInsert
对话流
报单操作
CThostFtdcTraderApi::
ReqOrderAction
CThostFtdcTraderSpi::
OnRspOrderAction
对话流
报价录入
CThostFtdcTraderApi::
ReqQuoteInsert
CThostFtdcTraderSpi::
OnRspQuoteInsert
对话流
报价操作
CThostFtdcTraderApi::
ReqQuoteAction
CThostFtdcTraderSpi::
OnRspQuoteAction
对话流
私有回
报
成交回报
N/A
CThostFtdcTraderSpi::
OnRtnTrade
私有流
报单回报
N/A
CThostFtdcTraderSpi::
OnRtnOrder
私有流
报单录入错误回
报
N/A
CThostFtdcTraderSpi::
OnErrRtnOrderInsert
私有流
报单操作错误回
报
N/A
CThostFtdcTraderSpi::
OnErrRtnOrderAction
私有流


查询
报单查询
CThostFtdcTraderApi::
ReqQryOrder
CThostFtdcTraderSpi::
OnRspQryOrder
查询流
成交查询
CThostFtdcTraderApi::
ReqQryTrade
CThostFtdcTraderSpi::
OnRspQryTrade
查询流
投资者查询
CThostFtdcTraderApi::
ReqQryInvestor
CThostFtdcTraderSpi::
OnRspQryInvestor
查询流
投资者持仓查询
CThostFtdcTraderApi::
ReqQryInvestorPosition
CThostFtdcTraderSpi::
OnRspQryInv

---

## 行情流控

行情流控
CTP交易系统基于安全和性能考虑，在行情中新增登录和流控限
制，分别对应的配置项和api回调中的表现各不相同。此处将会给大家
详细介绍。


◇ 1.行情登录验证
配置在行情前置组件上，配置项为
LoginVerify=yes
开启后登录行情前置时要求当前交易日UserID+IP 已成功登录
过交易系统。开启此功能会影响部分客户端，需要改 造适配：先登
录交易后登行情。
API报错“CTP: 不合法的登录”
回到顶部


◇ 2.单session登录频率限制
配置在行情前置组件上，配置项为
LoginFreq=1
如上配置表示每个session每秒可以登录的频率为一秒一次，默
认为该值
配置为0表示登录频率不限制
API报错“CTP: api login over limit freq”
回到顶部


◇ 3.给各个session发送缓存包限制
配置在行情前置组件上，配置项为
SendingListSize
前置给每个session发送数据的缓存包上限。缓存包超出上限则
主动断开会话。
该配置项如不设置，默认该值为10000。如设置，则实际请求包
的缓存上限为max（10000，设置的值）
回到顶部


◇ 4.行情前置连接数流控
前置连接数流控是指在本行情前置对同一IP每秒允许的最大API
连接请求数。
前置连接数流控配置在行情前置组件上，配置项为
【ConnectFreq】。如果不设置就表示不限制流控。
需要注意的是，API连接请求指的是API从init到
OnFrontConnected的过程，跟登录无关。可以简单理解为一次init就
是一个连接请求。
例如如果配置为20，则一秒内最多有20个session建立跟前置的
连接。
如果超过前置连接数流控则会被主动断开连接，触发
OnFrontDisconnected。因此用户如果发现自己的程序一连接前置就
被断开，则除了版本问题外，有可能是遇到了连接数流控。
想要指定ip不受流控限制的话，可以通过在front_md_se的bin
目录下面新建一个文件名为whiteiplist的文件，每个ip单独一行。
回到顶部


◇ 5.行情前置IP最大session连接数限制、最大
可订阅合约数限制
ctp系统版本自6.7.2P6开始，行情前置新增配置项。
每个IP上活跃的session个数限制，默认为0不限制
MaxIPSession=10
每个session上订阅合约个数限制，默认为0不限制
MaxSubInstCnt=0
默认不配置，即不限制。
若超 MaxIPSession 限制后，会断开新建 session
若超 MaxSubInstCnt 限制后，会给 API 返回订阅失败，失败原
因 6000，原因值“CTP:sub too many insts”
想要指定ip不受以上限制，可以通过在front_md_se的bin目录
下面新建一个文件名为whiteiplist的文件，每个ip单独一行。
回到顶部
< 前页 回目录 后页 >


TGate网关接入指南


◇ 1.TGate简介
TGate(CTP交易网关)是CTP fens组件的全新换代，相较于fens有
着更轻量、客户端适配成本低、易维护、易使用等优点。用户可以
通过查询的方式实时获取其所在中心可接入的服务地址信息，包括
交易、行情、银期转账等服务地址，灵活设计策略选择地址。即使
盘中切换了交易中心，客户端可自动快速获取新的地址接入。
回到顶部


◇ 2.TGate接入场景
TGate连接CTP获取全量各中心前置地址列表以及用户与交易中
心对应关系。配套提供的TGateAPi为终端实现统一接入管理提供完
整的解决方案。用户终端通过TGateAPi连接TGate发起查询前置地
址请求，无需登录，TGate根据客户请求返回所在交易中心全部可接
入的服务地址。
无论客户是在CTP次中心，还是在异构分中心做交易，只需将
其可连接的前置地址维护在CTP柜台，终端通过TGate获取地址列
表，不必再做过滤即可接入所在中心的交易系统。即使盘中发生交
易中心切换，对于终端也只需重新获取一次地址列表，重新连接即
可，省略繁琐的站点切换。
回到顶部


◇ 3.接口文件列表
文件名
详情
TGateFtdcApi.h
交易接口C++头文件包含交易相关的指
令。
TGateFtdcApiStruct.h
包含了所有用到的数据结构的头文件。
TGateFtdcApiDataType.h 包含了所有用到的数据类型的头文件。
tgateapi.dll
tgate的动态链接库和静态链接库。
tgateapi.lib
error.dtd
包含所有可能的错误信息。
error.xml
FTD_tgateapi.xml
FTD协议中用到的各种包和域
回到顶部


◇ 4.TGateApi使用介绍
新增TGateAPI实现终端与交易网关的接入与通讯。
TGateAPI提供了两个接口，分别为CTGateFtdcApi和
