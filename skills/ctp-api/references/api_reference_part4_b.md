询价响应有以下几种：
函数名称
说明
OnRspForQuoteInsert
只在录入错误的情况下返回，如字段校
验失败、被限制询价等；可以通过
ErrorID和ErrorMsg查看错误信息
OnRtnForQuoteRsp（交
易spi）
暂时不用
OnRtnForQuoteRsp（行
情spi）
录入成功后会推送，但是事先要调用行
情api里的SubscribeForQuoteRsp去订阅该
消息。
OnErrRtnForQuoteInsert
被交易所拒绝的询价，通过此接口返
回；可以通过ErrorID和ErrorMsg查看错
误信息
由于中金所、上期所和大商所的询价是来自交易所交易流，
而郑商所是来自交易所行情流，所以连接mdfront只能看到郑商所
的询价通知，连接行情front能收到所有交易所的询价通知。
3.查询询价


查询询价使用ReqQryForQuote函数。
接收查询结果使用OnRspQryForQuote函数
4.请求报价指令
请求报价指令使用ReqQuoteInsert函数。
上期所期货合约使用ReqOrderInsert报价
参数CThostFtdcInputQuoteField中，QuoteRef、AskOrderRef、
BidOrderRef非必填，如果手工填则要求AskOrderRef小于
BidOrderRef，否则会报“CTP：重复的报单”。
5.接收报价响应
报价响应有以下几种：
函数名称
说明
OnRspQuoteInsert
只在录入错误的情况下返回，如字段校验失
败、无做市商权限等；可以通过ErrorID和
ErrorMsg查看错误信息
OnRtnQuote
当报价录入成功后，CTP给出该报价响应
OnRtnOrder
报价录入成功后，如果是双边报价，CTP还会同
时衍生出买卖两笔报价衍生单报入交易所，并
返回相应的OnRtnOrder
OnRtnTrade
衍生单成交后，返回该成交回报
对于大商所，交易所在接受报价后，将不再维护报价状态的更
新，所以客户端在接收CTP的报价回报时，其报价状态没有完整生
命周期，因此采用CTP接口的开发人员需要重点关注报价对应衍生
报单的状态。上期所会维护报价的状态，但是跟报单状态独立，比
如，如果两腿报单都已经成交，依然可以撤报价单，并且能成功。


中金所的报价和撤销报价的时候，交易所会发送OnRtnQuote，会更
新为已撤销。但是如果单独撤销两个衍生单，中金所不再更新报价
的状态。
详见报价回调规则
6.撤销报价请求
撤销报价请求使用ReqQuoteAction函数。
参数CThostFtdcInputQuoteActionField中，使用
QuoteRef+SessionID+FrontID组合来撤单。
7.撤销报价衍生单
撤销报价衍生单使用ReqOrderAction函数。
参数CThostFtdcInputOrderActionField中，使用
FrontID+SessionID+OrderRef组合来撤单。
要注意的是，如果在报价的时候没有填写AskOrderRef和
BidOrderRef，那么报价衍生单的响应OnRtnOrder里的FrontID和
SessionID是0。
8.查询报价
查询报价使用ReqQryQuote函数。
接收查询结果使用OnRspQryQuote函数
回到顶部


◇ 2.四所区别
下面列出四所在询价和报价上的一些区别。注意：以下内容仅
供参考，如果交易所的规则发生变化，文档并不会实时更新。
1.四所询价对比
　
中金所
大商所
郑商所
上期所
询价间隔如何设置
交易所限
制
交易所限
制
CTP柜
台
交易所限
制
普通投资者是否可接收询
价申报
可以
可以
可以
可以
2.四所报价对比
　
中金所
大商所
郑商所
上期所
是否
支持
平仓
支持
支持
支持
支持
是否
必须
关联
询价
询价通知没
有询价编
号，不用填
询价通知有
编号，但是
非必填，如
做市商要完
成义务率则
要填
询价通知有编
号，但是非必
填，如做市商要
完成义务率则要
填
询价通知没
有询价编
号，不用填
是否
支持
单边
报价
不支持，提
示“CTP:报单
字段有误”
支持，数量
填0
不支持，提
示“CTP:报单字
段有误”
不支持，提
示“CTP:报单
字段有误”


是否
支持
市价
单
不支持，交
易所提
示：“已撤单
报单被拒绝
CFFEX:价格
跌破跌停板”
不支持，交
易所提示：
已撤单报单
被拒绝DCE:
期权价格必
须大于等于
TICK!
不支持，交易所
提示：“已撤单报
单被拒绝CZCE:
出错: 限价单价
格不能为0或负
数”
不支持
双边
报价
买卖
数量
可否
不一
致
可以不一致
可以不一致
CTP:不可以，提
示“CTP：报单字
段有误”
CTP:不可
以，提
示“CTP：报
单字段有误”
双边
报价
卖价
是否
必须
大于
买价
一定是低价
买，高价
卖。否则会
形成自成交
必须，否则
交易所提
示： 已撤单
报单被拒绝
DCE:双边报
价指令卖价
必须大于买
价
必须，否则交易
所提示：已撤单
报单被拒绝
CZCE: 出错：卖
价(40800)必需大
于买价(94800)
一定是低价
买，高价
卖。否则会
形成自成交
是否
支持
套保
属性
支持
支持
不支持，提
示“CTP：该期权
合约只支持投机
类型报单”
支持
是否
会撤
后台版本在
6.6.1以上后
不会撤销 接
入大商所七
会撤销
会撤销


销前
一次
报价
支持自动撤
销
期之后支持
自动撤销
是否
支持
撤单
腿
支持顶单功
能，不支持
撤单边衍生
单
可以
不可以
不可以
双边
报价
开平
可否
不一
致
可以
可以
可以
可以
回到顶部


◇ 3.OrderType和TradeType字段说明
一笔报价会有两笔报价衍生单（OnRtnOrder），报价成交后还
会有成交回报（OnRtnTrade）。OnRtnOrder的OrderType和
OnRtnTrade的TradeType取自交易所回报。
上期所：交易所不返回TradeType和OrderType，所以在收到交
易所回报后，两个字段都是空值。
大商所：报价会转换成两笔普通报单出去，所以TradeType和
OrderType都是0（普通成交）
中金所：交易所返回OrderType为1（报价衍生单），TradeType
为0（普通成交）
郑商所：交易所返回OrderType为1（报价衍生单），TradeType
为0（普通成交）
回到顶部
< 前页 回目录 后页 >




---

## 报单回调规则

报单回调规则
本文例举了一些常用的报单操作所对应的回调规则。在给定交易
所报单回报顺序的前提下，CTP返回给API的回调顺序是固定的。
目前大商所的回报比较特殊，当一笔委托全部成交后，大商所直
接返回该笔委托的成交回报，而不返回状态为全部成交的报单回报，
对于这种情况，CTP会补一个状态为全部成交的报单回报，下文会举
例说明。


◇ 1.术语说明
报单回报
指报单的状态回报，有未知单报单回报、未成交报单回报、部
分成交报单回报、全部成交报单回报和撤单回报。对应回调函数
OnRtnOrder，以OrderStatus字段区分。
成交回报
指报单成交后推送的成交回报，对应回调函数OnRtnTrade。
错单响应
指报单被CTP拒绝后返回的响应通知，对应回调函数
OnRspOrderInsert，其中ErrorID和ErrorMsg指明了错误原因。
错单回报
指报单被CTP或交易所拒绝后的报单的状态回报。对应回调函
数OnErrRtnOrderInsert。
回到顶部


◇ 2.回调规则
测试场景1
报单合约ag1207，报入1手，到交易所先未成交，后全部成交1
手。
报单
ReqOrderInsert
回调顺序：
OnRtnOrder （未知单）
此时CTP接收到交易所的未成交报单回报
OnRtnOrder （未成交）
此时CTP接收到交易所的成交回报和全部成交报单回报
OnRtnOrder （未成交）
OnRtnOrder （全部成交）
OnRtnTrade
测试场景2
报单合约ag1207，报入1手，到交易所立即全部成交1手。（注
意，此案例没有收到交易所的未成交回报状态，而是直接收到全部
成交状态）
报单
ReqOrderInsert
OnRtnOrder （未知单）


此时CTP接收到交易所的成交回报和成交报单回报
OnRtnOrder （未知单）
OnRtnOrder （全部成交报单已提交）
OnRtnTrade
测试场景3
报单合约ag1207，报入1手，到交易所先未成交，然后撤单1
手。
报单
ReqOrderInsert
OnRtnOrder（未知单）
此时CTP接收到交易所的未成交报单回报
OnRtnOrder（未成交）
ReqOrderAction
此时CTP接收到交易所的撤单报单回报
OnRtnOrder（未成交）
OnRtnOrder（已撤单）
测试场景4
报单合约ag1207，报入10手，到交易所先未成交，然后成交3
手，然后剩余全部成交7手。
报单
ReqOrderInsert


OnRtnOrder （未知单）
此时CTP接收到交易所的未成交报单回报
OnRtnOrder （未成交）
此时CTP接收到交易所的成交回报和部分成交报单回报
OnRtnOrder （未成交）
OnRtnOrder （部分成交）
OnRtnTrade
此时CTP接收到交易所的成交回报和全部成交报单回报
OnRtnOrder （部分成交）
OnRtnOrder （全部成交）
OnRtnTrade
测试场景5
报单合约ag1207，报入10手，到交易所先未成交，然后成交4
手，然后剩余全部撤单6手。
报单
ReqOrderInsert
OnRtnOrder （未知单）
此时CTP接收到交易所的未成交报单回报
OnRtnOrder （未成交）
此时CTP接收到交易所的成交回报和部分成交报单回报
OnRtnOrder （未成交）
OnRtnOrder （部分成交）
OnRtnTrade


发起撤单
ReqOrderAction
此时CTP接收到交易所的撤单回报
OnRtnOrder（部分成交）
OnRtnOrder（已撤单）
测试场景6
对一笔已经撤单的委托单再次撤单。
对一笔已撤单再次撤单
ReqOrderAction
触发撤单错误响应
OnRspOrderAction （CTP:报单已全成交或已撤销，不能
再撤）
触发撤单错误回报
OnErrRtnOrderAction （CTP:报单已全成交或已撤销，不
能再撤）
测试场景7
对一笔未成交委托进行撤单，因为委托号填写错误导致撤单失
败
撤一笔不存在的委托
ReqOrderAction
触发撤单错误响应（CTP:撤单找不到相应报单）
OnRspOrderAction


触发撤单错误回报
OnErrRtnOrderAction （CTP:撤单找不到相应报单）
测试场景8
报入上期所FAK单子，报入后部分成交部分撤单。
报入上期所FAK
ReqOrderInsert
OnRtnOrder （未知单）
此时CTP收到了交易所推送的撤单状态的报单回报
OnRtnOrder （已撤单，回报中VolumeTraded字段此时已
有值）
此时CTP收到了交易所推送的成交回报
OnRtnOrder （已撤单）
OnRtnTrade
相同场景下，能源中心和中金所FAK的回报与上期所一致。
测试场景9
报入大商所FAK单子，报入后部分成交部分撤单
报入大商所FAK
ReqOrderInsert
OnRtnOrder （未知单）
此时CTP收到了交易所推送的未成交状态的报单回报
OnRtnOrder （未成交）


此时CTP收到了交易所推送的成交回报
OnRtnOrder （部分成交）
OnRtnTrade
此时CTP收到了交易所推送的撤单状态的报单回报
OnRtnOrder （已撤单）
相同场景下，广期所FAK的回报与大商所一致。
测试场景10
报入郑商所FAK单子，报入后部分成交部分撤单
报入郑商所FAK
ReqOrderInsert
OnRtnOrder （未知单）
此时CTP收到了交易所推送的未成交状态的报单回报
OnRtnOrder （未成交）
此时CTP收到了交易所推送的部分成交状态的报单回报
OnRtnOrder （未成交）
OnRtnOrder （部分成交）
OnRtnTrade
此时CTP收到了交易所推送的撤单状态的报单回报
OnRtnOrder （已撤单）
回到顶部


◇ 3.大商所特殊回调规则
目前大商所的回报比较特殊，当一笔委托全部成交后，大商所
直接返回该笔委托的成交回报，而不返回状态为全部成交的报单回
报，对于这种情况，CTP会补一个状态为全部成交的报单回报。
测试场景1
报单合约c1911-C-1760，报入1手，到交易所先未成交，后全部
成交1手。
报单
ReqOrderInsert
OnRtnOrder （未知单）
此时CTP接收到交易所的未成交报单回报
OnRtnOrder （未成交）
此时CTP接收到交易所的成交回报
OnRtnOrder （全部成交）（CTP补充生成，且不重复推
送前一状态）
OnRtnTrade
测试场景2
报单合约c1911-C-1760，报入1手，到交易所立即全部成交1
手。注意，大商所不管会不会立即成交，只要委托进入报单簿后都
会返回一笔未成交报单回报给到CTP。
报单


ReqOrderInsert
OnRtnOrder （未知单）
此时CTP接收到交易所的未成交报单回报
OnRtnOrder （未成交）
此时CTP接收到交易所的成交回报
OnRtnOrder （全部成交）（CTP补充生成，且不重复推
送前一状态）
OnRtnTrade
测试场景3
报单合约c1911-C-1760，报入1手，到交易所先未成交，然后撤
单1手。
报单
ReqOrderInsert
OnRtnOrder （未知单）
此时CTP接收到交易所的未成交报单回报
OnRtnOrder （未成交）
发起撤单
ReqOrderAction
此时CTP接收到交易所的撤单回报
OnRtnOrder （未成交）
OnRtnOrder （已撤单）
回到顶部


< 前页 回目录 后页 >




---

## 大商所组保

大商所组保
大商所期权上市后，为了更好的服务市场，大商所启动了期权六
期项目，主要从行权功能优化、期权套保、组合保证金优惠三方面完
善期权相关业务。
大商所目前支持的组合有：期货对锁，期货跨期，期货跨品种，
备兑买权，备兑卖权，期权跨式，期权宽跨式共7种，投保标志支持投
机-投机，套保-套保，投机-套保，套保-投机共4种。
本文主要就API针对组保业务的一些调整和特性做相关说明，具
体业务规则请参考交易所文档。


◇ 1.接口说明
录入请求：ReqCombActionInsert
错误响应：OnErrRtnCombActionInsert，
OnRspCombActionInsert
正确响应：OnRtnCombAction
注意，这些接口并非新增，而是大商所的组合业务复用了此
接口。
回到顶部


◇ 2.可申请组合的合约
组合和拆分的合约为大商所组合优惠表中所列的合约，但目前
CTP暂时不支持此优惠表的查询。在实际操作中，用户可通过
OnRspQryInstrument或OnRtnDepthMarketData里返回的组合合约来
填写。
目前的套利单合约（期货跨期和跨品种）基本都在大商所组
合优惠表中。但是有一种情况，就是新合约上市，套利合约也是
新合约的时候，当天是不包括在组合优惠表中的。
回到顶部


◇ 3.申请组合填写规则
合约（InstrumentID）：
针对各组合类型举例如下：
a) 期权跨式组合合约申请和拆分（STD m1905-c-2700&m1905-
p-2700）
b) 期权宽跨式组合合约申请和拆分（STG m1905-p-
2400&m1905-c-2700）
c) 备兑组合合约申请和拆分（PRT m1905-c-2400&m1905）
d) 期货跨期组合合约申请和拆分（SP a1903&a1905）
e) 期货跨品种组合合约申请和拆分（SPC c1903&cs1909）
f) 期货对锁组合合约申请和拆分（SP a1903& a1903）
g) 期权买入垂直价差组合（BLS m1809-P-3150&m1809-P-
3100）
h) 期权卖出垂直价差组合（BES m1809-P-3350&m1809-P-
3150）
i) 期权日历价差组合（CAS m1807-P-3150&m1809-P-3150）
j) 买入期权期货组合（BFO m1809-P-3150&m1809）
k) 期权对锁组合合约申请和拆分（OPL m1809-P-3150&m1809-
P-3150）
买卖方向（Direction）
以上面所列组合类型为例：
a、 b：方向为卖。这种组合类型需要左右两腿都是卖持仓，即
空头持仓；


c：方向为卖。因为目前交易所只支持备兑卖权，不支持备兑买
权，即支持左腿是卖持仓的组合，不支持左腿是买持仓的组合。
若右腿组合的是多头的期货合约，则称为“备兑看涨期权组合持
仓”；若右腿组合的是空头的期货合约，则称为“备兑看跌期权组合
持仓”；
d、e：当方向是买的时候就是左买持右卖持，当方向是卖的时
候就是左卖持右买持；
f：方向只支持买，所以是只有左买持右卖持。
g：买入垂直价差组合：当方向是买时就是左买持仓右卖持，若
为卖则会报错“CTP:找不到合约”
h：卖出垂直价差组合：当方向是卖时就是左卖持仓右买持，若
为买则会报错“CTP:找不到合约”
i：日历价差：
1）卖出较近月份的看涨期权合约，买入较远月份相同执行价格
的看涨期权合约，组合的买卖方向为卖
2）卖出较近月份的看跌期权合约，买入较远月份相同执行价格
的看跌期权合约，组合的买卖方向为卖
j：买入期权期货组合：组合买卖方向为买
1）买入看涨期权，同时卖出对应期货合约
2）买入看跌期权，同时买入对应期货合约
k：期权对锁组合：组合买卖方向为买
1）在同一期权品种同一系列同一合约上建立数量相等、方向相
反的头寸
组合方向（CombDirection）
组合方向支持‘申请组合’和‘拆分组合’。


投机套保类型（HedgeFlag）
投机套保类型支持‘投机’（指左右两腿都是投机），‘保值’（指
左右两腿都是套保），‘投套（指左腿投机右腿套保）’，‘套投（指
左腿套保右腿投机）’。
盘中的话，如果通过套利报单的话（ReqOrderInsert），是只
支持投机-投机，套保-套保；如果是通过申请组合形成组合单的
话，支持投机-投机，套保-套保，投机-套保，套保-投机。
交易所（ExchangeID）
该字段必填。
回到顶部


◇ 4.仓位计算规则
组合合约申请接受后，会将现有的两腿持仓组合到一起，形成
组合持仓。此时，在持仓汇总（OnRspQryInvestorPosition）里会显
示三条持仓记录，分别是新增的组合合约持仓，原多头合约持仓和
原空头合约持仓。而持仓明细（OnRspQryInvestorPositionDetail）里
不会新增组合合约记录。
1) 比如投资者在大商所买入1手“SP a0905&a0909”合约，形成的
持仓汇总如下：
合约
多头持仓
空头持仓
SP a0905&a0909
1
0
a0905
1
0
a0909
1
0
2) 比如投资者在大商所组合1手“STG c1909-P-1680&c1909-C-
2020”合约，形成的持仓汇总如下：
合约
多头持仓
空头持仓
STG c1909-P-1680&c1909-C-2020
0
1
c1909-P-1680
0
1
c1909-C-2020
0
1
回到顶部


◇ 5.平仓盈亏计算和保证金释放计算原则
平仓盈亏计算和保证金释放计算原则如下：
1）在计算平仓盈亏时，采用先开先平原则。
2）在计算保证金释放时，采用先平单腿持仓，后平优惠组合持
仓的原则，优惠组合按照优先级从低到高的顺序进行打破。
3）投资者持仓明细里面的TimeFirstVolume，是指大商所的持仓
按照先开先平的平仓顺序平仓后的剩余手数
4）投资者持仓明细里面的Volume如果是大商所的持仓则按照
先单一后组合的平仓顺序平仓后的剩余手数
5）投资者持仓接口里面的PositionCostOffset，因为目前大商所
平仓还是按照先单一后组合进行平仓，而计算平仓盈亏需要按照先
开先平进行计算，所以增加该字段用以计算按照先开先平的原则的
持仓盈亏。
综上所述，在通过持仓明细去计算持仓成本、持仓盈亏、平仓
盈亏时，需要用TimeFirstVolume；计算保证金时，需要用Volume。
通过持仓汇总去计算持仓成本和持仓盈亏规则如下：
后台版本6.7.1之前
大商所先开先平持仓成本 = PositionCost- PositionCostOffset
大商所持仓盈亏：
多头：PositionProfit = LastPrice*Position*VolumeMultiple - 
（PositionCost - PositionCostOffset）
空头：PositionProfit = （PositionCost - PositionCostOffset） - 
LastPrice*Position*VolumeMultiple 
后台版本6.7.1及之后，PositionCostOffset字段废弃


大商所先开先平持仓成本 = PositionCost
大商所持仓盈亏：
多头：PositionProfit = LastPrice*Position*VolumeMultiple - 
PositionCost
空头：PositionProfit = PositionCost - 
LastPrice*Position*VolumeMultiple 
回到顶部


◇ 6.代码示例
场景：现有c1909-P-1680和c1909-C-2020各一手的投机卖仓，申
请组合代码示例如下：
    CThostFtdcInputCombActionField a = { 0 };
    strcpy_s(a.BrokerID, “9999”);
    strcpy_s(a.InvestorID, “00001”);
    strcpy_s(a.InstrumentID, “STG c1909-P-1680&c1909-C-2020”);
    strcpy_s(a.CombActionRef, "1");
    strcpy_s(a.UserID, “00001”);
    a.Direction = THOST_FTDC_D_Sell;
    a.Volume = 1;
    a.CombDirection = THOST_FTDC_CMDR_Comb;
    a.HedgeFlag = THOST_FTDC_HF_Speculation;
    strcpy_s(a.ExchangeID, “DCE”);
    m_pUserApi->ReqCombActionInsert(&a, nRequestID++);
回到顶部


◇ 7.FAQ
查询合约的时候发现合约id不全
对于老版本api如果有查询组合合约，或者收到的组合
合约id超过了30个字符，超过的部分会被截断，行情中也
是如此。
回到顶部
< 前页 回目录 后页 >




---

## 报价回调规则

报价回调规则
本文例举了一些常用的报价操作所对应的回调规则。在给定交易
所报价回报顺序的前提下，CTP返回给API的回调顺序是固定的。
特别要注意，大商所只返回一次报价状态回报，例如一笔报价从
报入到未成交到最终成交，一共有三个状态，但是大商所只推送未成
交报价回报，全部成交后不推送成交报价回报。甚至在查询报价时，
其报价状态也仍然是未成交状态。用户在测试和实现自己业务的时候
要格外注意这点。
目前国内几家交易所的报价规则各不相同，本文不做详细例举，
希望大家在梳理规则时学会举一反三。


◇ 1.术语说明
报价衍生单
报价报入交易所时，同时会衍生出两笔买卖报单，以普通报单
的形式报入交易所。这两笔报单称之为报价衍生单。
报价回报
指报价的状态回报，有未知单报价回报、未成交报价回报、全
部成交报价回报。对应回调函数OnRtnQuote，以QuoteStatus字段区
分。
报单回报
指报单的状态回报，有未知单报单回报、未成交报单回报、部
分成交报单回报、全部成交报单回报和撤单回报。对应回调函数
OnRtnOrder，以OrderStatus字段区分。
成交回报
指报单成交后推送的成交回报，对应回调函数OnRtnTrade。
回到顶部


◇ 2.回调规则
测试场景1
CZCE做市商报价，报价合约SR911P4100，开仓，买报价100，
卖报价150，买数量1手，卖数量1手，先买成交1手，后又卖成交1
手。
报价
ReqQuoteInsert
OnRtnOrder（未知单-卖）
OnRtnOrder（未知单-买）
OnRtnQuote（未知单）
此时CTP接收到交易所未成交报价回报和报单回报。
OnRtnQuote（未成交）
OnRtnOrder（未成交-买）
OnRtnOrder（未成交-卖）
此时CTP接收到交易所买方向全部成交的成交回报和报单
回报。
OnRtnOrder（全部成交-买）
OnRtnTrade（买）
此时CTP接收到交易所卖方向的全部成交成交回报和报单
回报，以及全部成交报价回报。
OnRtnQuote（全部成交）
OnRtnOrder（全部成交-卖）
OnRtnTrade（卖）


注意：报价的衍生单（报价两条单腿报单）的回报顺序取决
于交易所返回的衍生单的回报顺序。例如测试场景1中，交易所先
推送买未成交报单回报后推送卖未成交回报，则CTP也会照此顺
序推送。成交回报同理，因此对于报价及衍生单的买卖回报顺序
是不固定的。
测试场景2
DCE做市商报价，报价合约m1911-C-2400，开仓，买报价
400，卖报价500，买数量1手，卖数量1手，买成交1手，卖成交1
手。
报价
ReqQuoteInsert
OnRtnOrder（未知单-卖）
OnRtnOrder（未知单-买）
OnRtnQuote（未知单）
此时CTP接收到交易所未成交报价回报和报单回报。
OnRtnQuote（未成交）
OnRtnOrder（未成交-卖）
OnRtnOrder（未成交-买）
此时CTP接收到交易所买方向全部成交的成交回报和报单
回报。
OnRtnOrder（全部成交-卖）
OnRtnTrade（卖）
此时CTP接收到交易所卖方向的全部成交成交回报和报单
回报。
OnRtnOrder（全部成交-买）


OnRtnTrade（买）
注意：DCE区别于其他交易所，其报价状态只推送一次，即
使后续状态发生变化，也不再推送。因此测试场景2相比场景1，
会少一笔状态为全部成交的报价回报。
测试场景3
CZCE做市商报价撤单，报价合约CF001C12200，开仓，买报价
2199，卖报价3369，买数量1手，卖数量1手，撤报价。
报价
ReqQuoteInsert
OnRtnOrder （未知单—卖）
OnRtnOrder （未知单—买）
OnRtnQuote （未知单）
此时CTP接收到交易所未成交报价回报和报单回报。
OnRtnOrder （未成交—买）
OnRtnOrder （未成交—卖）
OnRtnQuote （未成交）
此时撤报价
此时CTP接收到交易所已撤单报价回报和报单回报。
OnRtnQuote （已撤单）
OnRtnOrder （已撤单—卖）
OnRtnOrder （已撤单—买）
测试场景4


DCE做市商报价，撤单腿报价。报价合约c1909-C-1660，开
仓，买报价179，卖报价371，买数量1手，卖数量1手；撤买单腿1
手，撤卖单腿1手。
报价
ReqQuoteInsert
OnRtnOrder （未知单—卖）
OnRtnOrder （未知单—买）
OnRtnQuote （未知单）
此时CTP接收到交易所未成交报价回报和报单回报。
OnRtnQuote （未成交）
OnRtnOrder （未成交—卖）
OnRtnOrder （未成交—买）
此时撤单腿操作
OnRtnOrder （未成交—买）
OnRtnOrder （已撤单—买）
OnRtnOrder （未成交—卖）
OnRtnOrder （已撤单—卖）
测试场景5
CZCE做市商报价，撤单腿报价，报价合约CF001C12200，开
仓，买报价2199，卖报价3369，买数量1手，卖数量1手，撤买单腿1
手，撤卖单腿1手。
报价
ReqQuoteInsert


OnRtnOrder （未知单—卖）
OnRtnOrder （未知单—买）
OnRtnQuote （未知单）
此时CTP接收到交易所未成交报价回报和报单回报。
OnRtnQuote （未成交）
OnRtnOrder （未成交—买）
OnRtnOrder （未成交—卖）
此时撤买单腿
此时CTP接收到交易所买撤单报单回报和错单回报。
OnRtnOrder （未成交—买）
OnErrRtnOrderAction [CZCE:出错: 所撤委托单没有找到]
此时撤卖单腿
此时CTP接收到交易所买撤单报单回报和错单回报。
OnRtnOrder （未成交—卖）
OnErrRtnOrderAction [CZCE:出错: 所撤委托单没有找到]
郑商所和上期所报价只支持撤报价，不支持撤单腿，因此报
错。
测试场景6
CZCE做市商报价。报价合约CF001C13000，开仓，买报价
1700，卖报价2800，买数量1手，卖数量1手；再报价，报价合约仍
是CF001C13000，开仓。
第一次报价
ReqQuoteInsert
OnRtnOrder （未知单—卖）


OnRtnOrder （未知单—买）
OnRtnQuote （未知单）
此时CTP接收到交易所未成交报价回报和报单回报。
OnRtnQuote （未成交）
OnRtnOrder （未成交—买）
OnRtnOrder （未成交—卖）
第二次报价（郑商所的第二次报价会自动撤销第一次报
价）
ReqQuoteInsert
OnRtnOrder （未知单—卖）
OnRtnOrder （未知单—买）
OnRtnQuote （未知单）
此时CTP接收到交易所如下回报：第一次报价的已撤单报
价回报和报单回报，第二次报价的未成交报价回报和报单回
报。
OnRtnQuote （已撤销，第一次报价）
OnRtnOrder （已撤单—买，第一次报价）
OnRtnOrder （已撤单—卖，第一次报价）
OnRtnQuote （未成交，第二次报价）
OnRtnOrder （未成交—买，第二次报价）
OnRtnOrder （未成交—卖，第二次报价）
郑商所的第二次报价会自动撤销第一次报价。
测试场景7


DCE做市商报价，撤报价。报价合约m1707-C-2700，开仓，买
报价63，卖报价349，买数量1手，卖数量1手；撤报价1手。
报价
ReqQuoteInsert
OnRtnOrder （未知单—卖）
OnRtnOrder （未知单—买）
OnRtnQuote （未知单）
此时CTP接收到交易所未成交报价回报和报单回报。
OnRtnQuote （未成交）
OnRtnOrder （未成交—卖）
OnRtnOrder （未成交—买）
此时撤报价操作
OnRtnOrder （未成交—卖）
OnRtnOrder （未成交—买）
OnRtnQuote （未成交）
OnRtnOrder （已撤单—卖）
OnRtnOrder （已撤单—买）
大商所报价后，交易所推送报价响应（未成交），此时撤报
价，返回两腿的报单回报（已撤单），但是报价状态不会再推送
回到顶部
< 前页 回目录 后页 >


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
