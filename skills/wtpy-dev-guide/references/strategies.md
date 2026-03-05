# Wondertrader-Rtd - Strategies

**Pages:** 14

---

## 经典日内CTA策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/strategies/ctadaytradestra.html

**Contents:**
- 经典日内CTA策略
- 1.要实现的CTA策略简介
  - 1.1ATR策略
  - 1.2菲阿里四价策略
  - 1.3空中花园策略
- 2.策略实现
  - 2.1 ATR策略的实现
  - 2.2菲阿里四价策略的实现
  - 2.3空中花园策略的实现
- 3.策略回测

​ 经典的CTA策略，有DualThrust策略、R-breaker策略、ATR策略、菲阿里四价策略、空中花园策略等。前两个策略已经有其他wtpy相关的文章进行过介绍，本文实现后面三个策略。

​ ATR（Average True Range）又称平均真实波动范围，主要是用来衡量市场波动的程度，是显示市场变化率的指标。

ATR 平均真实波幅（ATR）的计算方法：

​ 1.当前交易日的最高价与最低价间的波幅；

​ 2.前一交易日收盘价与当个交易日最高价间的波幅；

​ 3.前一交易日收盘价与当个交易日最低价间的波幅。

今日振幅、今日最高与昨收差价和今日最低与昨收差价中的最大值为真实波幅，在有了 真实波幅后，就可以利用一段时间的平均值计算 ATR 了。至于用多久计算，不同的使 用者习惯不同，10 天、20 天乃至 65 天都有。求真实波幅的 N 日移动平均，N 一般取 14 天，计算式如下所示：

依据 ATR 的特性，可以设计通道突破策略，策略主要特点是：

日内 ATR 突破基于当根 K 线开盘价与过去 N 个周期的 ATR；

上轨＝当根 K 线开盘价 N 周期 ATR*M，下轨＝当根 K 线开盘价－N 周期 ATRxM；

ATR 除了进场和出场策略外，还有止损止盈策略。由于 ATR 计算的是在某一个时间段内价格的真实波动范围，因此可以把该范围作为是计算止损和止盈的标准。

​ 菲阿里四价策略是一种比较简单的趋势型日内交易策略。昨天高点、昨天低点、昨日收盘价、今天开盘价，可并称为菲阿里四价。

​ 菲阿里四价是日内突破策略，所以每日收盘之前都需要进行平仓。

​ 昨日高点和昨日低点可以视为近期的一个波动范围，该范围的存在一定程度是一种压力 线，只有足够的价格上涨或者下跌才会突破前期的高点或者低点。因此突破位置是一个比较好的入场信号，如果突破该波动范围，则证明动能较大，后续走势强度维持较强的概率比较高，因此该策略采用以下开仓方式:

策略在开仓之后可能面临假突破的问题，因为该价位存在很大的阻力，可能是暂时性的突破，随机回落，因此具体策略使用之中可以设置一些过滤条件来剔除假突破的情况。 这样使得策略的胜率变大。开仓之后的止损止盈根据具体环境具体确定。

​ 空中花园比较看重开盘突破。开盘时的高开或者低开均说明有大的利好或者利空使得开盘大幅远离昨天的收盘价。开盘突破，是最快的一种入场方式。当然出错的概率也最高。因此为了提高策略的胜率，空中花园策略加了额外的条件，也就是开盘要大幅高开或者低开，形成一个空窗，因此顾 名思义称为空中花园，然后再根据是否突破上下轨来进行开仓判断。这样一来，策略的胜率将大大提高，不过由于对高开或者低开的幅度要求过高，一般是超过1%，因此使得策略的交易次数可能相对其它策略而言要偏低一些。开盘第一根 K 线是收阳还是收阴，是判断日内趋势可能运动方向的标准。在当天开盘高开或低开时更有效。

​ 空中花园在当天高开或低开时使用，即当开盘价>=昨天收盘价*1.01 或开盘价<=昨天收盘价x0.99 时；

​ 实际上是一种当天大幅高开（>1%），搏高开低走；反之，大幅低开（<1%）,博低开高走。

​ 根据上文中对于ATR策略的基本信息描述，我们设置当前bar之前的n根bar的收盘价平均为中轨，中轨加上K1倍的ATR为上轨，中轨减去K2倍的ATR为下轨，价格突破上轨，开仓做多，当持有多头仓位且价格回落到中轨时，平多；价格跌穿下轨时，开仓做空，当持有做空仓位时且价格回升到中轨时，平空。另外，收盘前平仓。

​ 因为有日内交易策略收盘前平仓的要求，所以先介绍一下wtpy上实现收盘前平仓的方式：

策略还需要计算均线和ATR，从而计算得出上轨和下轨：

根据以上内容，ATR策略的大致内容已经完成得差不多了，完整代码放到文档最后，感兴趣的读者可以自己实现一下。

​ 根据上述对菲阿里策略的介绍，设定交易逻辑如下：

​ 1.当前价高于昨日最高价且高于开盘价，做多；

​ 2.当前价低于昨日最低价且低于开盘价，做空；

​ 作为日内策略，菲阿里四价策略同样也需要尾盘清仓，尾盘清仓的代码与上文中ATR策略中的完全相同。为了防止在交易边界策略频繁开仓平仓损失过大，设定每天只能开仓一次，下面说明一下在wtpy上如何实现每日开仓次数限制。

菲阿里策略总体交易逻辑还是比较简单，完整代码同样放在文章最后，感兴趣的读者可以自己实现一下。

根据上文中的介绍，制定空中花园策略的交易逻辑：

2.开盘价>昨日收盘价*1.01且开盘价>当天第一根k线的最高价，做多；

3.开盘价<昨日收盘价*0.99且开盘价<当天第一根k线的最低价，做空；

笔者根据互联网上的资料将高开、低开的比例比较死板地设置成了1.01与0.99，如果读者对这个策略比较感兴趣，也可以自己添加2个参数来控制高开、低开的比例来进行回测。

选择2014年9月10日到2019年9月01日的股指期货进行回测，k1,k2都设定为0.5,使用的资金比率设定为20%，均线和ATR计算使用的K线条数都为10根，回测结果如图：

根据回测内容来看，总体还是有一些收益，不过收益部分主要集中在2016年之前，后面的年份表现就比较一般了。可以用demos中的Optimizer来寻找更加合适的参数。

选择2014年9月10日到2019年9月01日的股指期货进行回测,使用的资金比率设定为20%,回测结果：

分析回测结果，总体收益率比较一般，菲阿里策略也是一种趋势突破型的策略，交易逻辑在于，如果价格突破昨日的最高价，则认为还有继续上涨的潜力，此时买入，而下个跌破昨日最低点时，则认为还有继续向下的动力，此时卖出开仓，以获取其中的收益。总体而言，菲阿里策略判断趋势成立的标准实在有些过于简单，目前的市场趋势判断不仅是靠昨日的最高价最低价就可以建立的，因此获得的收益率也是不太理想。

选择2014年9月10日到2019年9月01日的股指期货进行回测,使用的资金比率设定为20%,回测结果：

空中花园策略也是趋势突破型的策略，策略进场只考虑当日的开盘价与昨日的收盘价之间的关系，博取大幅高开或者大幅低开的行情，本次回测仅仅使用空中花园策略默认定义的高开或低开幅度1%，但是意外地表现出了较好的回测结果。不过2016年至2018年初之间策略表现一般，资金曲线较平，推测是因为这段时间之内股指波动相对较小，因此1%的高开、低开判定条件相比之下过于严格，没有触发开仓条件。感兴趣的读者可以将策略中的高开与低开比例作为参数，调整参数后再进行回测。

将策略代码的.py文件放入wtpy/demos/cta_fut_bt/strategies，然后修改runBT文件，传入相应的参数，即可自己进行策略的回测。至此本文关于几个经典cta日间策略的分享就结束了，后续笔者还会分享一些经典的cta跨日策略的编写过程。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
TR=Max(Max((High-Low),ABS(REF(Close,1)-High)),ABS(REF(Close,1)-Low)); ATR=TR 的 N 日移动平均。
```

Example 2 (python):
```python
# 首先需要设定判断清仓时间区间的cleartimes，有夜盘和白盘需要清仓，格式如[[1455,1515],[2255,2300]]，因为回测使用股指期货，没有夜盘，所以只需使用14：55~15：15的区间
cleartimes = [[1455,1515]]  # 传入时间区间
self.__cleartimes__ = cleartimes  # 写在def __init__()中
# 以下写在 def on_calculate()中
curPos = context.stra_get_position(code)  # 获取当前持仓和价格
curTime = context.stra_get_time()

bCleared = False
for tmPair in self.__cleartimes__:
    if curTime >= tmPair[0] and curTime <= tmPair[1]:  # 当时间大于等于14：55时，对五分钟线数据来说，当日倒数第二根k线已经闭合，应在当日最后一根k线进行尾盘清仓
        if curPos != 0: # 如果持仓不为0，则要检查尾盘清仓
            context.stra_set_positions(code, 0, "clear") # 清仓直接设置仓位为0
            context.stra_log_text("尾盘清仓")
            bCleared = True
            break

            if bCleared:
                return
```

Example 3 (unknown):
```unknown
# 参数说明：k1,k2是传入的上轨、下轨的系数，barN_atr是计算ATR时使用的bar根数，barN_ma是计算均线时使用的bar根数
    	TR_SUM = 0
        # 计算之前N个bar的ATR
        for i in range(barN_atr):
            TR_SUM += max(highs[-1-i]-lows[-1-i], highs[-1-i]-closes[-2-i], closes[-2-i]-lows[-1-i])
        ATR = TR_SUM / barN_atr
        # 计算中轨，为n根bar收盘价的均线
        ma = np.average(closes[-barN_ma:-1])  # closes是通过context.stra_get_bars接口拉取df_bars之后用closes = df_bars.closes 得到的收盘价序列
        up_price = ma + k1 * ATR
        down_price = ma - k2 * ATR
```

Example 4 (unknown):
```unknown
if curPos == 0:
        	# 当价格突破上轨时做多，手数由当前总资金乘以参数money_pct然后除以一手所需保证金算出
            if curPrice > up_price:
                context.stra_enter_long(code, math.floor(cur_money * money_pct/trdUnit_price), 'enterlong')
                context.stra_log_text('当前价格:%.2f > 由atr计算出的上轨：%.2f，做多%s手' % (curPrice,
                                     up_price, math.floor(cur_money * money_pct/trdUnit_price)))
                return
            # 当价格跌破下轨时做空，手数同上
            if curPrice < down_price:
                context.stra_enter_short(code, math.floor(cur_money * money_pct/trdUnit_price), 'entershort')
                context.stra_log_text('当前价格:%.2f < 由atr计算出的下轨:%.2f,做空%s手'%(curPrice,
                                        down_price, math.floor(cur_money * money_pct/trdUnit_price)))
                return
        elif curPos != 0:
            # 当持有多仓且价格跌破均线时，平多
            if curPos > 0 and curPrice < ma:
                context.stra_set_position(code, 0, 'clear')
                context.stra_log_text('当前价格:%.2f < 由均线计算出的中轨:%.2f,平多' % (curPrice, ma,))
                return
            # 当持有空仓且价格突破均线时，平空
            elif curPos < 0 and curPrice > ma:
                context.stra_set_position(code, 0, 'clear')
                context.stra_log_text('当前价格:%.2f > 由均线计算出的中轨:%.2f,平空' % (curPrice, ma,))
                return
```

---

## 常见策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/strategies/index.html

**Contents:**
- 常见策略

© 版权所有 WonderTrader。 版本 280b46ec.

---

## HFT策略编写及回测

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/usage/hftstra.html

**Contents:**
- HFT策略编写及回测
- 多引擎设计
- HFT策略概述
- HFT策略接口
- 一个简单的HFT策略

WonderTrader采用多引擎设计，针对不同类型的策略提供独立的引擎，目前支持的引擎包括：

CTA引擎，也叫同步策略引擎，一般适用于标的较少，计算逻辑较快的策略，事件+时间驱动。典型的应用场景包括单标的择时、中频以下的套利等。Demo中提供的DualThrust策略，单次重算平均耗时，Python实现版本约70多微秒，C++实现版本约4.5微秒。

SEL引擎，也叫异步策略引擎，一般适用于标的较多，计算逻辑耗时较长的策略，时间驱动。典型应用场景包括多因子选股策略、截面多空策略等。

HFT引擎，也叫高频策略引擎，主要针对高频或者低延时策略，事件驱动，系统延迟在1微秒之内

UFT引擎，也叫极速策略引擎，主要针对超高频或者超低延时策略，事件驱动，系统延迟在200纳秒之内

要基于WonderTrader实现HFT策略，有两种方式：

基于wtcpp，直接使用纯C++开发策略。这种方式适合有较高C++编程能力，并且对延迟有较高要求的用户

基于wtpy，使用python开发策略。这种方式适合对延迟有要求但是不是特别敏感，对C++也不大熟悉的用户

无论哪种方式，底层都是纯C++实现，执行效率有保障。HFT引擎和CTA引擎的最大区别在于：

HFT引擎中策略直接和交易接口绑定，交易接口的回报直接推送给策略，策略需要自行维护订单。

CTA引擎中，策略信号和执行是彻底剥离的，CTA引擎可以和多个执行器对接，从而实现一个组合对接多个交易通道的机制。更多信息可以参考WonderTrader的M+1+N的执行架构。

和CTA策略一样，HFT策略也有一个基础结构：

从上面的代码可以看出，整个策略的结构大致可以分为四块：

其中行情数据的回调，主要包括on_tick、on_bar和level2数据回调，本文中只需要关注on_tick即可；交易通道的回调，主要是通知策略交易通道的连接和断开事件；交易回报的回调，主要是订单回报、成交回报以及下单回报。

HftContext提供了相应的各种接口，更多信息可以参考API相关文档的介绍。

wtpy的demo中提供了一个简单的HFT策略，大致逻辑就是利用最新的tick计算一个理论价格，然后比较最新价和理论价格的大小，再生成交易信号。

更多HFT相关信息可以参考wtpy/demos/hft_fut，也可以参考专栏文章WonderTrader高频交易初探及v0.6发布。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
class BaseHftStrategy:
    '''
    HFT策略基础类，所有的策略都从该类派生
    包含了策略的基本开发框架
    '''
    def __init__(self, name:str):
        self.__name__ = name
        
    
    def name(self) -> str:
        return self.__name__


    def on_init(self, context:HftContext):
        '''
        策略初始化，启动的时候调用
        用于加载自定义数据

        @context    策略运行上下文
        '''
        return
    
    def on_session_begin(self, context:HftContext, curTDate:int):
        '''
        交易日开始事件

        @curTDate   交易日，格式为20210220
        '''
        return

    def on_session_end(self, context:HftContext, curTDate:int):
        '''
        交易日结束事件

        @curTDate   交易日，格式为20210220
        '''
        return

    def on_backtest_end(self, context:CtaContext):
        '''
        回测结束时回调，只在回测框架下会触发

        @context    策略上下文
        '''
        return

    def on_tick(self, context:HftContext, stdCode:str, newTick:dict):
        '''
        Tick数据进来时调用

        @context    策略运行上下文
        @stdCode    合约代码
        @newTick    最新Tick
        '''
        return

    def on_order_detail(self, context:HftContext, stdCode:str, newOrdQue:dict):
        '''
        逐笔委托数据进来时调用

        @context    策略运行上下文
        @stdCode    合约代码
        @newOrdQue  最新逐笔委托
        '''
        return

    def on_order_queue(self, context:HftContext, stdCode:str, newOrdQue:dict):
        '''
        委托队列数据进来时调用

        @context    策略运行上下文
        @stdCode    合约代码
        @newOrdQue  最新委托队列
        '''
        return

    def on_transaction(self, context:HftContext, stdCode:str, newTrans:dict):
        '''
        逐笔成交数据进来时调用

        @context    策略运行上下文
        @stdCode    合约代码
        @newTrans   最新逐笔成交
        '''
        return

    def on_bar(self, context:HftContext, stdCode:str, period:str, newBar:dict):
        '''
        K线闭合时回调

        @context    策略上下文
        @stdCode    合约代码
        @period     K线周期
        @newBar     最新闭合的K线
        '''
        return

    def on_channel_ready(self, context:HftContext):
        '''
        交易通道就绪通知

        @context    策略上下文
        '''
        return

    def on_channel_lost(self, context:HftContext):
        '''
        交易通道丢失通知

        @context    策略上下文
        '''
        return

    def on_entrust(self, context:HftContext, localid:int, stdCode:str, bSucc:bool, msg:str, userTag:str):
        '''
        下单结果回报

        @context    策略上下文
        @localid    本地订单id
        @stdCode    合约代码
        @bSucc      下单结果
        @mes        下单结果描述
        '''
        return

    def on_order(self, context:HftContext, localid:int, stdCode:str, isBuy:bool, totalQty:float, leftQty:float, price:float, isCanceled:bool, userTag:str):
        '''
        订单回报
        @context    策略上下文
        @localid    本地订单id
        @stdCode    合约代码
        @isBuy      是否买入
        @totalQty   下单数量
        @leftQty    剩余数量
        @price      下单价格
        @isCanceled 是否已撤单
        '''
        return

    def on_trade(self, context:HftContext, localid:int, stdCode:str, isBuy:bool, qty:float, price:float, userTag:str):
        '''
        成交回报

        @context    策略上下文
        @stdCode    合约代码
        @isBuy      是否买入
        @qty        成交数量
        @price      成交价格
        '''
        return

    def on_position(self, context:HftContext, stdCode:str, isLong:bool, prevol:float, preavail:float, newvol:float, newavail:float):
        '''
        初始持仓回报
        实盘可用, 回测的时候初始仓位都是空, 所以不需要

        @context    策略上下文
        @stdCode    合约代码
        @isLong     是否为多
        @prevol     昨仓
        @preavail   可用昨仓
        @newvol     今仓
        @newavail   可用今仓
        '''
        return
```

Example 2 (python):
```python
from wtpy import BaseHftStrategy
from wtpy import HftContext

from datetime import datetime

def makeTime(date:int, time:int, secs:int):
    '''
    将系统时间转成datetime\n
    @date   日期，格式如20200723\n
    @time   时间，精确到分，格式如0935\n
    @secs   秒数，精确到毫秒，格式如37500
    '''
    return datetime(year=int(date/10000), month=int(date%10000/100), day=date%100, 
        hour=int(time/100), minute=time%100, second=int(secs/1000), microsecond=secs%1000*1000)

class HftStraDemo(BaseHftStrategy):

    def __init__(self, name:str, code:str, expsecs:int, offset:int, freq:int=30):
        BaseHftStrategy.__init__(self, name)

        '''交易参数'''
        self.__code__ = code            #交易合约
        self.__expsecs__ = expsecs      #订单超时秒数
        self.__offset__ = offset        #指令价格偏移
        self.__freq__ = freq            #交易频率控制，指定时间内限制信号数，单位秒

        '''内部数据'''
        self.__last_tick__ = None       #上一笔行情
        self.__orders__ = dict()        #策略相关的订单
        self.__last_entry_time__ = None #上次入场时间
        self.__cancel_cnt__ = 0         #正在撤销的订单数
        self.__channel_ready__ = False  #通道是否就绪
        

    def on_init(self, context:HftContext):
        '''
        策略初始化，启动的时候调用\n
        用于加载自定义数据\n
        @context    策略运行上下文
        '''

        #先订阅实时数据
        context.stra_sub_ticks(self.__code__)

        self.__ctx__ = context

    def check_orders(self):
        #如果未完成订单不为空
        if len(self.__orders__.keys()) > 0 and self.__last_entry_time__ is not None:
            #当前时间，一定要从api获取，不然回测会有问题
            now = makeTime(self.__ctx__.stra_get_date(), self.__ctx__.stra_get_time(), self.__ctx__.stra_get_secs())
            span = now - self.__last_entry_time__
            if span.total_seconds() > self.__expsecs__: #如果订单超时，则需要撤单
                for localid in self.__orders__:
                    self.__ctx__.stra_cancel(localid)
                    self.__cancel_cnt__ += 1
                    self.__ctx__.stra_log_text("cancelcount -> %d" % (self.__cancel_cnt__))

    def on_tick(self, context:HftContext, stdCode:str, newTick:dict):
        if self.__code__ != stdCode:
            return

        #如果有未完成订单，则进入订单管理逻辑
        if len(self.__orders__.keys()) != 0:
            self.check_orders()
            return

        if not self.__channel_ready__:
            return

        self.__last_tick__ = newTick

        #如果已经入场，则做频率检查
        if self.__last_entry_time__ is not None:
            #当前时间，一定要从api获取，不然回测会有问题
            now = makeTime(self.__ctx__.stra_get_date(), self.__ctx__.stra_get_time(), self.__ctx__.stra_get_secs())
            span = now - self.__last_entry_time__
            if span.total_seconds() <= 30:
                return

        #信号标志
        signal = 0
        #最新价作为基准价格
        price = newTick["price"]
        #计算理论价格
        pxInThry = (newTick["bid_price_0"]*newTick["ask_qty_0"] + newTick["ask_price_0"]*newTick["bid_qty_0"]) / (newTick["ask_qty_0"] + newTick["bid_qty_0"])

        context.stra_log_text("理论价格%f，最新价：%f" % (pxInThry, price))

        if pxInThry > price:    #理论价格大于最新价，正向信号
            signal = 1
            context.stra_log_text("出现正向信号")
        elif pxInThry < price:  #理论价格小于最新价，反向信号
            signal = -1
            context.stra_log_text("出现反向信号")

        if signal != 0:
            #读取当前持仓
            curPos = context.stra_get_position(self.__code__)
            #读取品种属性，主要用于价格修正
            commInfo = context.stra_get_comminfo(self.__code__)
            #当前时间，一定要从api获取，不然回测会有问题
            now = makeTime(self.__ctx__.stra_get_date(), self.__ctx__.stra_get_time(), self.__ctx__.stra_get_secs())

            #如果出现正向信号且当前仓位小于等于0，则买入
            if signal > 0 and curPos <= 0:
                #买入目标价格=基准价格+偏移跳数*报价单位
                targetPx = price + commInfo.pricetick * self.__offset__

                #执行买入指令，返回所有订单的本地单号
                ids = context.stra_buy(self.__code__, targetPx, 1, "buy")

                #将订单号加入到管理中
                for localid in ids:
                    self.__orders__[localid] = localid
                
                #更新入场时间
                self.__last_entry_time__ = now

            #如果出现反向信号且当前持仓大于等于0，则卖出
            elif signal < 0 and curPos >= 0:
                #买入目标价格=基准价格-偏移跳数*报价单位
                targetPx = price - commInfo.pricetick * self.__offset__

                #执行卖出指令，返回所有订单的本地单号
                ids = context.stra_sell(self.__code__, targetPx, 1, "sell")

                #将订单号加入到管理中
                for localid in ids:
                    self.__orders__[localid] = localid
                
                #更新入场时间
                self.__last_entry_time__ = now


    def on_bar(self, context:HftContext, stdCode:str, period:str, newBar:dict):
        return

    def on_channel_ready(self, context:HftContext):
        undone = context.stra_get_undone(self.__code__)
        if undone != 0 and len(self.__orders__.keys()) == 0:
            context.stra_log_text("%s存在不在管理中的未完成单%f手，全部撤销" % (self.__code__, undone))
            isBuy = (undone > 0)
            ids = context.stra_cancel_all(self.__code__, isBuy)
            for localid in ids:
                self.__orders__[localid] = localid
            self.__cancel_cnt__ += len(ids)
            context.stra_log_text("cancelcnt -> %d" % (self.__cancel_cnt__))
        self.__channel_ready__ = True

    def on_channel_lost(self, context:HftContext):
        context.stra_log_text("交易通道连接丢失")
        self.__channel_ready__ = False

    def on_entrust(self, context:HftContext, localid:int, stdCode:str, bSucc:bool, msg:str, userTag:str):
        if bSucc:
            context.stra_log_text("%s下单成功，本地单号：%d" % (stdCode, localid))
        else:
            context.stra_log_text("%s下单失败，本地单号：%d，错误信息：%s" % (stdCode, localid, msg))

    def on_order(self, context:HftContext, localid:int, stdCode:str, isBuy:bool, totalQty:float, leftQty:float, price:float, isCanceled:bool, userTag:str):
        if localid not in self.__orders__:
            return

        if isCanceled or leftQty == 0:
            self.__orders__.pop(localid)
            if self.__cancel_cnt__ > 0:
                self.__cancel_cnt__ -= 1
                self.__ctx__.stra_log_text("cancelcount -> %d" % (self.__cancel_cnt__))
        return

    def on_trade(self, context:HftContext, localid:int, stdCode:str, isBuy:bool, qty:float, price:float, userTag:str):
        return
```

---

## 实盘完整攻略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/usage/product.html

**Contents:**
- 实盘完整攻略
- 实盘运行步骤：
- 文件配置
- 准备数据组件
- 准备环境
- 修改策略
- 启动实盘

本文将完整的介绍一下，如何搭建一个可以直接用于生产环境的完整的流程。 本文将以配置股票仿真盘为例，使用DualThrust策略，通过XTP行情通道落地数据，并通过XTP仿真交易通道来进行仿真交易。

终端运行datakit组件runDT.py，一般在开盘前启动，如9:20，datakit负责在实盘中录制实时行情数据，存储在指定的数据目录中，同时通知策略进行接收

复制股票数据组件demohttps://github.com/wondertrader/wtpy/tree/master/demos/datakit_stk

打开配置文件dtcfg.yaml，配置XTP仿真行情通道

打开配置文件dtcfg.yaml，配置数据落地目录

完成上述工作以后，就可以执行runDT.py启动数据组件了

复制股票实盘demohttps://github.com/wondertrader/wtpy/tree/master/demos/cta_stk

打开配置文件config.yaml，修改环境配置

修改数据读取路径，确认和datakit设置的存储路径一致

打开配置文件tdtraders.yaml，修改XTP交易通道配置

打开配置文件tdparsers.yaml，修改行情通道配置

至此，config.yaml里面需要用户修改的修改项基本就修改完成了，其他的配置项保持即可。

DualThrust策略，在编写的时候，就考虑到了对股票的支持，所以加了一个参数isForStk。对于一般期货策略逻辑，要改写成支持股票，主要需要注意以下几点：

对于国内市场来说，股票买入必须按照1手100股来挂单

策略修改完成以后，还可以放到回测环境下，做一次回测。回测的教程可以参考回测自己的策略这一章。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
common  # 基础文件
    stk_comms.json  # 品种列表（交易所、品种类型、品种信息）
    stocks.json  # 股票/ETF列表
    holidays.json  # 节假日列表
    stk_sessions.json  # 交易时间模板
    fees_stk.json  # 佣金配置文件
datakit_stk  # 数据组件
    runDT.py  # 执行runDT.py启动数据组件
    dtcfg.yaml  # 行情通道、广播端口、数据落地，原demo中分离出了mdparsers.yaml, statemonitor.yaml
    logcfgdt.yaml
cta_stk  # 实盘组件
    run.py
    Strategies
        策略.py
    config.yaml  # 原demo中分离出了filters.yaml, executers.yaml, tdparsers.yaml, tdtraders.yaml, actpolicy.yaml
    logcfg.yaml
```

Example 2 (unknown):
```unknown
parsers:
-   active: true
    id: parser
    module: ParserXTP
    host: 120.27.164.138
    port: '6002'
    protocol: 1
    buffsize: 128
    clientid: 1    
    hbinterval: 15    
    user: 你的XTP仿真账号
    pass: 你的XTP仿真密码
    code: SSE.000001,SSE.600009,SSE.600036,SSE.600276,SZSE.000001
```

Example 3 (unknown):
```unknown
writer:
    module: WtDtStorage #数据存储模块
    async: true         #同步落地还是异步落地，期货推荐同步，股票推荐异步
    groupsize: 20       #日志分组大小，主要用于控制日志输出，当订阅合约较多时，推荐1000以上，当订阅的合约数较少时，推荐100以内
    path: ../STK_Data   #数据存储的路径
    savelog: false      #是否保存tick到csv
```

Example 4 (unknown):
```unknown
broadcaster:                    # UDP广播器配置项
    active: true
    bport: 3997                 # UDP查询端口，主要是用于查询最新的快照
    broadcast:                  # 广播配置
    -   host: 255.255.255.255   # 广播地址，255.255.255.255会向整个局域网广播，但是受限于路由器
        port: 9001              # 广播端口，接收端口要和广播端口一致
        type: 2                 # 数据类型，固定为2
```

---

## 优化自己的策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/usage/optimizer.html

**Contents:**
- 优化自己的策略
- 参数优化器简介
- DualThrust介绍
- 初步回测
- 加入止损
- 加入止盈
- 完整回测

wtpy针对CTA策略写了一个参数优化器WtCtaOptimizer，该模块主要通过遍历参数的方式，利用multiprocessing模块并发启动多个进程进行历史回测，然后汇总回测结果生成回测报表。 参数优化器主要支持以下几种参数类型：

数值类可变参数，即通过设置步长和数值范围进行参数遍历，绝大多数参数都是这个类型

列表类可变参数，即给定预设的列表，进行参数选择，一般用于遍历标的、周期等参数

首先我们还是祭出WonderTrader惯用的DualThrust策略。

DualThrust策略是一个突破策略，利用前N日的开高低收确定一个Range，然后利用当前K线的开盘价加上Range乘以一个系数得到一个上轨，减去Range乘以一个系数得到一个下轨，当最新价突破上轨时做多，突破下轨时做空。

DualThrust原模型中，上下轨是对称的，文本所用的DualThrust策略在实现时把上下轨的系数作为两个不同的参数，策略核心逻辑如下：

策略准备好了，我们先针对DualThrust不加止盈止损的绩效进行一个最优参数选择。参数优化器的调用代码如下：

本例中设置工作进程的个数为4个，用户可以自行根据自己的需求设置，一般推荐设置的个数和CPU内核数匹配。 优化器入口配置好了，就可以启动回测了。

优化器批量回测完成以后，我们可以得到一个汇总报表

我们先按照收益风险比大于3，进行初步筛选。

最终我们选择交易次数最多的一个参数对，即k1和k2都等于0.4这组，作为我们最基础的策略参数。先利用WtBtAnalyst做一下绩效分析，2019年股指期货大概在4000点左右，合约价值大概120万，按照一倍杠杆，设置初始资金为120万。

从绩效来看，这组参数还是很有潜力的。至于本例是否涉及到过拟合、是否要需要进行样本外数据的回测，这不是本文探讨的范畴。从绩效分析上来看，年化收益率达到了52.6%，但是最大回撤只有5.36%。

基本参数组合已经确定了，接下来我们要加入止损逻辑。为了简化逻辑，我们采用固定价差止损的方式，并且在策略进出场逻辑之前进行判断。止损的计算方式采用最新价和进场价的价差，当价差达到一定点位，则进行止损。 我们将止损点位区间设置为0到10，每次0.2一跳。代码如下：

汇总的回测结果如下。通过简单的分析，我们不难发现，虽然设置止损以后，对提高收益风险比有很大的促进，最大的是-0.2点止损，收益风险比可以达到17倍以上。但是付出的代价就是总收益的下降，设置了止损点以后，最终净收益比未设置止损点的时候，少了约30万到40万，约为原来的1/3到1/2。

下面我们再来看一下止盈逻辑。本文为了简化逻辑，采用固定点位止盈。 为了锁定更多利润，我们将止盈点的范围设置为0~500，每次5.0一跳。代码如下：

我们将汇总的结果中收益风险比大于4的截取出来，如下图：

由上图可以见，止盈逻辑对策略绩效的提升还是比较明显的，但是当止盈点位在150点到230之间，收益风险比基本上达到稳定，策略绩效提升约10%以上。

实际使用中，止盈止损当然不可能分为两个互相独立的逻辑来使用，反而是配合使用的。所以我们最后再将止盈止损的参数进行联合优化。 根据上面的回测结果，我们将止盈点位的范围控制在150到230之间，止损点位控制在-30到-10之间。代码如下：

从上图可以看出来，止盈止损完整的逻辑，对于提高收益风险比还是有提升的，但是净利润还是会比无止盈止损的时候降低不少。 我们再选择收益风险比最高的一组参数，看一下策略的绩效分析。

最大回撤并没有比无止盈止损的逻辑低，反而直接损失了收益率。 我们最后再看一下只有止盈逻辑的最优参数的绩效分析。

综合上面我们可以看出来，止损逻辑并没有对策略绩效有比较正向的改善，反而止盈逻辑能锁定不少原本要回吐的利润。止损无效的原因何在，可能是因为止损点位的范围设置不合理，或者是因为别的原因，这个就不在本文的讨论范围了，有兴趣的读者可以自行研究一下。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
optimizer.add_fixed_param(name="barCnt", val=50)
```

Example 2 (unknown):
```unknown
optimizer.add_mutable_param(name="k1", start_val=0.1, end_val=1.0, step_val=0.1, ndigits = 1)
```

Example 3 (unknown):
```unknown
optimizer.add_listed_param(name="code", val_list=["CFFEX.IF.HOT","CFFEX.IC.HOT"])
```

Example 4 (python):
```python
def on_calculate(self, context:CtaContext):
    code = self.__code__    #品种代码

    trdUnit = 1

    #读取最近50条1分钟线(dataframe对象)
    theCode = code
    df_bars = context.stra_get_bars(theCode, self.__period__, self.__bar_cnt__, isMain = True)

    #把策略参数读进来，作为临时变量，方便引用
    days = self.__days__
    k1 = self.__k1__
    k2 = self.__k2__

    #平仓价序列、最高价序列、最低价序列
    closes = df_bars.closes
    highs = df_bars.highs
    lows = df_bars.lows

    #读取days天之前到上一个交易日位置的数据
    hh = np.amax(highs[-days:-1])
    hc = np.amax(closes[-days:-1])
    ll = np.amin(lows[-days:-1])
    lc = np.amin(closes[-days:-1])

    #读取今天的开盘价、最高价和最低价
    # lastBar = df_bars.get_last_bar()
    openpx = df_bars.opens[-1]
    highpx = df_bars.highs[-1]
    lowpx = df_bars.lows[-1]

    '''
    !!!!!这里是重点
    1、首先根据最后一条K线的时间，计算当前的日期
    2、根据当前的日期，对日线进行切片,并截取所需条数
    3、最后在最终切片内计算所需数据
    '''

    #确定上轨和下轨
    upper_bound = openpx + k1* max(hh-lc,hc-ll)
    lower_bound = openpx - k2* max(hh-lc,hc-ll)

    #读取当前仓位
    curPos = context.stra_get_position(code)/trdUnit

    if curPos == 0:
        if highpx >= upper_bound:
            context.stra_enter_long(code, 1*trdUnit, 'enterlong')
            context.stra_log_text("向上突破%.2f>=%.2f，多仓进场" % (highpx, upper_bound))
            return

        if lowpx <= lower_bound and not self.__is_stk__:
            context.stra_enter_short(code, 1*trdUnit, 'entershort')
            context.stra_log_text("向下突破%.2f<=%.2f，空仓进场" % (lowpx, lower_bound))
            return
    elif curPos > 0:
        if lowpx <= lower_bound:
            context.stra_exit_long(code, 1*trdUnit, 'exitlong')
            context.stra_log_text("向下突破%.2f<=%.2f，多仓出场" % (lowpx, lower_bound))
            return
    else:
        if highpx >= upper_bound and not self.__is_stk__:
            context.stra_exit_short(code, 1*trdUnit, 'exitshort')
            context.stra_log_text("向上突破%.2f>=%.2f，空仓出场" % (highpx, upper_bound))
            return
```

---

## 网格交易CTA策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/strategies/gridstra.html

**Contents:**
- 网格交易CTA策略
- 1.网格交易法介绍
- 2.策略实现
- 3.回测结果
- 4.其他两种类型的网格策略

​ 根据笔者对于网格策略的粗略了解，在具有多空交易的市场中，网格策略主要可以分为三种：做多、做空、和中性，而在只能做多的股票市场中，就只能使用做多的网格策略了。这里先简单说明一下只做多的网格策略的具体交易规则：首先决定网格格数，以及上涨与下跌时每格相比入场价格的涨跌幅，然后选择合适的入场价格入场做多或买入50%可用资金，当价格上涨或下跌超过一网格的涨跌幅后，仓位也随之变化，每上涨一格卖出一份头寸，每下跌一格买入一份头寸，具体交易规则可以参考下图：

​ 如果是做空版本的网格策略，交易规则正好与上文所述的做多版本的网格策略相反：在入场价格做空50%的资金，价格每上涨一格则增加一份做空资金，每下跌一格则平仓一份做空资金。而中性版本的网格策略，则没有开始时50%资金的底仓，每上涨一格则做空一份资金，每下跌一格则做多一份资金。

​ 下文将详述在wtpy上实现一个只做多的网格策略的编写过程。

​ 了解了策略的交易逻辑和算法以后，我们首先要确定策略需要的参数，其中一部分是cta策略常用的固定参数，另一部分是网格策略需要的固定以及可变参数。

​ 参数中的短时均线和长时均线用于判断策略进场的基准价格，当短时均线上穿长时均线且没有仓位时会以当前价格作为基准价格进场。

第一步，根据交易品种的特点选择合适的参数·如：网格格数5。上涨时每格涨幅0.03、下跌时每格跌幅-0.02，短均线天数5，长均线天数10；

第二步，根据上一步中所选的参数，生成包含网格交易每格的边界以及每格的持仓比例的两个列表；

第三步，读取当前价格与仓位，与第二部中所算出的两个列表进行对照，获得交易之后所要达到的仓位比例，也就是目标仓位，然后计算得出达到目标仓位所需交易的手数并进行相应的操作。

写好了策略主体，将策略主体代码命名为GridStra_only_long.py放入wtpy/demos/cta_fut_bt/strategies目录下，然后我们还需要runBT.py来让策略跑起来，runBT.py的格式和代码如下：

将runBT.py放入wtpy/demos/cta_fut_bt运行即可，其中的参数可以自行修改替换。这次是使用的股指期货IF来进行回测，回测需要的日线数据可以通过下面的链接获取，然后放入wtpy/demos/storage/csv文件夹中。

https://pan.baidu.com/s/1DgULTifDaTS6a6VF36uvng?pwd=j8ym

​ 打开运行之后生成的xlsx文件，可以看到策略的绩效，如下图所示，没有优化过、随便设置的参数表现非常一般，可以使用demo中的optimizer来优化参数来找到更加合适的参数，具体用法可以参考wtpy的其他相关文章。

理解了做多的网格策略后，只要修改入场逻辑为死叉，把做多改为相反的做空，即可把上文中的策略修改为做空的网格策略，而中性的网格策略交易逻辑部分则较为不同，笔者自己实现了一下，与只做多的代码有主要区别的部分放在下面：

至此，在wtpy上实现一个网格策略的详细步骤介绍完毕，后续笔者将继续摸索wtpy的各个模块，分享在wtpy上一些其他经典的cta日间与跨日策略的实现过程。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
self.__short_days__ = short_days  # 计算短时均线时使用的天数
        self.__long_days__ = long_days  # 计算长时均线时使用的天数
        self.__num__ = num  # 单边网格格数
        self.__p1__ = p1    # 上涨时每格相比基准格的涨幅
        self.__p2__ = p2    # 下跌时每格相比基准格的跌幅(<0)
        self.__period__ = period    # 交易k线的时间级，如5分钟，1分钟
        self.__bar_cnt__ = barCnt   # 拉取的bar的次数
        self.__code__ = code 		# 策略实例名称
        self.__capital__ = capital  # 起始资金
        self.__margin_rate__ = margin_rate  # 保证金率
        self.__stop_loss__ = stop_loss  # 止损系数，止损点算法为网格最低格的价格*stop_loss
```

Example 2 (python):
```python
class BaseCtaStrategy:
    '''
    CTA策略基础类，所有的策略都从该类派生\n
    包含了策略的基本开发框架
    '''
    def __init__(self, name):
        self.__name__ = name

    def on_init(self, context:CtaContext):
        '''
        策略初始化，启动的时候调用\n
        用于加载自定义数据\n
        @context    策略运行上下文
        '''
        return
        
    def on_calculate(self, context:Context):
        '''
        策略主调函数，所有的计算逻辑都在这里完成
        '''
        return
```

Example 3 (python):
```python
from wtpy import BaseCtaStrategy
from wtpy import CtaContext
import numpy as np
import math


class GridStra(BaseCtaStrategy):
    
    def __init__(self, name:str, code:str, barCnt:int, period:str, short_days:int, long_days:int, num:int, p1:float, p2:float, capital, margin_rate= 0.1,stop_loss = 0.8):
        BaseCtaStrategy.__init__(self, name)

        self.__short_days__ = short_days  # 计算短时均线时使用的天数
        self.__long_days__ = long_days  # 计算长时均线时使用的天数
        self.__num__ = num  # 单边网格格数
        self.__p1__ = p1    # 上涨时每格相比基准格的涨幅
        self.__p2__ = p2    # 下跌时每格相比基准格的跌幅(<0)
        self.__period__ = period    # 交易k线的时间级，如5分钟，1分钟
        self.__bar_cnt__ = barCnt   # 拉取的bar的次数
        self.__code__ = code        # 策略实例名称
        self.__capital__ = capital  # 起始资金
        self.__margin_rate__ = margin_rate  # 保证金率
        self.__stop_loss__ = stop_loss  # 止损系数，止损点算法为网格最低格的价格*stop_loss


    def on_init(self, context:CtaContext):
        code = self.__code__    # 品种代码

        context.stra_get_bars(code, 'd1', self.__bar_cnt__, isMain=False)  # 在策略初始化阶段调用一次后面要拉取的K线
        context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain=True)
        context.stra_log_text("GridStra inited")

        pInfo = context.stra_get_comminfo(code)  # 调用接口读取品类相关数据
        self.__volscale__ = pInfo.volscale
        # 生成网格交易每格的边界以及每格的持仓比例
        self.__price_list__ = [1]
        self.__position_list__ = [0.5]
        num = self.__num__
        p1 = self.__p1__
        p2 = self.__p2__
        for i in range(num):
            self.__price_list__.append(1+(i+1)*p1)
            self.__price_list__.append(1+(i+1)*p2)
            self.__position_list__.append(0.5+(i+1)*0.5/num)
            self.__position_list__.append(0.5-(i+1)*0.5/num)
        self.__price_list__.sort()
        self.__position_list__.sort(reverse=True)

        print(self.__price_list__)
        print(self.__position_list__)


    def on_calculate(self, context:CtaContext):
        code = self.__code__    #品种代码

        #把策略参数读进来，作为临时变量，方便引用
        margin_rate = self.__margin_rate__
        price_list = self.__price_list__
        position_list = self.__position_list__
        capital = self.__capital__
        volscale = self.__volscale__
        stop_loss = self.__stop_loss__
        short_days = self.__short_days__
        long_days = self.__long_days__
        theCode = code

        # 读取日线数据以计算均线的金叉
        df_bars = context.stra_get_bars(theCode, 'd1', self.__bar_cnt__, isMain=False)
        closes = df_bars.closes
        ma_short_days1 = np.average(closes[-short_days:-1])
        ma_long_days1 = np.average(closes[-long_days:-1])
        ma_short_days2 = np.average(closes[-short_days - 1:-2])
        ma_long_days2 = np.average(closes[-long_days - 1:-2])
        # 读取最近50条5分钟线
        context.stra_get_bars(theCode, self.__period__, self.__bar_cnt__, isMain=True)

        #读取当前仓位,价格
        curPos = context.stra_get_position(code)
        curPrice = context.stra_get_price(code)
        if curPos == 0:
            self.cur_money = context.stra_get_fund_data(0) + capital  # 当没有持仓时，用总盈亏加上初始资金计算出当前总资金

        trdUnit_price = volscale * curPrice * margin_rate  # 计算交易一手所选的期货所需的保证金

        if curPos == 0:
            if (ma_short_days1 > ma_long_days1) and (ma_long_days2 > ma_short_days2):  # 当前未持仓且日线金叉时作为基准价格进场
                # 做多50%资金的手数，为了不使杠杆率过高，总资金乘以一手期货的保证金率再计算手数
                context.stra_enter_long(code, math.floor(self.cur_money*margin_rate*0.5/trdUnit_price), 'enterlong')
                self.benchmark_price = context.stra_get_price(code)  # 记录进场时的基准价格
                self.grid_pos = 0.5  # 记录grid_pos表示网格当前实际仓位
                context.stra_log_text("进场基准价格%.2f" % (self.benchmark_price))  # 输出日志到终端
                return

        elif curPos != 0:
            #  当前有持仓时，先根据当前价格计算出交易后需要达到的目标仓位target_pos
            for i in range(len(price_list)-1):
                if (price_list[i] <= (curPrice / self.benchmark_price)) & ((curPrice / self.benchmark_price) < price_list[i+1]):
                    #  当前价格处于上涨或下跌区间时 仓位都应该选择靠近基准的那一端
                    if curPrice / self.benchmark_price < 1:
                        target_pos = position_list[i+1]
                    else:
                        target_pos = position_list[i]
            # 计算当价格超出网格上边界或低于网格下边界时的目标仓位
            if curPrice / self.benchmark_price < price_list[0]:
                target_pos = 1
            if curPrice / self.benchmark_price > price_list[-1]:
                target_pos = 0
                context.stra_exit_long(code, context.stra_get_position(code), 'exitlong')
                context.stra_log_text("价格超出最大上边界，全部平多")
                self.grid_pos = target_pos
            # 止损逻辑 当价格低于下边界乘以设定的stoploss比例时止损
            if curPrice / self.benchmark_price < price_list[0] * stop_loss:
                target_pos = 0
                context.stra_exit_long(code, context.stra_get_position(code), 'exitlong')
                context.stra_log_text("价格低于最大下边界*%s，止损，全部平多" % (stop_loss))
                self.grid_pos = target_pos
            # 当目标仓位大于当前实际仓位时做多
            if target_pos > self.grid_pos:
                context.stra_enter_long(code, math.floor((target_pos-self.grid_pos)*self.cur_money/trdUnit_price), 'enterlong')
                context.stra_log_text("做多%d手,目标仓位%.2f,当前仓位%.2f,当前手数%d" % (math.floor((target_pos-self.grid_pos)*self.cur_money/trdUnit_price), target_pos, self.grid_pos,\
                                                                      context.stra_get_position(code)))
                self.grid_pos = target_pos
                return
            # 当目标仓位小于当前实际仓位时平多
            elif target_pos < self.grid_pos:
                context.stra_exit_long(code, math.floor((self.grid_pos-target_pos)*self.cur_money/trdUnit_price), 'exitlong')
                context.stra_log_text("平多%d手,目标仓位%.2f,当前仓位%.2f,当前手数%d" % (math.floor((self.grid_pos-target_pos)*self.cur_money/trdUnit_price), target_pos, self.grid_pos,\
                                                                      context.stra_get_position(code)))
                self.grid_pos = target_pos
                return
```

Example 4 (python):
```python
from wtpy import WtBtEngine,EngineType
from wtpy.apps import WtBtAnalyst

from Strategies.GridStra_only_long import GridStra


if __name__ == "__main__":
    #创建一个运行环境，并加入策略
    engine = WtBtEngine(EngineType.ET_CTA)
    engine.init('../common/', "configbt.json")
    engine.configBacktest(201409260930,201910121500)
    engine.configBTStorage(mode="csv", path="../storage/")
    engine.commitBTConfig()
    straInfo = GridStra(name='pygsol_IF', code="CFFEX.IF.HOT", barCnt=50, period="m5", short_days=5, long_days=10,\
                        num=5, p1=0.05, p2=-0.05, capital=10000000, margin_rate=0.1, stop_loss=0.8)
    engine.set_cta_strategy(straInfo)

    engine.run_backtest()

    analyst = WtBtAnalyst()
    analyst.add_strategy("pygsol_IF", folder="./outputs_bt/pygsol_IF/", init_capital=10000000, rf=0.02, annual_trading_days=240)
    analyst.run()

    kw = input('press any key to exit\n')
    engine.release_backtest()
```

---

## 使用C++编写策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/cpp/cppstra.html

**Contents:**
- 使用C++编写策略
- C++策略和Python策略对比
- C++策略的创建流程
- C++策略的定义介绍
- 如何编写自己的C++策略
- Context模式
- C++策略开发调试流程
- 写在最后

WonderTrader作为一个C++的量化交易平台，使用C++编写策略是一个很自然的事情。也只有用C++编写的策略，才能将WonderTrader的性能优势发挥到极致。

相较于wtpy使用python开发策略，C++开发策略的难度提升了不少，对于开发人员的要求也提高了非常多。但是即便如此，C++带来的性能上的跃升，还是让不少用户客服了重重困难，使用C++编写策略。

笔者曾经做过一个性能对比。DualThrust是一个非常经典的CTA日内策略，从上个世纪八十年代出现到现在，曾经长期在“前十大最赚钱的策略”中占有一席之地。DualThrust的基本逻辑并不复杂，如下图所示：

用MAX(HH-LC,HC-LL)，作为计算上下边界的基准值，用今日开盘价作为基准价，然后用上边界系数和下边界系数，分别计算出上边界的价格和下边界的价格，最终的策略逻辑如下：

当持仓为0的时候，价格突破上边界时，开多进场，价格突破下边界时，开空进场

当持仓为多的时候，价格突破上边界时，保持仓位，价格突破下边界时，多反空

当持仓为空的时候，价格突破上边界时，空反多，价格突破下边界时，保持仓位

笔者分别用C++和Python实现了该策略。C++版本核心逻辑代码如下：

时间区间：2017.01.01 09:30 - 2019.12.01 15:00

python版本单次主逻辑重算平均耗时73.301us

C++版本单次主逻辑重算平均耗时4.575us

从上面的结果，很容易就可以看出来，C++版本的性能大约是Python版本性能的16倍左右。这样的性能提升，尤其对于很多延迟敏感的策略来说，确实值得再基于WonderTrader做一个C++版本的策略了。

前面的DualThrust策略，其实就是一个基于C++实现的CTA策略。我们在使用C++开发策略的时候，主要参考解决方案中自带的以下几个工程：

WtCtaStraFact - CTA策略工厂工程

WtSelStraFact - SEL策略工厂工程

WtHftStraFact - HFT策略工厂工程

WtUftStraFact - UFT策略工厂工程

从上面就可以很容易看出来，策略工程都是采用的工厂模式来组织和实现的。工厂模式，是一种非常常用的设计模式，是一种创建对象的最佳方式。工厂模式主要有以下两种类：

工厂类，调用者创建对象时，只需要调用工厂类的创建接口，即可创建出对应的策略对象

产品类，在策略工厂的语境下，即策略类。工厂类在创建策略对象的时候，调用策略的构造函数

从上面的描述可以看出来，工厂类封装了不同策略的调用细节，返回给调用者一个统一的接口对象。这样的好处就是包括以下几点：

创建策略时，只需要只掉工厂创建策略时所需要的名称即可

很容易横向扩展，工厂模块可以扩展出很多个不同策略的实现，而接口调用部分不会受到任何影响

屏蔽了策略的具体实现，使用者只需要关心策略的通用接口即可

我们以WtCtaStraFact工程为例，其工厂类接口的定义如下：

从上面的代码可以看出来，除了工厂类接口的定义之外，还定义了工厂类接口创建和删除的函数指针，这两个函数指针主要是用于最终编译出来的模块中实现，这样外部程序加载策略工厂模块的时候，可以直接调用这两个接口进行工厂类对象的创建和销毁工作，实现的代码如下：

最后，我们再看一下策略工厂其他几个接口的实现代码：

综合前面的介绍，可以总结出C++策略工厂模块的调用流程：

首先，加载工厂模块xxxStraFact.dll(linux下为.so，下同)

调用工厂创建接口createStrategyFact，得到工厂对象IXXXStrategyFact的指针

调用工厂对象IXXXStrategyFact的接口createStrategy创建策略对象XXXStrategy的指针

调用策略对象XXXStrategy的init接口进行初始化

C++策略创建完成，将策略加入XXXEngine中，等待XXXEngine内部调度逻辑触发

前面介绍了C++策略在策略工厂中如何创建的，最终策略逻辑还是要在策略本身来实现。不同类型的策略，定义略有区别。 SEL策略的定义和CTA策略比较类似，以CTA策略为例，策略类定义如下：

HFT策略和UFT策略，因为直接和交易通道交互，所以策略的定义会更复杂一些。UFT策略的定义大致和HFT一样，以UFT策略为例，策略定义如下：

前面介绍了策略工厂以及不同类型的策略定义，也梳理了策略创建及初始化的基本流程。不过策略，最终还是要落在策略类本身的实现上。我们以WtCtaStraFact工厂下的StraDualThrust为例，前面已经介绍了策略的主体计算逻辑on_schedule，下面我们再来根据策略的接口调用顺序来分步介绍一下该策略的具体实现。

首先被调用的是策略的构造函数，这个比较简单，不会有太多额外的参数传入，代码如下：

策略创建成功以后，会调用策略的init接口，传入策略的参数，代码如下：

策略参数传入结束以后，则触发策略的on_init接口。这个on_init接口和init不同的地方在于：init是对象创建之后马上就会调用，主要用于传入策略参数；而on_init是策略运行流程中的一环，主要作用是让策略在这个接口中，处理一些数据订阅和数据准备的工作。一般来说，init是不涉及到策略运行的环境，所以并不给策略传入Context指针，而on_init是策略运行环境就绪以后才会触发。on_init代码如下：

策略初始化完成以后，会调用on_session_begin接口，如果是生产环境，每次启动就调用一次该接口。DualThrust策略实现中，在交易日开始的时候实现了一个主力合约换月的逻辑，代码如下：

调用完以上接口以后，策略已经处于一个就绪状态，这个时候则会根据策略订阅的主K线的频率来触发on_shedule接口，这个接口前面做不同版本的对比的时候已有提及，这里就不做赘述了。

如果在on_init订阅了tick数据，那么每个tick到来的时候还会触发on_tick接口。回测框架中，如果没有开启tick回测，则会使用K线的开高低收来模式4笔tick数据，并触发on_tick回调。代码如下：

最后每个交易日结束的时候，会触发on_session_end接口，这里可以放一些收盘作业的逻辑，保存一些用户自定义的数据。值得一提的是，如果这里增加了一些自定义数据的保存，那么千万不要忘记在on_session_begin里对保存的数据做一个加载和处理。

如果你觉得在on_session_begin和on_session_end两个接口处理自定义数据加载和保存比较麻烦的话，Context中还提供了stra_save_user_data和stra_load_user_data两个接口，框架底层会自动处理好数据保存和加载的逻辑，不需要再增加额外的代码。

实际上在使用C++编写策略时，策略需要调用的接口都封装到了Context对象中。这种模式的好处就是策略只需要跟Context打交道，无需在引入其他接口对象，如果要扩展接口的话，也只需要从Context入手即可。回测框架和实盘框架，只需要将Context做不同实现即可，对于策略来说，是不需要区分实盘和回测环境的。

此外，为了保证wtpy和WonderTrader使用上的一致性，wtpy也将Strategy+Context这种模式直接照搬了过去，并且统一了Python和C++中Context的接口名。

以下为ICtaStraCtx接口的部分定义：

前面已经将C++策略相关的内容都介绍了，最后我们来看一下，开发的具体流程：

首先根据目标策略复制一个工程，如WtCtaStraFact，并将工程名改成自己需要的工程名XXXStraFact

然后，修改工厂类的类名为自己需要的类名，如将WtStraFact改成XXXStraFact

第三，派生一个策略类，如StraXXXX，并完成策略的实现

第四，修改工厂类中的enumStrategy和createStrategy接口，将预定的策略名称和策略类关联起来

第五，修改sln解决方案，将新的策略工厂添加进去(linux下，需要修改CMakeLists.txt)

最后，编译该策略工厂模块，生成最终的模块文件XXXStraFact.dll(linux为libXXXStraFact.so)

经过以上步骤，生成的模块就可以进行调试了。可以使用WtBtRunner进行回测框架下的调试，也可以使用WtRunner进行实盘框架下的调试。 策略调试稳定以后，就可以编译release的版本，放到各种环境下使用新的策略工厂模块了。

C++开发策略的大致流程就介绍完了。这次介绍主要介绍的是基本模式和主要流程，实际上C++策略开发的细节很多，除了接口本身的使用之外，还有很多语言层面的细节，这些都是C++开发过程中难以绕开的问题。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
void WtStraDualThrust::on_schedule(ICtaStraCtx* ctx, uint32_t curDate, uint32_t curTime)
{
	std::string code = _code;

	WTSKlineSlice *kline = ctx->stra_get_bars(code.c_str(), _period.c_str(), _count, true);
	if(kline == NULL)
	{
		//这里可以输出一些日志
		return;
	}

	if (kline->size() == 0)
	{
		kline->release();
		return;
	}


	int32_t days = (int32_t)_days;

	double hh = kline->maxprice(-days, -2);
	double ll = kline->minprice(-days, -2);

	WTSValueArray* closes = kline->extractData(KFT_CLOSE);
	double hc = closes->maxvalue(-days, -2);
	double lc = closes->minvalue(-days, -2);
	double curPx = closes->at(-1);
	closes->release();///!!!这个释放一定要做

	double openPx = kline->at(-1)->open;

	double upper_bound = openPx + _k1 * (std::max(hh - lc, hc - ll));
	double lower_bound = openPx - _k2 * std::max(hh - lc, hc - ll);

	//设置指标值
	ctx->set_index_value("DualThrust", "upper_bound", upper_bound);
	ctx->set_index_value("DualThrust", "lower_bound", lower_bound);

	WTSCommodityInfo* commInfo = ctx->stra_get_comminfo(_code.c_str());

	double curPos = ctx->stra_get_position(_moncode.c_str()) / trdUnit;
	if(decimal::eq(curPos,0))
	{
		if(curPx >= upper_bound)
		{
			ctx->stra_enter_long(_moncode.c_str(), 1, "DT_EnterLong");
			//向上突破
			ctx->stra_log_info(fmt::format("向上突破{}>={},多仓进场", curPx, upper_bound).c_str());

			//添加图表标记
			ctx->add_chart_mark(curPx, "wt-mark-buy", "DT_EnterLong");
		}
		else if (curPx <= lower_bound && !_isstk)
		{
			ctx->stra_enter_short(_moncode.c_str(), 1, "DT_EnterShort");
			//向下突破
			ctx->stra_log_info(fmt::format("向下突破{}<={},空仓进场", curPx, lower_bound).c_str());
		}
	}
	//else if(curPos > 0)
	else if (decimal::gt(curPos, 0))
	{
		if(curPx <= lower_bound)
		{
			//多仓出场
			ctx->stra_exit_long(_moncode.c_str(), 1, "DT_ExitLong");
			ctx->stra_log_info(fmt::format("向下突破{}<={},多仓出场", curPx, lower_bound).c_str());
		}
	}
	//else if(curPos < 0)
	else if (decimal::lt(curPos, 0))
	{
		if (curPx >= upper_bound && !_isstk)
		{
			//空仓出场
			ctx->stra_exit_short(_moncode.c_str(), 1, "DT_ExitShort");
			ctx->stra_log_info(fmt::format("向上突破{}>={},空仓出场", curPx, upper_bound).c_str());
		}
	}

	//这个释放一定要做
	kline->release();
}
```

Example 2 (python):
```python
def on_calculate(self, context:CtaContext):
        code = self.__code__    #品种代码

        trdUnit = 1

        #读取最近50条1分钟线(dataframe对象)
        theCode = code
        if self.__is_stk__:
            theCode = theCode + "-" # 如果是股票代码，后面加上一个+/-，+表示后复权，-表示前复权
        df_bars = context.stra_get_bars(theCode, self.__period__, self.__bar_cnt__, isMain = True)

        #把策略参数读进来，作为临时变量，方便引用
        days = self.__days__
        k1 = self.__k1__
        k2 = self.__k2__

        #平仓价序列、最高价序列、最低价序列
        closes = df_bars.closes
        highs = df_bars.highs
        lows = df_bars.lows

        #读取days天之前到上一个交易日位置的数据
        hh = np.amax(highs[-days:-1])
        hc = np.amax(closes[-days:-1])
        ll = np.amin(lows[-days:-1])
        lc = np.amin(closes[-days:-1])

        #读取今天的开盘价、最高价和最低价
        # lastBar = df_bars.get_last_bar()
        openpx = df_bars.opens[-1]
        highpx = df_bars.highs[-1]
        lowpx = df_bars.lows[-1]

        '''
        !!!!!这里是重点
        1、首先根据最后一条K线的时间，计算当前的日期
        2、根据当前的日期，对日线进行切片,并截取所需条数
        3、最后在最终切片内计算所需数据
        '''

        #确定上轨和下轨
        upper_bound = openpx + k1* max(hh-lc,hc-ll)
        lower_bound = openpx - k2* max(hh-lc,hc-ll)

        #读取当前仓位
        curPos = context.stra_get_position(code)/trdUnit

        if curPos == 0:
            if highpx >= upper_bound:
                context.stra_enter_long(code, 1, 'enterlong')
                context.stra_log_text(f"向上突破{highpx:.2f}>={upper_bound:.2f}，多仓进场")
                return

            if lowpx <= lower_bound and not self.__is_stk__:
                context.stra_enter_short(code, 1, 'entershort')
                context.stra_log_text(f"向下突破{lowpx:.2f}<={lower_bound:.2f}，空仓进场")
                return
        elif curPos > 0:
            if lowpx <= lower_bound:
                context.stra_exit_long(code, 1, 'exitlong')
                context.stra_log_text(f"向下突破{lowpx:.2f}<={lower_bound:.2f}，多仓出场")
                return
        else:
            if highpx >= upper_bound and not self.__is_stk__:
                context.stra_exit_short(code, 1, 'exitshort')
                context.stra_log_text(f"向上突破{highpx:.2f}>={upper_bound:.2f}，空仓出场")
                return
```

Example 3 (javascript):
```javascript
//////////////////////////////////////////////////////////////////////////
//策略工厂接口
typedef void(*FuncEnumStrategyCallback)(const char* factName, const char* straName, bool isLast);

class ICtaStrategyFact
{
public:
	ICtaStrategyFact(){}
	virtual ~ICtaStrategyFact(){}

public:
	/*
	 *	获取工厂名
	 */
	virtual const char* getName() = 0;

	/*
	 *	枚举策略
	 */
	virtual void enumStrategy(FuncEnumStrategyCallback cb) = 0;

	/*
	 *	根据名称创建K线级别策略
	 */
	virtual CtaStrategy* createStrategy(const char* name, const char* id) = 0;


	/*
	 *	删除策略
	 */
	virtual bool deleteStrategy(CtaStrategy* stra) = 0;
};

//创建执行工厂
typedef ICtaStrategyFact* (*FuncCreateStraFact)();
//删除执行工厂
typedef void(*FuncDeleteStraFact)(ICtaStrategyFact* &fact);
```

Example 4 (unknown):
```unknown
extern "C"
{
	EXPORT_FLAG ICtaStrategyFact* createStrategyFact()
	{
		ICtaStrategyFact* fact = new WtStraFact();
		return fact;
	}

	EXPORT_FLAG void deleteStrategyFact(ICtaStrategyFact* fact)
	{
		if (fact != NULL)
			delete fact;
	}
};
```

---

## 经典跨日CTA策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/strategies/ctacrossdaystra.html

**Contents:**
- 经典跨日CTA策略
- 1.要实现的cta策略简介
  - 1.1均线策略
  - 1.2布林线
  - 1.3ATR通道
  - 1.4MACD策略
- 2.策略实现
  - 2.1 均线策略、布林带策略和ATR策略
  - 2.2MACD策略
- 3.策略回测

本文主要介绍几种趋势型cta跨日策略在wtpy上的实现过程，选择了四个策略：均线、布林线、ATR通道、MACD。

均线策略是一种既可以简单又可以复杂的策略，想要简单可以只用一条短均线突破长 均线作为买卖点，想要复杂可以叠加多周期均线或者多空头排列，我们在此仅以收盘价突 破 N 日均线作为策略基础，具体策略设定如下：

我们的均线策略就 2 个参数：均线周期 N、开仓线宽度 b。其中当 b=0 时，就是最原 始的均线突破，我们这里加了一个宽度参数 b 使得其成为一个通道策略，在寻找参数的过 程中可以同时尝试两种策略。

布林线策略我们也采用 2 个参数，具体设定如下：

1.上轨 = N 日均线 ＋ b × Std（N）

2.下轨 = N 日均线 － b × Std（N）

其中 N 为采用均线的周期，b 为布林带宽度。

ATR 通道策略与布林线策略也比较类似，但是其带宽计算采用 ATR 值。ATR 全称 Average True Range，一般称作平均真实波幅，由 J. Welles Wilder Jr 发明，可以用来衡量 价格的波动性。ATR 指标并不会指出市场波动的方向，仅仅以价格波动的幅度来表明市场 的波动性。Wilder 定义真实波动范围（TR）为以下的最大者：

当前交易日的最高价减去当前交易日的最低价。

当前交易日的最高价减去前一交易日收盘价的绝对值

当前交易日的最低价减去前一交易日收盘价的绝对值。

根据以上方法计算出的 TR（真实波幅）的 N 日平均值就是 ATR，ATR 指标是一个非常好的入场工具，它并不会告诉我们市场将会向哪个方向波动，但是可以告诉我们当前市场的 波动水平。根据 ATR得出的波动幅度我们可以鉴别出市场的横盘整理区间，当价格突破这个横盘整理区间的时候，市场很有可能形成了某种趋势，我们可以入场进行交易。根据 ATR 计算出来的横盘整理区间，我们获得了一个通道突破策略：

1.通道上轨 = N 日均价＋ N 日 ATR * b；

2.通道下轨 = N 日均价－ N 日 ATR * b；

其中均线以及 ATR 采用相同周期 N，b 为带宽参数。以上三个策略我们都只用到了 2 个类似的参数：周期、带宽。

MACD 也是最常见的技术指标之一，但是它所涉及计算相比于前三种策略略微复杂。通常 MACD 利用收盘价的短期（常用为 12 日）指数移动平均线与长期（常用为 26 日）指数移动平均线之间的聚合与分离状况，对买进、卖出时机作出研判的技术指标。其计算方法如下：

K1 日 EMA（K1） = 前一日 EMA（K1）×（K1－1）/（K1＋1） + 收盘价×2/(K1 ＋1)

K2 日 EMA（K2） = 前一日 EMA（K2）×（K2－1）/（K2＋1） + 收盘价×2/(K2 ＋1)

差离值 DIF = EMA（K1）－EMA（K2）

根据差离值 DIF 计算其 N 日的 EMA，即离差平均值，是所求的 DEA 值

今日 DEA = 前一日 DEA×（N－1）/（N＋1） + 今日 DIF×2/（N＋1）

MACD = 2 ×（DIF － DEA）；

DIF > 0 & MACD > 0 ，入场做多

DIF < 0 & MACD < 0 ，入场做空

DIF < 0 平多，DIF > 0 平空

在 MACD 策略中，我们有 3 个参数：K1 为快均线周期参数，K2 为慢均线周期参数， N 为 DEM 周期参数。

根据以上对于策略逻辑的简单介绍，不难发现前三个策略的交易逻辑几乎相同，使用的参数都为两个，且参数功能都是用于决定周期与通道宽度。策略之间的区别仅在于计算通道上下轨的过程不同，计算出了上下轨后，三个策略的交易逻辑都完全相同了。因此笔者将前三个策略写在一个py文件里，传入一个参数进行控制文件使用哪种策略的计算方法。

策略的交易逻辑需要在当日进行判断后，下一交易日才执行相应的交易行为，例如：收盘价突破上轨下一交易日做多、收盘价突破下轨下一交易日做空、单笔交易亏损 1%下一交易日平仓。在回测中，实现下一交易日开盘即进行交易需要在当前交易日的最后一根bar发出交易指令，即用5分钟线数据时，对于没有夜盘的品种，需要在14：55~15：00发出交易指令，这样程序就会在下一根bar的开盘执行交易。当策略在日内确认当前价格满足策略的开仓平仓条件时，记录一个self.trade_next_day，self.trade_next_day =-1，1，0分别代表了下一交易日做空、做多以及平仓。

这样这三个策略实现中会遇到的问题就基本解决了，文档最后也会给出完整的策略代码。

MACD策略的难点在于各项指标的计算方法要相对较为复杂，但在python中可以用5行代码比较简单地实现各个指标的计算，代码如下：

有了MACD的各项数据指标，根据这个指标进行的交易逻辑的代码也呼之欲出了：

以上代码实现了DIF > 0 & MACD > 0 ，入场做多、DIF < 0 & MACD < 0 ，入场做空、DIF < 0 平多，DIF > 0 平空、单笔交易亏损 1%下一交易日平仓的交易逻辑。

选用20140910~20190901的股指期货主力合约5分钟和日线进行回测

均线策略使用参数days=10，k=0.01的回测结果:

布林带策略使用参数days=10,k=0.5的回测结果：

ATR策略使用参数days=10，k=0.1的回测结果:

MACD策略使用经典的k1=12,k2=26,days=9的一组参数的回测结果：

以上策略的回测表现都比较一般，如果从2016年开始计算，除了ATR策略依靠最后阶段的表现有部分收益，其他三个策略都处于亏损状态。不过作为趋势型策略，在2015的牛市中都还是有比较不错的收益。

将策略代码的.py文件放入wtpy/demos/cta_fut_bt/strategies，然后修改runBT文件，传入相应的参数，即可自己进行策略的回测。

至此，在wtpy上实现一些cta策略的示例文档结束了，读者可以选其中感兴趣的策略自己回测一下，也可以参考其中的一些写法做自己想做的策略。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
# 在def __init__()中加入策略使用的参数
        self.__k__ = k  # n日均线加上k倍指标作为上轨和下轨 (AVG时，k是百分数如0.01)
        self.__days__ = days  # 取的均线天数
        self.__type__ = type  # 策略种类，有三种，AVG均线策略，BOLL布林带，ATR平均真实波幅
# 在def on_calculate()中根据type的种类分别计算通道上下轨
        if type not in ['AVG', 'BOLL', 'ATR']:
            print('输入的type错误,应输入AVG,BOLL,ATR中的一个')
            exit()
        if type == 'AVG':
            up_price = ma_days * (1+k)
            down_price = ma_days * (1-k)
        elif type == 'BOLL':
            std = np.std(closes[-days:-1])
            up_price = ma_days + k * std
            down_price = ma_days - k * std
        elif type == 'ATR':
            highs = df_bars.highs
            lows = df_bars.lows
            closes = df_bars.closes
            TR_SUM = 0
            for i in range(days):
                TR_SUM += max(highs[-1 - i] - lows[-1 - i], highs[-1 - i] - closes[-2 - i],
                              closes[-2 - i] - lows[-1 - i])
            ATR = TR_SUM / days
            up_price = ma_days + k * ATR
            down_price = ma_days - k * ATR
```

Example 2 (python):
```python
def on_session_begin(self, context:CtaContext, curTDate:int): # 写在def on_calculate()之前，每个交易日开始的时候会运行一次，重置self.trade_next_day的值
        self.trade_next_day = 2
```

Example 3 (unknown):
```unknown
# 当前未持仓且价格突破上轨，记录self.trade_next_day为1（做多）；当前未持仓且价格跌破下轨，记录self.trade_next_day为-1（做空），单笔交易亏损1%，记录self.trade_next_day为0（平仓）
    	if curPrice > up_price and curPos == 0:
            self.trade_next_day = 1
        elif curPrice < down_price and curPos == 0:
            self.trade_next_day = -1
        if cur_money < self.cur_money * 0.99 and curPos != 0:
            self.trade_next_day = 0
```

Example 4 (unknown):
```unknown
# 判断当前的交易时间是否在当日最后一根bar期间，然后用self.trade_next_day记录的下一交易日操作进行交易。每次开仓后记录开仓时的总资金self.cur_money，以便用于判断亏损是否超过1%
    	if curTime >= 1455 and curTime <= 1500:
            if self.trade_next_day == 1:
                context.stra_enter_long(code,math.floor(self.cur_money*money_pct/trdUnit_price)
                                        ,'enterlong')
                self.cur_money = capital + context.stra_get_fund_data(0)
                context.stra_log_text('下一交易日做多%s手'% (math.floor(self.cur_money*money_pct/trdUnit_price)))
            elif self.trade_next_day == -1:
                context.stra_enter_short(code,math.floor(self.cur_money*money_pct/trdUnit_price)
                                        ,'entershort')
                self.cur_money = capital + context.stra_get_fund_data(0)
                context.stra_log_text('下一交易日做空%s手'% (math.floor(self.cur_money*money_pct/trdUnit_price)))
            elif self.trade_next_day == 0:
                context.stra_set_position(code, 0, 'clear')
                context.stra_log_text('亏损超过百分之一，下一交易日平仓')
```

---

## SEL策略编写及回测

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/usage/selstra.html

**Contents:**
- SEL策略编写及回测
- 多引擎设计
- SEL策略概述
- SEL策略接口
- 一个简单的SEL策略

WonderTrader采用多引擎设计，针对不同类型的策略提供独立的引擎，目前支持的引擎包括：

CTA引擎，也叫同步策略引擎，一般适用于标的较少，计算逻辑较快的策略，事件+时间驱动。典型的应用场景包括单标的择时、中频以下的套利等。Demo中提供的DualThrust策略，单次重算平均耗时，Python实现版本约70多微秒，C++实现版本约4.5微秒。

SEL引擎，也叫异步策略引擎，一般适用于标的较多，计算逻辑耗时较长的策略，时间驱动。典型应用场景包括多因子选股策略、截面多空策略等。

HFT引擎，也叫高频策略引擎，主要针对高频或者低延时策略，事件驱动，系统延迟在1微秒之内

UFT引擎，也叫极速策略引擎，主要针对超高频或者超低延时策略，事件驱动，系统延迟在200纳秒之内

要基于WonderTrader实现SEL策略，有两种方式：

基于wtcpp，直接使用纯C++开发策略。这种方式适合有较高C++编程能力的用户

基于wtpy，使用python开发策略。这种方式适合对C++不大熟悉的用户

无论哪种方式，底层都是纯C++实现，执行效率有保障。 SEL引擎和CTA引擎的相同点：

SEL引擎和CTA引擎都采用M+1+N的执行架构，信号和执行完全剥离，支持同一个组合同时向多个交易通道执行

SEL引擎同时支持交易时间和非交易时间驱动on_calculate，而CTA引擎只支持交易时间驱动on_calculate

SEL引擎采用异步事件驱动的机制，而CTA引擎采用同步事件驱动的机制。如在某个时间节点T0，CTA引擎会把数据阻塞，直到所有策略的回调执行完成，再处理下一笔数据；而SEL引擎则不会阻塞数据，但是策略获取的数据，都是以T0时间为截止时间的。

通过上面可以对比可以发现：CTA引擎和SEL引擎在一定范围内是可以互相替换的，但是一旦组合的计算量变大，阻塞数据会导致很大的延迟从而影响理论撮合价格，那么SEL引擎就相对更合适一些。所以SEL引擎适用的策略场景大致有：

和CTA策略一样，SEL策略也有一个基础结构：

SelContext提供了相应的各种接口，更多信息可以参考API相关文档的介绍。

wtpy的demo中提供了一个简单的SEL策略，还是基于DualThrust做一个SEL的实现。

更多SEL相关信息可以参考wtpy/demos/sel_fut_bt。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
class BaseSelStrategy:
    '''
    选股策略基础类，所有的多因子策略都从该类派生
    包含了策略的基本开发框架
    '''
    def __init__(self, name:str):
        self.__name__ = name
        
    
    def name(self) -> str:
        return self.__name__


    def on_init(self, context:SelContext):
        '''
        策略初始化，启动的时候调用
        用于加载自定义数据

        @context    策略运行上下文
        '''
        return
    
    def on_session_begin(self, context:SelContext, curTDate:int):
        '''
        交易日开始事件

        @curTDate   交易日，格式为20210220
        '''
        return

    def on_session_end(self, context:SelContext, curTDate:int):
        '''
        交易日结束事件

        @curTDate   交易日，格式为20210220
        '''
        return
    
    def on_calculate(self, context:SelContext):
        '''
        K线闭合时调用，一般作为策略的核心计算模块
        @context    策略运行上下文
        '''
        return

    def on_calculate_done(self, context:SelContext):
        '''
        K线闭合时调用，一般作为策略的核心计算模块
        @context    策略运行上下文
        '''
        return

    def on_backtest_end(self, context:CtaContext):
        '''
        回测结束时回调，只在回测框架下会触发

        @context    策略上下文
        '''
        return

    def on_tick(self, context:SelContext, stdCode:str, newTick:dict):
        '''
        tick数据进来时调用
        生产环境中，每笔行情进来就直接调用
        回测环境中，是模拟的逐笔数据
        @context    策略运行上下文
        @stdCode    合约代码
        @newTick    最新逐笔
        '''
        return

    def on_bar(self, context:SelContext, stdCode:str, period:str, newBar:dict):
        '''
        K线闭合时回调
        @context    策略上下文
        @stdCode    合约代码
        @period     K线周期
        @newBar     最新闭合的K线
        '''
        return
```

Example 2 (python):
```python
from wtpy import BaseSelStrategy
from wtpy import SelContext
import numpy as np


class StraDualThrustSel(BaseSelStrategy):
    def __init__(self, name, codes:list, barCnt:int, period:str, days:int, k1:float, k2:float, isForStk:bool = False):
        BaseSelStrategy.__init__(self, name)

        self.__days__ = days
        self.__k1__ = k1
        self.__k2__ = k2

        self.__period__ = period
        self.__bar_cnt__ = barCnt
        self.__codes__ = codes

        self.__is_stk__ = isForStk
    

    def on_init(self, context:SelContext):
        return

    
    def on_calculate(self, context:SelContext):
        curTime = context.stra_get_time()
        trdUnit = 1
        if self.__is_stk__:
            trdUnit = 100

        for code in self.__codes__:
            sInfo = context.stra_get_sessioninfo(code)
            if not sInfo.isInTradingTime(curTime):
                continue

             #读取最近50条1分钟线(dataframe对象)
            theCode = code
            if self.__is_stk__:
                theCode = theCode + "Q"
            df_bars = context.stra_get_bars(theCode, self.__period__, self.__bar_cnt__)

            #把策略参数读进来，作为临时变量，方便引用
            days = self.__days__
            k1 = self.__k1__
            k2 = self.__k2__

            #平仓价序列、最高价序列、最低价序列
            closes = df_bars.closes
            highs = df_bars.highs
            lows = df_bars.lows

            #读取days天之前到上一个交易日位置的数据
            hh = np.amax(highs[-days:-1])
            hc = np.amax(closes[-days:-1])
            ll = np.amin(lows[-days:-1])
            lc = np.amin(closes[-days:-1])

            #读取今天的开盘价、最高价和最低价
            # lastBar = df_bars.get_last_bar()
            openpx = df_bars.opens[-1]
            highpx = df_bars.highs[-1]
            lowpx = df_bars.lows[-1]

            '''
            !!!!!这里是重点
            1、首先根据最后一条K线的时间，计算当前的日期
            2、根据当前的日期，对日线进行切片,并截取所需条数
            3、最后在最终切片内计算所需数据
            '''

            #确定上轨和下轨
            upper_bound = openpx + k1* max(hh-lc,hc-ll)
            lower_bound = openpx - k2* max(hh-lc,hc-ll)

            #读取当前仓位
            curPos = context.stra_get_position(code)/trdUnit

            if curPos == 0:
                if highpx >= upper_bound:
                    context.stra_set_position(code, 1*trdUnit, 'enterlong')
                    context.stra_log_text("{} 向上突破{}>={}，多仓进场".format(code, highpx, upper_bound))
                    continue

                if lowpx <= lower_bound and not self.__is_stk__:
                    context.stra_set_position(code, -1*trdUnit, 'entershort')
                    context.stra_log_text("{} 向下突破{}<={}，空仓进场".format(code, lowpx, lower_bound))
                    continue
            elif curPos > 0:
                if lowpx <= lower_bound:
                    context.stra_set_position(code, 0, 'exitlong')
                    context.stra_log_text("{} 向下突破{}<={}，多仓出场".format(code, lowpx, lower_bound))
                    #raise Exception("except on purpose")
                    continue
            else:
                if highpx >= upper_bound and not self.__is_stk__:
                    context.stra_set_position(code, 0, 'exitshort')
                    context.stra_log_text("{} 向上突破{}>={}，空仓出场".format(code, highpx, upper_bound))
                    continue


    def on_tick(self, context:SelContext, code:str, newTick:dict):
        return

    def on_bar(self, context:SelContext, code:str, period:str, newBar:dict):
        return
```

---

## 基于wtpy使用强化学习训练策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/levelup/rlfrm.html

**Contents:**
- 基于wtpy使用强化学习训练策略
- 为什么要搞强化学习
- Wt4ElegantRL

随着越来越多的量化团队，将机器学习运用到量化交易，量化平台对机器学习的适配也成了必然的趋势。正是基于这样一个契机，WonderTrader也针对强化学习做了一些适配，可以让强化学习框架很方便的基于wtpy进行智能体的训练。

强化学习（Reinforcement learning，RL）讨论的问题是一个智能体(agent) 怎么在一个复杂不确定的环境(environment) 里面去极大化它能获得的奖励。通过感知所处环境的 状态(state) 对动作(action) 的反应(reward)， 来指导更好的动作，从而获得最大的收益(return)，这被称为在交互中学习，这样的学习方法就被称作强化学习。

目前wtpy的强化学习适配，是用户自己适配的，一开始适配的是ElegantRL，后来拓展到了rllib和stable baseline3，感兴趣的可以去了解该项目:

https://github.com/drlgistics/Wt4ElegantRL

wtpy的demo也提供了演示的代码，如下：

更多详情可以参考demo: wtpy/demos/cta_fut_rl

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
import numpy as np

# from gym import Env
# from gym.spaces import Box

from wtpy.CtaContext import CtaContext
from wtpy.StrategyDefs import BaseCtaStrategy
from wtpy.WtBtEngine import WtBtEngine, EngineType

class EnvStrategy(BaseCtaStrategy):
    def __init__(self, name:str, code:str, period:str, count:int):
        super().__init__(name=name)

        self.__code__:str = code
        self.__period__:str = period
        self.__count__:int = count

        self.obs = 1
        self.reward = 1
        self.action = 0

    def on_init(self, context:CtaContext):
        #先订阅实时数据
        context.stra_get_bars(self.__code__, self.__period__, self.__count__, True)

    def on_calculate(self, context: CtaContext):
        print('on_calculate action%s'%self.action)

        # todo 输出 obs和reward 给外部
        self.obs += 1
        self.reward += 1

    def on_calculate_done(self, context: CtaContext):
        print('on_calculate_done action%s'%self.action)

    def on_backtest_end(self, context: CtaContext):
        print('on_backtest_end')



class WtEnv():
    def __init__(self) -> None:
        super().__init__()

        self._iter_ = 0
        self._strategy = None

        #创建一个运行环境
        self._engine_:WtBtEngine = WtBtEngine(EngineType.ET_CTA)
        self._engine_.init('../common/', "configbt.yaml")
        self._engine_.configBacktest(201909100930,201912011500)
        self._engine_.configBTStorage(mode="csv", path="../storage/")
        self._engine_.commitBTConfig()

    def reset(self) -> np.ndarray:
        self.close()
        self._iter_ += 1

        #创建一个策略，并加入运行环境
        self._strategy = EnvStrategy(name='EnvStrategy_%s'%self._iter_, code='CFFEX.IF.HOT', period='m5', count=60)
        # 设置策略的时候，一定要安装钩子
        self._engine_.set_cta_strategy(self._strategy, slippage=1, hook=True)
        # 回测一定要异步运行，不然这里不会返回，回测结束了才会返回
        self._engine_.run_backtest(bAsync=True)

        self._strategy.action = 0
        self._engine_.cta_step()

        #todo 怎么从on_calc里拿到obs和reward
        return self._strategy.obs

    def step(self, action:np.ndarray) -> tuple:
        # 单步触发oncalc
        bSucc = self._engine_.cta_step()

        obs = self._strategy.obs # todo 怎么从取得oncalc里的obs数据
        reward = self._strategy.reward # todo 怎么从取得reward里的obs数据
        done = True if np.random.randint(1, 100)==99 else False #是否结束
        done = not bSucc
        return obs, reward, done, {}
        print("state updated")

         #todo  怎么把action传入oncalc里
        self._strategy.action = action
        print("action updated, Go!")
        
        bSucc = self._engine_.cta_step()
        print("action executed")

        return obs, reward, done, {}
    
    def close(self) -> None:
        self._engine_.stop_backtest()

    def __del__(self):
        self._engine_.release_backtest()

if __name__ == '__main__':
    # env = DemoEnv()
    # done = False
    # obs = env.reset()
    # while not done:
    #     obs, reward, done, info = env.step(1)
    #     print(obs, reward, done, info)
    # env.close()

    env = WtEnv()
    for i in range(10): #模拟训练10次
        print('第%s次训练'%i)
        obs = env.reset()
        done = False
        action = 0
        while not done:
            action += 1 #模拟智能体产生动作
            obs, reward, done, info = env.step(action)
            print('obs%s'%obs, 'reward%s'%reward, done, info)
    env.close()
```

---

## 回测自己的策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/usage/mystrategy.html

**Contents:**
- 回测自己的策略
- 回测运行步骤
- 文件配置
  - configbt.yaml
- 编写自己的策略
- 回测自己的策略

根据策略配置文件（对于回测，commodities.json中一定要有回测的品种，contracts.json无所谓） a. common基础文件：品种与合约须手动检查，与runBT.py对应；其他的可以用demo的模板，详解见基础文件详解章节 b. storage数据：存放历史数据

将编写的策略.py放入Strategies文件夹下

runBT.py中的WtBtAnalyst模块会生成绩效分析.xlsx，可以据此分析优化策略（此模块目前仅适用CTA策略）

configbt.yaml中的配置是默认值，提供一个模版，可以在runBT.py中通过代码覆盖

确定策略模型 策略模块的API介绍见API详解章节。我们选择一个知名度相对较高的R-Breaker模型来进行编写。模型的原理可以参考如下文档： 解密并强化日内经典策略R-Breaker

R-Breaker的策略逻辑由以下4部分构成：

确定策略要用的参数 根据上述的策略逻辑，我们可以确定出策略要用到的几个基本参数：

K线追溯条数N，用于确定N日最高价、最低价、收盘价3个数据

观察价格系数a，用于计算观察买入价和卖出价

反转价格系数b、c，用于计算反转买入价和卖出价

突破价格系数d，用于计算突破买入价和卖出价

清仓截止时间cleartime，用于判定是否需要清仓出场

首先从demo里选择期货回测demo: cta_fut_bt

将策略模块RBreaker.py复制到strategies/目录下

修改runBT.py，设置好RBreaker策略的参数

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
common/  # 基础文件
	commodities.json  # 品种列表（可以用ctp_loader更新）
	contracts.json  # 合约列表（可以用ctp_loader更新）
	holidays.json  # 节假日列表
	hots.json  # 主力合约映射列表（主力合约才需要）
	sessions.json  # 交易时间模板
	fees.json  # 佣金配置文件
storage/  # 数据
	csv/
		历史K线数据.csv
	his/
		ticks/
			{交易所}/  # 如 SHFE/
				{交易日}/  # 如 20220325/
					{合约代码}.dsb  # 如 rb2210.dsb
cta_fut_bt/  # 策略回测
	runBT.py
	configbt.yaml  # 环境配置
	logcfgbt.yaml  # 日志配置
	Strategies/  # 存放各个策略py文件
		策略.py
	BtLogs/  #（运行后生成）日志
		Runner.log
		Strategy_{策略实例名}.log
	outputs_bt/   #（运行后生成）用于绩效分析的文件
		{策略实例名}/
```

Example 2 (unknown):
```unknown
replayer:
    basefiles:
        commodity: ../common/commodities.json   #品种列表
        contract: ../common/contracts.json      #合约列表
        holiday: ../common/holidays.json        #节假日列表
        hot: ../common/hots.json                #主力合约映射表
        session: ../common/sessions.json        #交易时间模板
    stime: 201909010900         #回测开始时间，精确到分钟
    etime: 201912011500         #回测结束时间，精确到分钟
    fees: ../common/fees.json   #佣金配置文件
    mode: csv   #回测历史数据存储，csv或者bin/wtp，其中bin/wtp都是一个意思
    store:
        module: WtDataStorage   #历史数据存储模块，如果是csv，该配置不生效
        path: ../storage/       #历史数据存储跟目录
env:
    mocker: cta                 #回测引擎，cta/sel/hft/exec/uf
```

Example 3 (unknown):
```unknown
1）计算6个目标价位
  根据昨日的开高低收价位计算出今日的6个目标价位，按照价格高低依次是：

  突破买入价（Bbreak）
  观察卖出价（Ssetup）
  反转卖出价（Senter）
  反转买入价（Benter）
  观察买入价（Bsetup）
  突破卖出价（Sbreak）
  他们的计算方法如下：（其中a、b、c、d为策略参数）

  观察卖出价（Ssetup）= High + a * (Close – Low)
  观察买入（Bsetup）= Low – a * (High – Close)
  反转卖出价（Senter）= b / 2 * (High + Low) – c * Low
  反转买入价（Benter）= b / 2 * (High + Low) – c * High
  突破卖出价（Sbreak）= Ssetup - d * (Ssetup – Bsetup)
  突破买入价（Bbreak）= Bsetup + d * (Ssetup – Bsetup)

  2）设计委托逻辑

  趋势策略情况：

  若价格>突破买入价，开仓做多；
  若价格<突破卖出价，开仓做空；
  反转策略情况：

  若日最高价>观察卖出价，然后下跌导致价格<反转卖出价，开仓做空或者反手（先平仓再反向开仓）做空；
  若日最低价<观察买入价，然后上涨导致价格>反转买入价，开仓做多或者反手（先平仓再反向开仓）做多；
  3）设定相应的止盈止损。

  4）日内策略要求收盘前平仓。
```

Example 4 (python):
```python
from wtpy import BaseCtaStrategy
from wtpy import CtaContext

class StraRBreaker(BaseCtaStrategy):

    def __init__(self, name:str, code:str, barCnt:int, period:str, N:int, a:float, b:float, c:float, d:float, cleartimes:list):
        BaseCtaStrategy.__init__(self, name)

        self.__N__ = N
        self.__a__ = a
        self.__b__ = b
        self.__c__ = c
        self.__d__ = d

        self.__period__ = period
        self.__bar_cnt__ = barCnt
        self.__code__ = code
        self.__cleartimes__ = cleartimes    # 尾盘清仓需要多个时间区间，因为夜盘和白盘都要清仓，格式如[[1455,1515],[2255,2300]]

    def on_init(self, context:CtaContext):
        code = self.__code__
        context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
        context.stra_log_text("R-Breaker inited")


    def on_calculate(self, context:CtaContext):
        code = self.__code__    #品种代码

        #读取当前仓位
        curPos = context.stra_get_position(code)
        curTime = context.stra_get_time()

        bCleared = False
        for tmPair in self.__cleartimes__:
            if curTime >= tmPair[0] and curTime <= tmPair[1]:
                if curPos != 0: # 如果持仓不为0，则要检查尾盘清仓
                    context.stra_set_positions(code, 0, "clear") # 清仓直接设置仓位为0
                    context.stra_log_text("尾盘清仓")
                bCleared = True
                break

        if bCleared:
            return

        df_bars = context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
        N = self.__N__
        a = self.__a__
        b = self.__b__
        c = self.__c__
        d = self.__d__

        #平仓价序列、最高价序列、最低价序列
        closes = df_bars["close"]
        highs = df_bars["high"]
        lows = df_bars["low"]

        #读取days天之前到上一个交易日位置的数据
        hh = highs[-N:].max()   #N条最高价
        ll = lows[-N:].min()    #N条最低价
        lc = closes.iloc[-1]    #最后收盘价

        Ssetup = hh + a * (lc - ll)
        Bsetup = ll - a * (hh - lc)
        Senter = b / 2 * (hh + ll) - c * ll
        Benter = b / 2 * (hh + ll) - c * hh
        Sbreak = Ssetup - d * (Ssetup - Bsetup)
        Bbreak = Bsetup + d * (Ssetup - Bsetup)

        curPx = lc  #最新价就是最后一个收盘价

        if curPos == 0:
            if curPx >= Bbreak:
                context.stra_enter_long(code, 1, 'rb-bbreak')
                context.stra_log_text("向上突破%.2f>=%.2f，多仓进场" % (curPx, Bbreak))
            elif curPx <= Sbreak:
                context.stra_enter_short(code, 1, 'rb-sbreak')
                context.stra_log_text("向下突破%.2f<=%.2f，空仓进场" % (curPx, Bbreak))
        elif curPos > 0:
            if curPx <= Senter:
                context.stra_enter_short(code, 1, 'rb-senter')
                context.stra_log_text("向下反转%.2f<=%.2f，多反空" % (curPx, Senter))
        elif curPos < 0:
            if curPx >= Benter:
                context.stra_enter_long(code, 1, 'rb-benter')
                context.stra_log_text("向上反转%.2f>=%.2f，空反多" % (curPx, Benter))


    def on_tick(self, context:CtaContext, code:str, newTick:dict):
        return

    def on_bar(self, context:CtaContext, code:str, period:str, newBar:dict):
        return
```

---

## 随机选股策略

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/strategies/random50.html

**Contents:**
- 随机选股策略
- 1.随机50交易法介绍
- 2.策略实现
- 3.回测结果

沪深300指数的成分股每年调整2次（每半年调整一次），一般是1月份和7月份进行调整，每次调整比例不超过10%。每日随机选取50只沪深300的成分股股票作为股票池等权重买入，在第二日重新随机选取50只沪深300成分股股票作为新的股票池。卖出不在新股票池内的股票，买入新股票池中的股票，若新股票池当中的股票已经买入则继续持有，保证每日固定持有50只股票。重复该过程。

若新股票池的股票已经买入，则继续持有，并对股票池进行更新。

第一步，由于沪深300成分股会发生变化，每日都要重新读取沪深300成分股的名单

第二步，生成50个不重复的随机数，确定当日的股票池，使用初始资金等权重买入

第三步，第二个交易日重新生成随机的新股票池，对股票池的持仓进行更新。保证每日持有50支股票。

写好了策略主体，将策略主体代码命名为Hushen300.py放入wtpy/demos/cta_stk_bt/strategies目录下，然后我们还需要runBT.py来让策略跑起来，runBT.py的格式和代码如下：

将runBT.py放入wtpy/demos/cta_stk_bt运行即可，其中的参数可以自行修改替换。这次是使用的沪深300成分股数据来进行回测，回测需要的日线数据可以通过下面的链接获取，然后放入wtpy/demos/storage/hushen300/csv文件夹中。

https://pan.baidu.com/s/12WJd3tMRBdOgsDXiiBA_sg?pwd=wtpy

打开运行之后生成的xlsx文件，可以看到策略的绩效，如下图所示：

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
self.__capital__ = capital # 起始资金
self.__period__ = period # 交易k线的时间级，如5分钟，1天
self.__bar_cnt__ = barCnt # 拉取的bar的次数
self.__codes__ = codes # 所有沪深300成分股股票代码
self.__codes2__ = [] #每日50股票池
```

Example 2 (python):
```python
class BaseSTKStrategy:
    '''
    STK策略基础类，所有的策略都从该类派生\n
    包含了策略的基本开发框架
    '''
    def __init__(self, name):
        self.__name__ = name

    def on_init(self, context:STKContext):
        '''
        策略初始化，启动的时候调用\n
        用于加载自定义数据\n
        @context    策略运行上下文
        '''
        return
        
    def on_calculate(self, context:Context):
        '''
        策略主调函数，所有的计算逻辑都在这里完成
        '''
        return
```

Example 3 (python):
```python
from wtpy import BaseCtaStrategy
from wtpy import CtaContext
import random
import pandas as pd

class StraHushenStk(BaseCtaStrategy):
    def __init__(self, name:str, codes:list, capital:float, barCnt:int, period:str):
        BaseCtaStrategy.__init__(self, name)

		self.__capital__ = capital # 起始资金
		self.__period__ = period # 交易k线的时间级，如5分钟，1天
		self.__bar_cnt__ = barCnt # 拉取的bar的次数
		self.__codes__ = codes # 所有沪深300成分股股票代码
		self.__codes2__ = [] # 每日50股票池

    def on_init(self, context:CtaContext):
        codes = self.__codes__    # 所有沪深300成分股股票代码
        codes2 = self.__codes2__ # 每日50股票池

        for i in range(0,len(codes)):
            if i == 0:
                context.stra_get_bars(codes[i], self.__period__, self.__bar_cnt__, isMain=True) # 设置第一支股票为主要品种
            else:
                context.stra_get_bars(codes[i], self.__period__, self.__bar_cnt__, isMain=False)

        context.stra_log_text("Hushen inited")

    
    def on_calculate(self, context:CtaContext):
        codes = self.__codes__    # 所有沪深300成分股股票代码
        codes2 = self.__codes2__  # 每日50股票池
        capital = self.__capital__ # 初始资金
        date = context.stra_get_date() # 获取当前日期

        #读取当日对应的沪深300成分股代码
        stocks = pd.read_csv('E:/沪深300历年成分股/{}.csv'.format(date), index_col=0)['order_book_id'].values 

        nums = [] # 股票池的数量
        while len(nums) < 50: # 每日固定生成50个不重复的随机数
            num = random.randint(0, len(stocks)-1)
            if num in nums: # 若随机数重复，跳过
                pass
            elif num not in nums: # 确保保存的随机数不重复
                stock = stocks[num]
                if stock[-1] == 'E': # 将深市股票代码转化为wtpy识别的股票代码
                    code = "SZSE.STK.{}".format(stock[:6])
                    curPrice = context.stra_get_price(code)
                    if curPrice > 0: # 确保股票当日价格数据正常
                        codes2.append(code) # 将股票放入股票池
                        nums.append(num) # 将随机数放入nums
                elif stock[-1] == 'G': # 将沪市股票代码转化为wtpy识别的股票代码
                    code = "SSE.STK.{}".format(stock[:6])
                    curPrice = context.stra_get_price(code)
                    if curPrice > 0: # 确保股票当日价格数据正常
                        codes2.append(code) # 将股票放入股票池
                        nums.append(num) # 将随机数放入nums

        for code in list(context.stra_get_all_position().keys()): # 遍历当前的持仓
            curPos = context.stra_get_position(code) # 获取每只股票的持仓头寸
            if code not in codes2 and curPos > 0: # 若当前持有的股票不在新生成的股票池内
                context.stra_exit_long(code, curPos, 'exitlong') # 平仓

        for code in codes2: # 遍历股票池
            curPos = context.stra_get_position(code) # 获取当前仓位
            curPrice = context.stra_get_price(code) # 获取当前价格
            if curPos == 0: # 若尚未持仓，买入该股票
                unit = int((200000/curPrice)/100) # 确定买入的数量
                context.stra_enter_long(code, unit, 'enterlong') # 买入unit手code
            elif curPos > 0: # 若已经持有该股票，则继续保留
                pass
        self.__codes2__ = [] # 当天交易结束后，将股票池清空
```

Example 4 (python):
```python
from wtpy import WtBtEngine,EngineType
from Strategies.Hushen import StraHushenStk
from wtpy.apps import WtBtAnalyst
import os
path = "E:/wtpy/deploy/storage/hushen300/csv"
datanames = os.listdir(path)
files = [string.replace('.csv','') for string in datanames]
files = [string.replace('_d','') for string in files]

if __name__ == "__main__":
    #创建一个运行环境，并加入策略
    engine = WtBtEngine(EngineType.ET_CTA)
    engine.init(folder='../common/', cfgfile="configbt.yaml", commfile="stk_comms.json", contractfile="stocks.json")
    engine.configBacktest(201101010930,202210241500)
    engine.configBTStorage(mode="csv", path="../storage/hushen300")
    engine.commitBTConfig()
    straInfo = StraHushenStk(name='Hushen', codes=files, barCnt=10, period="d", capital=10000000)
    engine.set_cta_strategy(straInfo)

    engine.run_backtest()

    #绩效分析
    analyst = WtBtAnalyst()
    analyst.add_strategy("Hushen", folder="./outputs_bt", init_capital=10000000, rf=0.02, annual_trading_days=240)
    analyst.run()

    kw = input('press any key to exit\n')
    engine.release_backtest()
```

---

## WtExecMon独立执行管理器

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/usage/wtexecmon.html

**Contents:**
- WtExecMon独立执行管理器
- WtExecMon是什么
- WtExecMon怎么用

在设计的时候，WonderTrader已经尽可能地考虑了各种场景下的不同需求，自上而下的架构设计，可以让WonderTrader很轻松的在不同的场景下适配起来。无论WonderTrader适应的场景多么全面，总是有WonderTrader不能覆盖的交易场景。

那么在WonderTrader中，是否可以从不同的场景中，设计一种共性的基础功能模块呢？答案是可以的，那就是交易执行。无论策略的实现方式是什么样的，最终都要通过交易接口下单。

WtExecMon独立执行管理器就是从WonderTrader中将执行模块单独剥离出来，封装成一个独立的模块。用户可以绕开CTA引擎、SEL引擎等，直接将最终的组合目标仓位丢给独立执行管理器。

WtExecMon会调用多账户执行器，并且针对不同标的创建不同的执行单元，不同标的的执行算法也通过线程池同步并发，最大限度的提升执行效率。

WtExecMon的使用比较简单，只需要启动以后，然后直接设置标的的目标仓位即可。调用代码如下：

更多信息可以参考demo: wtpy/demos/test_execmon。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
from wtpy import WtExecApi
import time

def test_exec_mon():
    api = WtExecApi()
    api.initialize(logCfg = "logcfgexec.yaml")
    api.config(cfgfile = 'cfgexec.yaml')
    api.run()

    time.sleep(10)
    api.set_position("CFFEX.IF.HOT", 1)

test_exec_mon()
```

---

## 策略模块详解

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/apis/structure.html

**Contents:**
- 策略模块详解
- 策略基本结构
- 回调函数详解

WonderTrader的策略非常简单，一共就4个接口函数on_init、on_bar、on_tick、on_calculate，核心逻辑只需要写入on_calculate里即可。 下面以CTA策略为例做介绍，所有策略的基类都类似，只是名字不同（BaseCtaStrategy、BaseHftStrategy、BaseSelStrategy）

为了详细的介绍各个接口函数的作用，我们以demo中的DualThrust策略为例，来参照介绍

初始化接口 on_init 初始化接口，是一个重要的接口，最大的作用是通过拉取系统所需要的历史K线，向系统注册一个主K线的闭合回调，以及tick数据的回调。

从上面的代码片段可以看到，DualThrust策略在初始化的时候，拉取了K线数据。WonderTrader里的K线数据都不用自己管理，开发策略的时候，直接调用接口就可以了。WonderTrader后台做了一系列的工作，保证K线处理的速度和准确度，用户不需要担心数据拷贝等细节问题。 调用了拉取历史K线的接口以后，系统就会自动注册对应K线的闭合事件回调以及tick数据回调，即激活了on_bar和on_tick事件。

K线闭合接口 on_bar K线闭合接口，K线闭合的时候系统会自动调用该接口。对于多条同周期K线的策略，比如配对交易，用到了两个关联品种的1分钟线K线。那么系统在1分钟线闭合的时候会自动触发两次on_bar事件，通知策略K线闭合。 如果策略对于K线闭合有特殊的响应，则可以重写该接口。K线闭合的闭合，一般来说按照on_init里读取K线数据的先后顺序来触发响应。如果主K线本轮也闭合了，当所有闭合K线回调完成以后，系统就会调用on_calculate接口触发策略主逻辑重算。

context是策略的环境，系统提供的所有接口都在context里

stdCode是标准品种代码，如CFFEX.IF.HOT，这个代码和用户使用的代码一致，即使是主力合约代码，也不会转换成分月合约，方便用户处理

period是K线周期，和用户拉取的K线周期一致，格式如m1、m3、d1

newBar是闭合的K线的数据，是一个dict，字段如下

tick回调接口 on_tick tick回调接口在收到新的tick数据的时候触发。对于使用分钟线的策略来说，一个交易日内K线会多次闭合触发on_bar。但是对于使用日线的策略来说，因为当日的K线并没有闭合，所以历史K线中是没有当日K线的数据的，这个时候就需要在on_tick中来读取最新交易日的数据了。

context是策略的环境，和on_bar一致的

stdCode是标准合约代码，同on_bar中的stdCode

newTick是最新的tick数据，是一个dict，字段如下

策略重算接口 on_calculate on_calculate是策略的核心逻辑所在，是整个策略的重中之重。on_calculate是以主K线的闭合来触发的。当主K线闭合后，并且所有的K线的闭合事件都已经回调以后，就会触发on_calculate进行主逻辑重算。context具体接口和用法我们在后面会详细介绍，这里就不作赘述了。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
from wtpy import Context

class BaseCtaStrategy:
    '''
    策略基础类，所有的策略都从该类派生\n
    包含了策略的基本开发框架
    '''
    def __init__(self, name):
        self.__name__ = name
        
    
    def name(self):
        return self.__name__


    def on_init(self, context:Context):
        '''
        策略初始化，启动的时候调用\n
        用于加载自定义数据\n
        @context    策略运行上下文
        '''
        return

    
    def on_calculate(self, context:Context):
        '''
        K线闭合时调用，一般作为策略的核心计算模块\n
        @context    策略运行上下文
        '''
        return


    def on_tick(self, context:Context, stdCode:str, newTick:dict):
        '''
        逐笔数据进来时调用\n
        生产环境中，每笔行情进来就直接调用\n
        回测环境中，是模拟的逐笔数据\n
        @context    策略运行上下文\n
        @code       合约代码
        @newTick    最新逐笔
        '''
        return

    def on_bar(self, context:Context, stdCode:str, period:str, newBar:dict):
        '''
        K线闭合时回调
        @context    策略上下文\n
        @code       合约代码
        @period     K线周期
        @newBar     最新闭合的K线
        '''
        return
```

Example 2 (python):
```python
from wtpy import BaseCtaStrategy
from wtpy import Context

class StraDualThrust(BaseCtaStrategy):
    
    def __init__(self, name:str, code:str, barCnt:int, period:str, days:int, k1:float, k2:float, isForStk:bool = False):
        BaseCtaStrategy.__init__(self, name)

        self.__days__ = days
        self.__k1__ = k1
        self.__k2__ = k2

        self.__period__ = period
        self.__bar_cnt__ = barCnt
        self.__code__ = code

        self.__is_stk__ = isForStk

    def on_init(self, context:Context):
        code = self.__code__    #品种代码
        if self.__is_stk__:
            code = code + "Q"

        #这里演示了品种信息获取的接口
        #pInfo = context.stra_get_comminfo(code)
        #print(pInfo)

        context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
        context.stra_log_text("DualThrust inited")

        #读取存储的数据
        self.xxx = context.user_load_data('xxx',1)

    
    def on_calculate(self, context:Context):
        code = self.__code__    #品种代码

        trdUnit = 1
        if self.__is_stk__:
            trdUnit = 100

        #读取最近50条1分钟线(dataframe对象)
        theCode = code
        if self.__is_stk__:
            theCode = theCode + "Q"
        df_bars = context.stra_get_bars(theCode, self.__period__, self.__bar_cnt__, isMain = True)

        #把策略参数读进来，作为临时变量，方便引用
        days = self.__days__
        k1 = self.__k1__
        k2 = self.__k2__

        #平仓价序列、最高价序列、最低价序列
        closes = df_bars["close"]
        highs = df_bars["high"]
        lows = df_bars["low"]

        #读取days天之前到上一个交易日位置的数据
        hh = highs[-days:-1].max()
        hc = closes[-days:-1].max()
        ll = lows[-days:-1].min()
        lc = closes[-days:-1].min()

        #读取今天的开盘价、最高价和最低价
        lastBar = df_bars.iloc[-1]
        openpx = lastBar["open"]
        highpx = lastBar["high"]
        lowpx = lastBar["low"]

        '''
        !!!!!这里是重点
        1、首先根据最后一条K线的时间，计算当前的日期
        2、根据当前的日期，对日线进行切片,并截取所需条数
        3、最后在最终切片内计算所需数据
        '''

        #确定上轨和下轨
        upper_bound = openpx + k1* max(hh-lc,hc-ll)
        lower_bound = openpx - k2* max(hh-lc,hc-ll)

        #读取当前仓位
        curPos = context.stra_get_position(code)/trdUnit

        if curPos == 0:
            if highpx >= upper_bound:
                context.stra_enter_long(code, 1*trdUnit, 'enterlong')
                context.stra_log_text("向上突破%.2f>=%.2f，多仓进场" % (highpx, upper_bound))
                #修改并保存
                self.xxx = 1
                context.user_save_data('xxx', self.xxx)
                return

            if lowpx <= lower_bound and not self.__is_stk__:
                context.stra_enter_short(code, 1*trdUnit, 'entershort')
                context.stra_log_text("向下突破%.2f<=%.2f，空仓进场" % (lowpx, lower_bound))
                return
        elif curPos > 0:
            if lowpx <= lower_bound:
                context.stra_exit_long(code, 1*trdUnit, 'exitlong')
                context.stra_log_text("向下突破%.2f<=%.2f，多仓出场" % (lowpx, lower_bound))
                #raise Exception("except on purpose")
                return
        else:
            if highpx >= upper_bound and not self.__is_stk__:
                context.stra_exit_short(code, 1*trdUnit, 'exitshort')
                context.stra_log_text("向上突破%.2f>=%.2f，空仓出场" % (highpx, upper_bound))
                return


    def on_tick(self, context:Context, stdCode:str, newTick:dict):
        return

    def on_bar(self, context:Context, stdCode:str, period:str, newBar:dict):
        return
```

Example 3 (python):
```python
def on_init(self, context:Context):
    code = self.__code__    #品种代码
    if self.__is_stk__:
        code = code + "Q"

    context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
    context.stra_log_text("DualThrust inited")
```

Example 4 (python):
```python
def on_bar(self, context:Context, stdCode:str, period:str, newBar:dict):
    return
```

---
