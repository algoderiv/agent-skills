# Wtpy-Zz - Source Reading

**Pages:** 46

---

## 常见问题

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/常见问题/

**Contents:**
- 常见问题

---

## 概念问题

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98/%E6%A6%82%E5%BF%B5%E8%A7%A3%E9%87%8A.html

**Contents:**
- 概念问题
- 信号与执行

信号可以理解为是一种要求，即策略要求在某种情况下，账户需要要达到某种持仓状态，而执行便是具体使仓位符合要求的一系列具体动作，包括包括发单、撤单、追单等等。

在vnpy或掘金等事件驱动类型的交易品台中，策略开发者需要同时管理信号与执行，而WT则将信号与执行进行了解耦，策略仅负责信号的计算与发出，具体的执行由执行器负责。具体流程如下(图片由@Praying绘制)：

以内置的WtMinImpactExeUnit（最小冲击执行单元）为例，只需要告诉执行器需要达到的目标持仓即可，内部会自动进行发单，追单，撤单等操作。

---

## 上下文

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/4.UFT%E5%BC%95%E6%93%8E/%E4%B8%8A%E4%B8%8B%E6%96%87.html

**Contents:**
- 上下文

---

## 事件函数

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/3.SEL%E5%BC%95%E6%93%8E/%E4%BA%8B%E4%BB%B6%E5%87%BD%E6%95%B0.html

**Contents:**
- 事件函数

---

## 编译问题

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98/%E7%BC%96%E8%AF%91%E9%97%AE%E9%A2%98.html

**Contents:**
- 编译问题
- 1. C2589 "(":"::"右边的非法标记
  - 报错内容
  - 解决方案
- 2. wtuftcore这个项目报未定义标识符错误
  - 报错内容
  - 解决方案

Visual C++ 在windows下min和max与<windows.h>中传统的min/max宏定义有冲突。会导致无法使用。为了禁用Visual C++中的 min/max宏定义，可以在包含<windows.h>头文件之前加上：NOMINMAX，或者在预处理中添加。

在TraderAdapter.cpp添加using namespace std;即可

---

## 源码解析

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/

**Contents:**
- 源码解析

CPP源码解析，配合理解WonderTrader的逻辑流程

---

## ExecuteUnit

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/7.ExecuteUnit.html

**Contents:**
- ExecuteUnit

---

## HFT引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/2.HFT%E5%BC%95%E6%93%8E/

**Contents:**
- HFT引擎

HFT引擎，也叫高频策略引擎，主要针对高频或者低延时策略，事件驱动，系统延迟在1-2微秒之间

---

## 事件函数

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/1.CTA%E5%BC%95%E6%93%8E/%E4%BA%8B%E4%BB%B6%E5%87%BD%E6%95%B0.html

**Contents:**
- 事件函数
- __init__
- on_init
- on_session_begin
- on_session_end
- on_tick
- on_bar
- on_calculate
- on_calculate_done

学过Python的小伙伴都知道，Python中类在被创建时，会调用__init__函数，来接受类创建时传入的各个参数。WT中的策略其实是一个继承自BaseCtaStrategy类的子类。因此当他被创建时，便会触发__init__函数，用来传入参数等信息。

以demo中使用的DualThrust.py为例：

注意，我们策略中所有的回调函数，包括__init__，均是对父类的重写。包括接受的参数与内部逻辑在内，均可以自己重写，切勿被demo中的__init__误导，误以为wt中的策略必须要接受这些参数。__init__建议接收的参数只有name，其他的均可以自己决定。

在回测中，on_init是在数据回放器准备好回测数据后执行，而实盘中，则是连接到行情服务后执行。on_init的主要功能是订阅行情数据，加载外部数据等。

__init__是策略初始化时触发的，是策略第一个被触发的函数。on_init是回测数据或行情服务准备好后触发的，是策略在接受数据前最后一个触发的函数。

逐笔数据进来时调用，生产环境中，每笔行情进来就直接调用，回测环境中，是模拟的逐笔数据

K线闭合时调用，一般作为策略的核心计算模块

on_calculate与on_bar在仅仅订阅一个品种时效果相同，但当订阅了多个品种时，存在以下区别：on_bar在每个品种K线闭合时都会触发一次。on_calculate在所有订阅的品种都触发一次on_bar后才会触发。这样做的好处是在处理截面策略时，能够保证行情数据能够对齐。

强化学习中使用，因为强化学习需要现在on_calculate获取view然后传给框架，最后在on_calculate中发出信号，而一般策略只需要使用on_calculate即可。

**Examples:**

Example 1 (python):
```python
# 继承策略基类BaseCtaStrategy
class StraDualThrust(BaseCtaStrategy):
    # 重写初始化代码，接受的参数可以自己决定
    def __init__(self, name:str, code:str, barCnt:int, period:str, days:int, k1:float, k2:float, isForStk:bool = False):
        # 初始化父类，name参数表示该策略的名称
        BaseCtaStrategy.__init__(self, name)

        # 接受参数，并把它赋值给自己的成员
        self.__days__ = days
        self.__k1__ = k1
        self.__k2__ = k2

        self.__period__ = period
        self.__bar_cnt__ = barCnt
        self.__code__ = code

        self.__is_stk__ = isForStk
```

Example 2 (python):
```python
# 继承策略基类BaseCtaStrategy
class StraDualThrust(BaseCtaStrategy):
    # 重写初始化代码，接受的参数可以自己决定
    def __init__(self, name:str, code:str, barCnt:int, period:str, days:int, k1:float, k2:float, isForStk:bool = False):
        # 初始化父类，name参数表示该策略的名称
        BaseCtaStrategy.__init__(self, name)

        # 接受参数，并把它赋值给自己的成员
        self.__days__ = days
        self.__k1__ = k1
        self.__k2__ = k2

        self.__period__ = period
        self.__bar_cnt__ = barCnt
        self.__code__ = code

        self.__is_stk__ = isForStk
```

Example 3 (python):
```python
def on_init(self, context:CtaContext):
    code = self.__code__    #品种代码
    if self.__is_stk__:
        code = code + "Q"

    #这里演示了品种信息获取的接口
    pInfo = context.stra_get_comminfo(code)
    print(pInfo)

    # 订阅K线
    context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
    # 订阅Tick
    context.stra_sub_ticks(code)

    context.stra_log_text("DualThrust inited")
```

Example 4 (python):
```python
def on_init(self, context:CtaContext):
    code = self.__code__    #品种代码
    if self.__is_stk__:
        code = code + "Q"

    #这里演示了品种信息获取的接口
    pInfo = context.stra_get_comminfo(code)
    print(pInfo)

    # 订阅K线
    context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
    # 订阅Tick
    context.stra_sub_ticks(code)

    context.stra_log_text("DualThrust inited")
```

---

## DataKit

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/2.DataKit.html

**Contents:**
- DataKit

---

## 为什么要写笔记

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/1.%E5%89%8D%E8%A8%80/1.%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%86%99%E7%AC%94%E8%AE%B0.html

**Contents:**
- 为什么要写笔记

在我学习WonderTrader时，我发现由于项目采用C++语言开发，阅读源码的难度较大，而项目处于起步初期，相关资料较少，这两点导致WonderTrader(后面简称WT)的学习曲线非常陡峭。在和作者 @wondertrader 交流后，决定记录我自己学习WonderTrader的过程，让希望学习WonderTrader的大家能够少走一些弯路。

---

## Parsers

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/10.Parsers.html

**Contents:**
- Parsers

---

## dataFeed

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E5%B7%A5%E5%85%B7%E9%9B%86/dataFeed.html

**Contents:**
- dataFeed

datakit提供了录制实时行情的功能，但如果需要从其他数据源中获取历史行情，则需要借助dataFeed的帮助。

dataFeed代码中提供了一个基类Ifeed，Ifeed中实现了代码转换、目录结构处理等功能，但没有实现get_tick与get_bar方法，这个是需要我们自己定义的子类实现的。

下面以从米筐获取历史数据为例，讲解如何通过继承Ifeed来实现历史数据获取。

补充一个从csv中加载的demo，从csv中获取数据的代码略有不同，主要是接口层面的

继承Ifeed的子类主要需要实现get_tick与get_bar两个功能，在这两个功能中，需要返回wt格式的DataFarme有一下需要注意的细节：

**Examples:**

Example 1 (python):
```python
import rqdatac as rq
from wtpy.WtCoreDefs import WTSBarStruct, WTSTickStruct
from wtpy.wrapper import WtDataHelper
import pandas as pd
from tqdm import tqdm
import os
class Ifeed(object):
    def __init__(self):
        self.dthelper = WtDataHelper()
        self.period_map = {"m1":"min1","m5":"min5","d":"day","tick":"ticks"}
        self.frequency_map = {
            "m1":"1m",
            "m5":"5m",
            "d":"1d",
        }
    
    def get_tick(self,symbol,start_date=None,end_date=None):
        return
    
    def get_bar(self,symbol,frequency,start_date=None,end_date=None):
        return
    
    def parse_code(self,code):
        items = code.split(".")
        return items[0],items[1],items[2]

    def code_std(self,stdCode:str):
        stdCode = stdCode.upper()
        items = stdCode.split(".")
        exchg = self.exchgStdToRQ(items[0])
        if len(items) == 2:
            # 简单股票代码，格式如SSE.600000
            return items[1] + "." + exchg
        elif items[1] in ["IDX","ETF","STK","OPT"]:
            # 标准股票代码，格式如SSE.IDX.000001
            return items[2] + "." + exchg
        elif len(items) == 3:
            # 标准期货代码，格式如CFFEX.IF.2103
            if items[2] != 'HOT':
                return ''.join(items[1:])
            else:
                return items[1] + "88"
            
    def cover_d_bar(self,df):
        count = len(df)
        BUFFER = WTSBarStruct * count
        buffer = BUFFER()
        for index, row in tqdm(df.iterrows()):
            curBar = buffer[index]
            curBar.date = int(row["date"])
            curBar.open = float(row["open"])
            curBar.high = float(row["high"])
            curBar.low = float(row["low"])
            curBar.close = float(row["close"])
            curBar.vol = float(row["vol"])
            curBar.money = float(row["money"])
            curBar.hold = float(row["hold"])
        return buffer
    
    def cover_m_bar(self,df):
        count = len(df)
        BUFFER = WTSBarStruct * count
        buffer = BUFFER()
        for index, row in tqdm(df.iterrows()):
            curBar = buffer[index]
            curBar.time = int((int(row["date"])-19900000)*10000 + int(row["time"]))
            curBar.open = float(row["open"])
            curBar.high = float(row["high"])
            curBar.low = float(row["low"])
            curBar.close = float(row["close"])
            curBar.vol = float(row["vol"])
            curBar.money = float(row["money"])
            curBar.hold = float(row["hold"])
        return buffer
        
    def cover_tick(self,df):
        count = len(df)
        BUFFER = WTSTickStruct * count
        buffer = BUFFER()
        for index, row in tqdm(df.iterrows()):
            curTick = buffer[index]
            curTick.exchg = bytes(row["exchg"],'utf-8')
            curTick.code = bytes(row["code"],'utf-8')
            curTick.price = float(row["price"])
            curTick.open = float(row["open"])
            curTick.high = float(row["high"])
            curTick.low = float(row["low"])
            curTick.settle_price = float(row["settle_price"])
            curTick.total_volume = float(row["total_volume"])
            curTick.volume = float(row["volume"])
            curTick.total_turnover = float(row["total_turnover"])
            curTick.turn_over = float(row["turn_over"])
            curTick.open_interest = float(row["open_interest"])
            curTick.diff_interest = float(row["diff_interest"])
            curTick.trading_date = int(row["trading_date"])
            curTick.action_date = int(row["action_date"])
            curTick.action_time = int(int(row["action_time"]) / 1000)
            curTick.pre_close = float(row["pre_close"])
            curTick.pre_settle = float(row["pre_settle"])
            curTick.pre_interest = float(0.0)
            for x in range(0,5):
                setattr(curTick,f"bid_price_{x}",float(row["bid_" + str(x+1)]))
                setattr(curTick,f"bid_qty_{x}",float(row["bid_qty_" + str(x+1)]))
                setattr(curTick,f"ask_prices_{x}",float(row["ask_" + str(x+1)]))
                setattr(curTick,f"ask_qty_{x}",float(row["ask_qty_" + str(x+1)]))
        return buffer
        
    def bar_df_to_dsb(self,df,dsb_file,period):
        if "d" in period:
            buffer = self.cover_d_bar(df)
        elif "m" in period:
            buffer = self.cover_m_bar(df)      
        self.dthelper.store_bars(barFile=dsb_file,firstBar=buffer,count=len(buffer),period=period)

    def tick_df_to_dsb(self,df,dsb_file):
        buffer = self.cover_tick(df)
        self.dthelper.store_ticks(tickFile=dsb_file, firstTick=buffer, count=len(buffer))
        
    # 新下的数据会覆盖旧的数据
    def store_bin_bar(self,storage_path,code,start_date=None,end_date=None,frequency="1m",col_map=None):
        df = self.get_bar(code,start_date,end_date,frequency)
        period = self.period_map[frequency]
        save_path = os.path.join(storage_path,"bin",period)
        if not os.path.exists(save_path):
            os.makedirs(save_path)
        dsb_path = os.path.join(save_path,f"{code}_{frequency}.dsb")
        self.bar_df_to_dsb(df,dsb_path,frequency)
        
    def store_bin_tick(self,storage_path,code,start_date=None,end_date=None,col_map=None):
        df = self.get_tick(code,start_date,end_date)
        save_path = os.path.join(storage_path,"bin","ticks")
        if not os.path.exists(save_path):
            os.makedirs(save_path)
        g = df.groupby("trading_date")
        for trading_date,g_df in g:
            g_df = g_df.reset_index()
            dsb_path = os.path.join(save_path,f"{code}_tick_{trading_date}.dsb")
            self.tick_df_to_dsb(g_df,dsb_path)
    
    # 除了转换为dsb格式，还会按照his的格式进行存储
    def store_his_bar(self,storage_path,code,start_date=None,end_date=None,frequency="1m",skip_saved=False):
        print(f"开始转存{code}")
        exchange,pid,month = self.parse_code(code)
        if exchange == "CZCE":
            month = month[-3:]
        if frequency not in self.frequency_map.keys():
            print("周期只能为m1、m5或d,回测或实盘中会自动拼接")
        period = self.period_map[frequency]
        save_path = os.path.join(storage_path,"his",period,exchange)
        if not os.path.exists(save_path):
                try:
                    os.makedirs(save_path)
                except:
                    pass
        dsb_name = f"{pid}{month}.dsb"
        dsb_path = os.path.join(save_path,dsb_name)
        if skip_saved:
            saved_list = os.listdir(save_path)
            if dsb_name in saved_list:
                print(f"重复数据，跳过{dsb_name}")
                return
        df = self.get_bar(code,start_date,end_date,frequency)
        if df is None:
            print(f"{code}没有数据")
            return
        self.bar_df_to_dsb(df,dsb_path,frequency)
        
    def store_his_tick(self,storage_path,code,start_date=None,end_date=None,skip_saved=False):
        print(f"开始转存{code}")
        exchange,pid,month = self.parse_code(code)
        if exchange == "CZCE":
            month = month[-3:]
        # 分天下载，避免内存超出
        for date in pd.date_range(start_date,end_date):
            save_path = os.path.join(storage_path,"his","ticks",exchange,date.strftime('%Y%m%d'))
            if not os.path.exists(save_path):
                try:
                    os.makedirs(save_path)
                except:
                    pass
            dsb_name = f"{pid}{month}.dsb"
            if skip_saved:
                saved_list = os.listdir(save_path)
                if dsb_name in saved_list:
                    print(f"重复数据，跳过{dsb_name}")
                    continue
            t_day = date.strftime('%Y.%m.%d')
            df = self.get_tick(code,t_day,t_day)
            if (df is None) or (df.empty):
                print(f"{date}:{code}没有数据")
                continue
            dsb_path = os.path.join(save_path,f"{pid}{month}.dsb")
            self.tick_df_to_dsb(df,dsb_path)
            
    def bar_dsb_to_csv(self,binFolder,csvFolder):
        self.dthelper.dump_bars(binFolder, csvFolder)
    
    def tick_dsb_to_csv(self,binFolder,csvFolder):
        self.dthelper.dump_ticks(binFolder, csvFolder)
```

Example 2 (python):
```python
import rqdatac as rq
from wtpy.WtCoreDefs import WTSBarStruct, WTSTickStruct
from wtpy.wrapper import WtDataHelper
import pandas as pd
from tqdm import tqdm
import os
class Ifeed(object):
    def __init__(self):
        self.dthelper = WtDataHelper()
        self.period_map = {"m1":"min1","m5":"min5","d":"day","tick":"ticks"}
        self.frequency_map = {
            "m1":"1m",
            "m5":"5m",
            "d":"1d",
        }
    
    def get_tick(self,symbol,start_date=None,end_date=None):
        return
    
    def get_bar(self,symbol,frequency,start_date=None,end_date=None):
        return
    
    def parse_code(self,code):
        items = code.split(".")
        return items[0],items[1],items[2]

    def code_std(self,stdCode:str):
        stdCode = stdCode.upper()
        items = stdCode.split(".")
        exchg = self.exchgStdToRQ(items[0])
        if len(items) == 2:
            # 简单股票代码，格式如SSE.600000
            return items[1] + "." + exchg
        elif items[1] in ["IDX","ETF","STK","OPT"]:
            # 标准股票代码，格式如SSE.IDX.000001
            return items[2] + "." + exchg
        elif len(items) == 3:
            # 标准期货代码，格式如CFFEX.IF.2103
            if items[2] != 'HOT':
                return ''.join(items[1:])
            else:
                return items[1] + "88"
            
    def cover_d_bar(self,df):
        count = len(df)
        BUFFER = WTSBarStruct * count
        buffer = BUFFER()
        for index, row in tqdm(df.iterrows()):
            curBar = buffer[index]
            curBar.date = int(row["date"])
            curBar.open = float(row["open"])
            curBar.high = float(row["high"])
            curBar.low = float(row["low"])
            curBar.close = float(row["close"])
            curBar.vol = float(row["vol"])
            curBar.money = float(row["money"])
            curBar.hold = float(row["hold"])
        return buffer
    
    def cover_m_bar(self,df):
        count = len(df)
        BUFFER = WTSBarStruct * count
        buffer = BUFFER()
        for index, row in tqdm(df.iterrows()):
            curBar = buffer[index]
            curBar.time = int((int(row["date"])-19900000)*10000 + int(row["time"]))
            curBar.open = float(row["open"])
            curBar.high = float(row["high"])
            curBar.low = float(row["low"])
            curBar.close = float(row["close"])
            curBar.vol = float(row["vol"])
            curBar.money = float(row["money"])
            curBar.hold = float(row["hold"])
        return buffer
        
    def cover_tick(self,df):
        count = len(df)
        BUFFER = WTSTickStruct * count
        buffer = BUFFER()
        for index, row in tqdm(df.iterrows()):
            curTick = buffer[index]
            curTick.exchg = bytes(row["exchg"],'utf-8')
            curTick.code = bytes(row["code"],'utf-8')
            curTick.price = float(row["price"])
            curTick.open = float(row["open"])
            curTick.high = float(row["high"])
            curTick.low = float(row["low"])
            curTick.settle_price = float(row["settle_price"])
            curTick.total_volume = float(row["total_volume"])
            curTick.volume = float(row["volume"])
            curTick.total_turnover = float(row["total_turnover"])
            curTick.turn_over = float(row["turn_over"])
            curTick.open_interest = float(row["open_interest"])
            curTick.diff_interest = float(row["diff_interest"])
            curTick.trading_date = int(row["trading_date"])
            curTick.action_date = int(row["action_date"])
            curTick.action_time = int(int(row["action_time"]) / 1000)
            curTick.pre_close = float(row["pre_close"])
            curTick.pre_settle = float(row["pre_settle"])
            curTick.pre_interest = float(0.0)
            for x in range(0,5):
                setattr(curTick,f"bid_price_{x}",float(row["bid_" + str(x+1)]))
                setattr(curTick,f"bid_qty_{x}",float(row["bid_qty_" + str(x+1)]))
                setattr(curTick,f"ask_prices_{x}",float(row["ask_" + str(x+1)]))
                setattr(curTick,f"ask_qty_{x}",float(row["ask_qty_" + str(x+1)]))
        return buffer
        
    def bar_df_to_dsb(self,df,dsb_file,period):
        if "d" in period:
            buffer = self.cover_d_bar(df)
        elif "m" in period:
            buffer = self.cover_m_bar(df)      
        self.dthelper.store_bars(barFile=dsb_file,firstBar=buffer,count=len(buffer),period=period)

    def tick_df_to_dsb(self,df,dsb_file):
        buffer = self.cover_tick(df)
        self.dthelper.store_ticks(tickFile=dsb_file, firstTick=buffer, count=len(buffer))
        
    # 新下的数据会覆盖旧的数据
    def store_bin_bar(self,storage_path,code,start_date=None,end_date=None,frequency="1m",col_map=None):
        df = self.get_bar(code,start_date,end_date,frequency)
        period = self.period_map[frequency]
        save_path = os.path.join(storage_path,"bin",period)
        if not os.path.exists(save_path):
            os.makedirs(save_path)
        dsb_path = os.path.join(save_path,f"{code}_{frequency}.dsb")
        self.bar_df_to_dsb(df,dsb_path,frequency)
        
    def store_bin_tick(self,storage_path,code,start_date=None,end_date=None,col_map=None):
        df = self.get_tick(code,start_date,end_date)
        save_path = os.path.join(storage_path,"bin","ticks")
        if not os.path.exists(save_path):
            os.makedirs(save_path)
        g = df.groupby("trading_date")
        for trading_date,g_df in g:
            g_df = g_df.reset_index()
            dsb_path = os.path.join(save_path,f"{code}_tick_{trading_date}.dsb")
            self.tick_df_to_dsb(g_df,dsb_path)
    
    # 除了转换为dsb格式，还会按照his的格式进行存储
    def store_his_bar(self,storage_path,code,start_date=None,end_date=None,frequency="1m",skip_saved=False):
        print(f"开始转存{code}")
        exchange,pid,month = self.parse_code(code)
        if exchange == "CZCE":
            month = month[-3:]
        if frequency not in self.frequency_map.keys():
            print("周期只能为m1、m5或d,回测或实盘中会自动拼接")
        period = self.period_map[frequency]
        save_path = os.path.join(storage_path,"his",period,exchange)
        if not os.path.exists(save_path):
                try:
                    os.makedirs(save_path)
                except:
                    pass
        dsb_name = f"{pid}{month}.dsb"
        dsb_path = os.path.join(save_path,dsb_name)
        if skip_saved:
            saved_list = os.listdir(save_path)
            if dsb_name in saved_list:
                print(f"重复数据，跳过{dsb_name}")
                return
        df = self.get_bar(code,start_date,end_date,frequency)
        if df is None:
            print(f"{code}没有数据")
            return
        self.bar_df_to_dsb(df,dsb_path,frequency)
        
    def store_his_tick(self,storage_path,code,start_date=None,end_date=None,skip_saved=False):
        print(f"开始转存{code}")
        exchange,pid,month = self.parse_code(code)
        if exchange == "CZCE":
            month = month[-3:]
        # 分天下载，避免内存超出
        for date in pd.date_range(start_date,end_date):
            save_path = os.path.join(storage_path,"his","ticks",exchange,date.strftime('%Y%m%d'))
            if not os.path.exists(save_path):
                try:
                    os.makedirs(save_path)
                except:
                    pass
            dsb_name = f"{pid}{month}.dsb"
            if skip_saved:
                saved_list = os.listdir(save_path)
                if dsb_name in saved_list:
                    print(f"重复数据，跳过{dsb_name}")
                    continue
            t_day = date.strftime('%Y.%m.%d')
            df = self.get_tick(code,t_day,t_day)
            if (df is None) or (df.empty):
                print(f"{date}:{code}没有数据")
                continue
            dsb_path = os.path.join(save_path,f"{pid}{month}.dsb")
            self.tick_df_to_dsb(df,dsb_path)
            
    def bar_dsb_to_csv(self,binFolder,csvFolder):
        self.dthelper.dump_bars(binFolder, csvFolder)
    
    def tick_dsb_to_csv(self,binFolder,csvFolder):
        self.dthelper.dump_ticks(binFolder, csvFolder)
```

Example 3 (python):
```python
class RqFeed(Ifeed):
    def __init__(self,user=None,passwd=None):
        super().__init__()
        # 米筐的一些初始化
        self.rq = rq
        self.rq.init(user,passwd)
        # 因为每个数据源中各个字段的名称可能不同，因此需要我们自己统一转换为wt数据中的字段
        # bar数据字段映射，key为数据源中的字段，value为wt数据中对应的字段
        self.bar_col_map = {
            "date":"date",
            "time":"time",
            "open":"open",
            "high":"high",
            "low":"low",
            "close":"close",
            "total_turnover":"money",
            "volume":"vol",
            "open_interest":"hold",
        }
        # tick数据字段映射
        self.tick_col_map = {
            "code":"code",
            "exchg":"exchg",
            "last":"price",
            "open":"open",
            "high":"high",
            "low":"low",
            "volume":"total_volume",
            "vol":"volume",
            "total_turnover":"total_turnover",
            "turn_over":"turn_over",
            "open_interest":"open_interest",
            "diff_interest":"diff_interest",
            "trading_date":"trading_date",
            "date":"action_date",
            "time":"action_time",
            "prev_close":"pre_close",
            "settle_price":"settle_price",
            "prev_settlement":"pre_settle",
        }
        for i in [1,2,3,4,5]:
            self.tick_col_map[f"a{i}"] = f"ask_{i}"
            self.tick_col_map[f"b{i}"] = f"bid_{i}"
            self.tick_col_map[f"a{i}_v"] = f"ask_qty_{i}"
            self.tick_col_map[f"b{i}_v"] = f"bid_qty_{i}"
    
    # 米筐与WT中一些代码之间的转换
    def exchgStdToRQ(self,exchg:str) -> str:
        if exchg == 'SSE':
            return "XSHG"
        elif exchg == 'SZSE':
            return "XSHE"
        else:
            return exchg
    
    # 代码标准化处理
    def code_std(self,stdCode:str):
        stdCode = stdCode.upper()
        items = stdCode.split(".")
        exchg = self.exchgStdToRQ(items[0])
        if len(items) == 2:
            # 简单股票代码，格式如SSE.600000
            return items[1] + "." + exchg
        elif items[1] in ["IDX","ETF","STK","OPT"]:
            # 标准股票代码，格式如SSE.IDX.000001
            return items[2] + "." + exchg
        elif len(items) == 3:
            # 标准期货代码，格式如CFFEX.IF.2103
            if items[2] != 'HOT':
                return ''.join(items[1:])
            else:
                return items[1] + "88"
    
    # 获取tick数据df，主要要转换为统一的格式，比如字段名要重命名为WT的
    def get_tick(self,code,start_date=None,end_date=None):
        symbol = self.code_std(code)
        exchange,pid,month = self.parse_code(code)
        df = self.rq.get_price(symbol,start_date=start_date,end_date=end_date,frequency="tick")
        if df is None:
            return None
        df = df.reset_index()
        df["exchg"] = code.split(".")[0]
        df["code"] = code.split(".")[1] + code.split(".")[2]
        #改一下时间的格式
        if "datetime" in df.columns:
            df["datetime"] = pd.to_datetime(df["datetime"])
            df["date"] =  df["datetime"].dt.strftime("%Y%m%d")
            df["time"] =  df["datetime"].dt.strftime("%H%M%S%f")
            df["trading_date"] =  df["trading_date"].dt.strftime("%Y%m%d")
        else:
            df["date"] =  df["date"].dt.strftime("%Y%m%d")
            df["time"] = "000000"
            
        multiplier = self.rq.futures.get_contract_multiplier(pid.upper(),start_date,end_date)["contract_multiplier"].max()
        df["settle_price"] = ((df["total_turnover"] / df["volume"]) * multiplier).fillna(0.0)
        df["turn_over"] = (df["total_turnover"] - df["total_turnover"].shift(1)).fillna(0.0)
        df["vol"] = (df["volume"] - df["volume"].shift(1)).fillna(0.0)
        df["diff_interest"] = (df["open_interest"] - df["open_interest"].shift(1)).fillna(0.0)
        df = df[[col for col in self.tick_col_map.keys()]]
        # 重命名
        df = df.rename(columns=self.tick_col_map)
        return df
    
    # 获取bar数据的df
    def get_bar(self, code, start_date=None,end_date=None,frequency="1m"):
        if frequency not in self.frequency_map.keys():
            print("周期只能为m1、m5或d,回测或实盘中会自动拼接")
        rq_frequency = self.frequency_map[frequency]
        symbol = self.code_std(code)
        df = self.rq.get_price(symbol,start_date=start_date,end_date=end_date,frequency=rq_frequency)
        if df is None:
            return None
        df = df.reset_index()
        #改一下时间的格式
        if "datetime" in df.columns:
            df["datetime"] = pd.to_datetime(df["datetime"])
            df["date"] =  df["datetime"].dt.strftime("%Y%m%d")
            df["time"] =  df["datetime"].dt.strftime("%H%M")
        else:
            df["date"] =  df["date"].dt.strftime("%Y%m%d")
            df["time"] = "000000"
        df = df[[col for col in self.bar_col_map.keys()]]
        df = df.rename(columns=self.bar_col_map)
        return df
```

Example 4 (python):
```python
class RqFeed(Ifeed):
    def __init__(self,user=None,passwd=None):
        super().__init__()
        # 米筐的一些初始化
        self.rq = rq
        self.rq.init(user,passwd)
        # 因为每个数据源中各个字段的名称可能不同，因此需要我们自己统一转换为wt数据中的字段
        # bar数据字段映射，key为数据源中的字段，value为wt数据中对应的字段
        self.bar_col_map = {
            "date":"date",
            "time":"time",
            "open":"open",
            "high":"high",
            "low":"low",
            "close":"close",
            "total_turnover":"money",
            "volume":"vol",
            "open_interest":"hold",
        }
        # tick数据字段映射
        self.tick_col_map = {
            "code":"code",
            "exchg":"exchg",
            "last":"price",
            "open":"open",
            "high":"high",
            "low":"low",
            "volume":"total_volume",
            "vol":"volume",
            "total_turnover":"total_turnover",
            "turn_over":"turn_over",
            "open_interest":"open_interest",
            "diff_interest":"diff_interest",
            "trading_date":"trading_date",
            "date":"action_date",
            "time":"action_time",
            "prev_close":"pre_close",
            "settle_price":"settle_price",
            "prev_settlement":"pre_settle",
        }
        for i in [1,2,3,4,5]:
            self.tick_col_map[f"a{i}"] = f"ask_{i}"
            self.tick_col_map[f"b{i}"] = f"bid_{i}"
            self.tick_col_map[f"a{i}_v"] = f"ask_qty_{i}"
            self.tick_col_map[f"b{i}_v"] = f"bid_qty_{i}"
    
    # 米筐与WT中一些代码之间的转换
    def exchgStdToRQ(self,exchg:str) -> str:
        if exchg == 'SSE':
            return "XSHG"
        elif exchg == 'SZSE':
            return "XSHE"
        else:
            return exchg
    
    # 代码标准化处理
    def code_std(self,stdCode:str):
        stdCode = stdCode.upper()
        items = stdCode.split(".")
        exchg = self.exchgStdToRQ(items[0])
        if len(items) == 2:
            # 简单股票代码，格式如SSE.600000
            return items[1] + "." + exchg
        elif items[1] in ["IDX","ETF","STK","OPT"]:
            # 标准股票代码，格式如SSE.IDX.000001
            return items[2] + "." + exchg
        elif len(items) == 3:
            # 标准期货代码，格式如CFFEX.IF.2103
            if items[2] != 'HOT':
                return ''.join(items[1:])
            else:
                return items[1] + "88"
    
    # 获取tick数据df，主要要转换为统一的格式，比如字段名要重命名为WT的
    def get_tick(self,code,start_date=None,end_date=None):
        symbol = self.code_std(code)
        exchange,pid,month = self.parse_code(code)
        df = self.rq.get_price(symbol,start_date=start_date,end_date=end_date,frequency="tick")
        if df is None:
            return None
        df = df.reset_index()
        df["exchg"] = code.split(".")[0]
        df["code"] = code.split(".")[1] + code.split(".")[2]
        #改一下时间的格式
        if "datetime" in df.columns:
            df["datetime"] = pd.to_datetime(df["datetime"])
            df["date"] =  df["datetime"].dt.strftime("%Y%m%d")
            df["time"] =  df["datetime"].dt.strftime("%H%M%S%f")
            df["trading_date"] =  df["trading_date"].dt.strftime("%Y%m%d")
        else:
            df["date"] =  df["date"].dt.strftime("%Y%m%d")
            df["time"] = "000000"
            
        multiplier = self.rq.futures.get_contract_multiplier(pid.upper(),start_date,end_date)["contract_multiplier"].max()
        df["settle_price"] = ((df["total_turnover"] / df["volume"]) * multiplier).fillna(0.0)
        df["turn_over"] = (df["total_turnover"] - df["total_turnover"].shift(1)).fillna(0.0)
        df["vol"] = (df["volume"] - df["volume"].shift(1)).fillna(0.0)
        df["diff_interest"] = (df["open_interest"] - df["open_interest"].shift(1)).fillna(0.0)
        df = df[[col for col in self.tick_col_map.keys()]]
        # 重命名
        df = df.rename(columns=self.tick_col_map)
        return df
    
    # 获取bar数据的df
    def get_bar(self, code, start_date=None,end_date=None,frequency="1m"):
        if frequency not in self.frequency_map.keys():
            print("周期只能为m1、m5或d,回测或实盘中会自动拼接")
        rq_frequency = self.frequency_map[frequency]
        symbol = self.code_std(code)
        df = self.rq.get_price(symbol,start_date=start_date,end_date=end_date,frequency=rq_frequency)
        if df is None:
            return None
        df = df.reset_index()
        #改一下时间的格式
        if "datetime" in df.columns:
            df["datetime"] = pd.to_datetime(df["datetime"])
            df["date"] =  df["datetime"].dt.strftime("%Y%m%d")
            df["time"] =  df["datetime"].dt.strftime("%H%M")
        else:
            df["date"] =  df["date"].dt.strftime("%Y%m%d")
            df["time"] = "000000"
        df = df[[col for col in self.bar_col_map.keys()]]
        df = df.rename(columns=self.bar_col_map)
        return df
```

---

## 上下文

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/1.CTA%E5%BC%95%E6%93%8E/%E4%B8%8A%E4%B8%8B%E6%96%87.html

**Contents:**
- 上下文
- 输出接口
  - stra_log_text
- 时间接口
  - stra_get_tdate
  - stra_get_date
  - stra_get_time
  - 账户接口
  - stra_get_position_avgpx
  - stra_get_position_profit

策略运行上下文Context是策略可以直接访问的唯一对象，策略所有的接口都通过Context对象调用。

获取最新价格，一般在获取了K线以后再获取该价格

stra_prepare_bars与stra_get_bars功能相似，都能够起到订阅K线的作用，区别在于stra_get_bars会返回获取到的历史K线。同样stra_get_ticks也能起到stra_sub_ticks订阅数据的功能，但stra_get_ticks会订阅后并返回tick数据，而stra_sub_ticks仅订阅

如果订阅了多个合约，请确保仅选择了一个作为主K线，如果设置了多个主K线，可能会导致无法触发on_bar与on_calculate

设置仓位，引擎会根据目标仓位与已有仓位自动下单

推荐使用stra_set_position代替stra_enter_long、stra_enter_short、stra_exit_long、stra_exit_short等下单函数

多头信号入场，具体如何下单，参考actpolicy

空头信号入场，具体如何下单，参考actpolicy

---

## 准备工作

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/1.%E5%89%8D%E8%A8%80/4.%E5%87%86%E5%A4%87%E5%B7%A5%E4%BD%9C.html

**Contents:**
- 准备工作

在获取阅读后续内容前，请自行准备好代码，配置相关环境，并保证能够正常编译生成解决方案，具体的配置方法，请看开发环境搭建这里不再赘述

---

## DataManager

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/4.DataManager.html

**Contents:**
- DataManager

---

## DataReader

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/8.DataReader.html

**Contents:**
- DataReader

---

## 编码类

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/%E7%BC%96%E7%A0%81%E7%B1%BB.html

**Contents:**
- 编码类
- StdCode(str)

WT中，合约的标准表示形式，格式为交易所.品种.分月，品种区分大小写，如SHFE.ni.2201、SHFE.ni.HOT。

---

## WonderTrader Learning Notes

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/

**Contents:**
- WonderTrader Learning Notes

---

## 准备怎么写

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/1.%E5%89%8D%E8%A8%80/3.%E6%80%8E%E4%B9%88%E5%86%99.html

**Contents:**
- 准备怎么写

我自己是一个WonderTrader新手与CPP新手，因此我会在其中添加大量的思考与探索内容，甚至包括许多CPP基础知识。我希望这在记录我学习WonderTrader的过程中，也能帮助你了解这一个优秀的量化交易框架。我习惯自上而下的学习一个东西，因此我不会直接讲解源码，而会从上层出发，先分析问题（需求），再分析思路，最后再讲解实现原理和demo，希望能够达到一种浅入深出的效果。

在写这个笔记的同时，我也在和你们一起学习WonderTrader甚至CPP。有任何观点或问题，欢迎提交Issues或者联系我本人@ZzzzHeJ

Email: huerjie1996@foxmial.com

---

## 事件函数

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/2.HFT%E5%BC%95%E6%93%8E/%E4%BA%8B%E4%BB%B6%E5%87%BD%E6%95%B0.html

**Contents:**
- 事件函数
- __init__
- on_init
- on_session_begin
- on_session_end
- on_tick
- on_bar
- on_calculate
- on_calculate_done
- on_order_detail

HFT引擎中的事件大多与CTA引擎中一致，为了保证文档的完整性，这里重复介绍，如果仅想看HFT特有的，请点击链接跳转HFT特有事件

学过Python的小伙伴都知道，Python中类在被创建时，会调用__init__函数，来接受类创建时传入的各个参数。WT中的策略其实是一个继承自BaseCtaStrategy类的子类。因此当他被创建时，便会触发__init__函数，用来传入参数等信息。

以demo中使用的DualThrust.py为例：

注意，我们策略中所有的回调函数，包括__init__，均是对父类的重写。包括接受的参数与内部逻辑在内，均可以自己重写，切勿被demo中的__init__误导，误以为wt中的策略必须要接受这些参数。__init__建议接收的参数只有name，其他的均可以自己决定。

在回测中，on_init是在数据回放器准备好回测数据后执行，而实盘中，则是连接到行情服务后执行。on_init的主要功能是订阅行情数据，加载外部数据等。

__init__是策略初始化时触发的，是策略第一个被触发的函数。on_init是回测数据或行情服务准备好后触发的，是策略在接受数据前最后一个触发的函数。

逐笔数据进来时调用，生产环境中，每笔行情进来就直接调用，回测环境中，是模拟的逐笔数据

K线闭合时调用，一般作为策略的核心计算模块

on_calculate与on_bar在仅仅订阅一个品种时效果相同，但当订阅了多个品种时，存在以下区别：on_bar在每个品种K线闭合时都会触发一次。on_calculate在所有订阅的品种都触发一次on_bar后才会触发。这样做的好处是在处理截面策略时，能够保证行情数据能够对齐。

强化学习中使用，因为强化学习需要现在on_calculate获取view然后传给框架，最后在on_calculate中发出信号，而一般策略只需要使用on_calculate即可。

下面为HFT引擎特有的事件，主要为订单或成交相关的事件，在CTA引擎中，订单的相关处理由引擎管理，而HFT引擎则将这些细节暴露给策略自行处理，用于满足不同策略开发的需求。

初始持仓回报，实盘可用, 回测的时候初始仓位都是空, 所以不需要，在实盘中，该回调函数也仅在策略启动时执行一次，如果需要本地记录持仓时，可以配合该函数初始化本地持仓池

**Examples:**

Example 1 (python):
```python
# 继承策略基类BaseCtaStrategy
class StraDualThrust(BaseHftStrategy):
    # 重写初始化代码，接受的参数可以自己决定
    def __init__(self, name:str, code:str, barCnt:int, period:str, days:int, k1:float, k2:float, isForStk:bool = False):
        # 初始化父类，name参数表示该策略的名称
        BaseHftStrategy.__init__(self, name)

        # 接受参数，并把它赋值给自己的成员
        self.__days__ = days
        self.__k1__ = k1
        self.__k2__ = k2

        self.__period__ = period
        self.__bar_cnt__ = barCnt
        self.__code__ = code

        self.__is_stk__ = isForStk
```

Example 2 (python):
```python
# 继承策略基类BaseCtaStrategy
class StraDualThrust(BaseHftStrategy):
    # 重写初始化代码，接受的参数可以自己决定
    def __init__(self, name:str, code:str, barCnt:int, period:str, days:int, k1:float, k2:float, isForStk:bool = False):
        # 初始化父类，name参数表示该策略的名称
        BaseHftStrategy.__init__(self, name)

        # 接受参数，并把它赋值给自己的成员
        self.__days__ = days
        self.__k1__ = k1
        self.__k2__ = k2

        self.__period__ = period
        self.__bar_cnt__ = barCnt
        self.__code__ = code

        self.__is_stk__ = isForStk
```

Example 3 (python):
```python
def on_init(self, context:CtaContext):
    code = self.__code__    #品种代码
    if self.__is_stk__:
        code = code + "Q"

    #这里演示了品种信息获取的接口
    pInfo = context.stra_get_comminfo(code)
    print(pInfo)

    # 订阅K线
    context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
    # 订阅Tick
    context.stra_sub_ticks(code)

    context.stra_log_text("DualThrust inited")
```

Example 4 (python):
```python
def on_init(self, context:CtaContext):
    code = self.__code__    #品种代码
    if self.__is_stk__:
        code = code + "Q"

    #这里演示了品种信息获取的接口
    pInfo = context.stra_get_comminfo(code)
    print(pInfo)

    # 订阅K线
    context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
    # 订阅Tick
    context.stra_sub_ticks(code)

    context.stra_log_text("DualThrust inited")
```

---

## 源码解析

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/源码解析/

**Contents:**
- 源码解析

CPP源码解析，配合理解WonderTrader的逻辑流程

---

## 使用问题

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98/%E4%BD%BF%E7%94%A8%E9%97%AE%E9%A2%98.html

**Contents:**
- 使用问题
- 没有接受到行情回调
  - 错误描述
  - 解决方案
- datakit 收盘作业没有成功清理rt数据
  - 错误描述
  - 解决方案
- 未知的奇怪错误
  - 错误描述
  - 解决方案

无法接收到行情回调，包括on_tick, on_bar, on_calculate

datakit报尝试多次无法删除rt数据的问题

这种问题通常是在datakit进行收盘作业时，策略仍然没有停止，导致rt数据文件被占用。建议使用控制台定时调动策略的启停止。

各种奇奇怪怪的与内存相关的错误，包括闪崩的问题

---

## 1.前言

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/1.%E5%89%8D%E8%A8%80/

**Contents:**
- 1.前言

---

## WTPY

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/

**Contents:**
- WTPY

---

## 1.前言

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/1.前言/

**Contents:**
- 1.前言

---

## hotpicker

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E5%B7%A5%E5%85%B7%E9%9B%86/hotpicker.html

**Contents:**
- hotpicker

该工具负责更新基础配置中的hots.json与seconds.json。建议盘后更新。

hotpicker通过爬虫访问交易所官网来获取信息，有时可能并不稳定，作者开发了一种可以用米筐代替交易所作为数据源的工具，有需要请自取： RqCacheMonExchg

---

## WtMonSvr

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E5%B7%A5%E5%85%B7%E9%9B%86/WtMonSvr.html

**Contents:**
- WtMonSvr
- 启动服务
- 添加策略组合
- 添加调度

WtMonSvr是一个强大的控制台程序，提供实盘监控，自动调度，在线回测等功能。通过WtMonSvr，可以方便的设置程序的定时启动，配合datakit,hotpicker,ctp_loader等工具，可以实现数据、配置的自动更新。

demo中部署和回测监控文件夹，均需要使用绝对路径，如果没有改文件夹，则需要自己新建一个

运行后，浏览器中输入http://127.0.0.1:8099/console进入控制台

账户名为superman，密码为Helloworld!

组合配置中消息地址需要与自己配置文件中的消息地址一致，否则控制台无法接收到信息

添加成功后，点击调度，可是设置任务调度信息。

在调度中心中可以设置调度，调度即设置启动的相关信息，如工作目录，启动代码名，用哪个python启动，何时启动，是否开启进程守护等。

工作目录为代码所在目录，进程守护即是否在判断程序中断后，自动重启，检测间隔则未检查程序是否中断的间隔，计划任务为是否定时启动、停止、重启等调度设置。

对于python程序，在启动参数中，设置需要启动的python脚本，执行程序设置为用于执行的python解释器

而cpp程序则不用设置启动参数，只需要把执行程序设置为可执行程序就可以

对于任务信息的设置，可以这么理解，实际上就是通过cmd执行一个如下命令

**Examples:**

Example 1 (python):
```python
from wtpy.monitor import WtMonSvr, WtBtMon
from wtpy import WtDtServo

dtServo = WtDtServo()
# 配置基础文件
dtServo.setBasefiles(commfile="../common/commodities.json", 
                contractfile="../common/contracts.json", 
                holidayfile="../common/holidays.json", 
                sessionfile="../common/sessions.json", 
                hotfile="../common/hots.json")
dtServo.setStorage("../storage/")
dtServo.commitConfig()

# 设置部署文件夹，需要绝对路径
svr = WtMonSvr(deploy_dir="代码所在目录/deploy")
# 设置回测监控文件夹，需要绝对路径
btMon = WtBtMon(deploy_folder="代码所在目录/bt_deploy", logger=svr.logger)
svr.set_bt_mon(btMon)
svr.set_dt_servo(dtServo)
# 启动的端口
svr.run(port=8099, bSync=False)
input("press enter key to exit\n")
```

Example 2 (python):
```python
from wtpy.monitor import WtMonSvr, WtBtMon
from wtpy import WtDtServo

dtServo = WtDtServo()
# 配置基础文件
dtServo.setBasefiles(commfile="../common/commodities.json", 
                contractfile="../common/contracts.json", 
                holidayfile="../common/holidays.json", 
                sessionfile="../common/sessions.json", 
                hotfile="../common/hots.json")
dtServo.setStorage("../storage/")
dtServo.commitConfig()

# 设置部署文件夹，需要绝对路径
svr = WtMonSvr(deploy_dir="代码所在目录/deploy")
# 设置回测监控文件夹，需要绝对路径
btMon = WtBtMon(deploy_folder="代码所在目录/bt_deploy", logger=svr.logger)
svr.set_bt_mon(btMon)
svr.set_dt_servo(dtServo)
# 启动的端口
svr.run(port=8099, bSync=False)
input("press enter key to exit\n")
```

---

## DataWriter

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/9.DataWriter.html

**Contents:**
- DataWriter

---

## 上下文

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/2.HFT%E5%BC%95%E6%93%8E/%E4%B8%8A%E4%B8%8B%E6%96%87.html

**Contents:**
- 上下文
- 输出接口
  - stra_log_text
- 时间接口
  - stra_get_tdate
  - stra_get_date
  - stra_get_time
  - stra_get_secs
  - 账户接口
  - stra_get_position_avgpx

策略运行上下文Context是策略可以直接访问的唯一对象，策略所有的接口都通过Context对象调用。

获取最新价格，一般在获取了K线以后再获取该价格

stra_prepare_bars与stra_get_bars功能相似，都能够起到订阅K线的作用，区别在于stra_get_bars会返回获取到的历史K线。同样stra_get_ticks也能起到stra_sub_ticks订阅数据的功能，但stra_get_ticks会订阅后并返回tick数据，而stra_sub_ticks仅订阅

如果订阅了多个合约，请确保仅选择了一个作为主K线，如果设置了多个主K线，可能会导致无法触发on_bar与on_calculate

---

## 包括什么

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/1.%E5%89%8D%E8%A8%80/2.%E5%8C%85%E6%8B%AC%E4%BB%80%E4%B9%88.html

**Contents:**
- 包括什么

本笔记包括WonderTrader框架设计、实现原理、入门，CPP源码讲解的相关内容，是我自己学习过程中的一些记录，同时也包含一些CPP的基础知识和常见问题的解决，希望能够对大家有所帮助。

---

## SEL引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/3.SEL%E5%BC%95%E6%93%8E/

**Contents:**
- SEL引擎

也叫异步策略引擎，一般适用于标的较多，计算逻辑耗时较长的策略，时间驱动。典型应用场景包括多因子选股策略、截面多空策略等。

---

## RiskMonitor

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/6.RiskMonitor.html

**Contents:**
- RiskMonitor

---

## ctp_loader

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E5%B7%A5%E5%85%B7%E9%9B%86/ctp_loader.html

**Contents:**
- ctp_loader

该工具负责更新基础配置中的commodities.json与contracts.json。建议盘中更新。

---

## UFT引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/4.UFT%E5%BC%95%E6%93%8E/

**Contents:**
- UFT引擎

也叫极速策略引擎，主要针对超高频或者超低延时策略，事件驱动，系统延迟在200纳秒之内

---

## Executers

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/5.Executers.html

**Contents:**
- Executers

---

## datakit

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E5%B7%A5%E5%85%B7%E9%9B%86/datakit.html

**Contents:**
- datakit
- dtcfg.yaml
- statemonitor.yaml
- mdparsers.yaml
- runDT

仿真或实盘中的实时行情，需要通过datakit工具实时接收，datakit将接受到的行情分发给不同的策略，同时并保存到本地作为当日的历史行情，在收盘后会的盘后处理中，将当日数据合并到历史行情中。盘后工作的时间取决于datakit的配置文件statemonitor.yaml

datakit接受哪些合约的行情，取决于contracts.json，所以记得定时使用ctp_loader进行更新。

datakit工具配置，制定了基础配置文件的所在目录、行情分发设置以及指定其他配置文件

如果需要开启多个datakit的话，需要设置不同的bport与path

datakit的配置文件之一，指定了不同交易时段品种的收盘时间，初始化时间，名称以及盘后工作时间

**Examples:**

Example 1 (unknown):
```unknown
basefiles:                                    # 基础配置文件目录
    commodity: ..\common\commodities.json
    contract: ..\common\contracts.json
    holiday: ..\common\holidays.json
    session: ..\common\sessions.json
broadcaster:                                  # 数据广播设置
    active: true
    bport: 3997                               # 端口
    broadcast:                                # 广播设置，策略配置中tdparsers.yaml的配置需要与这个一致，否则无法接收到数据
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
parsers: mdparsers.yaml                       # 行情服务设置
statemonitor: statemonitor.yaml               # 状态设置
writer:                                       # 行情记录设置
    module: WtDtStorage     #数据存储模块
    async: true             #同步落地还是异步落地，期货推荐同步，股票推荐异步
    groupsize: 20           #日志分组大小，主要用于控制日志输出，当订阅合约较多时，推荐1000以上，当订阅的合约数较少时，推荐100以内
    path: ../STK_Data       #数据存储的路径
    savelog: false          #是否保存tick到csv
    disabletick: false      #不保存tick数据，默认false
    disablemin1: false      #不保存min1数据，默认false
    disablemin5: false      #不保存min5数据，默认false
    disableday: false       #不保存day数据，默认false
    disabletrans: false     #不保存股票l2逐笔成交数据，默认false
    disableordque: false    #不保存股票l2委托队列数据，默认false
    disableorddtl: false    #不保存股票l2逐笔委托数据，默认false
    disablehis: false       #收盘作业不转存历史数据，默认false
```

Example 2 (unknown):
```unknown
basefiles:                                    # 基础配置文件目录
    commodity: ..\common\commodities.json
    contract: ..\common\contracts.json
    holiday: ..\common\holidays.json
    session: ..\common\sessions.json
broadcaster:                                  # 数据广播设置
    active: true
    bport: 3997                               # 端口
    broadcast:                                # 广播设置，策略配置中tdparsers.yaml的配置需要与这个一致，否则无法接收到数据
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
parsers: mdparsers.yaml                       # 行情服务设置
statemonitor: statemonitor.yaml               # 状态设置
writer:                                       # 行情记录设置
    module: WtDtStorage     #数据存储模块
    async: true             #同步落地还是异步落地，期货推荐同步，股票推荐异步
    groupsize: 20           #日志分组大小，主要用于控制日志输出，当订阅合约较多时，推荐1000以上，当订阅的合约数较少时，推荐100以内
    path: ../STK_Data       #数据存储的路径
    savelog: false          #是否保存tick到csv
    disabletick: false      #不保存tick数据，默认false
    disablemin1: false      #不保存min1数据，默认false
    disablemin5: false      #不保存min5数据，默认false
    disableday: false       #不保存day数据，默认false
    disabletrans: false     #不保存股票l2逐笔成交数据，默认false
    disableordque: false    #不保存股票l2委托队列数据，默认false
    disableorddtl: false    #不保存股票l2逐笔委托数据，默认false
    disablehis: false       #收盘作业不转存历史数据，默认false
```

Example 3 (unknown):
```unknown
FD0900:
    closetime: 1515     # 收盘时间
    inittime: 850       # 初始化时间
    name: 期白0900      # 名称
    proctime: 1600      # 盘后工作时间
FD0915:
    closetime: 1530
    inittime: 900
    name: 期白0915
    proctime: 1600
```

Example 4 (unknown):
```unknown
FD0900:
    closetime: 1515     # 收盘时间
    inittime: 850       # 初始化时间
    name: 期白0900      # 名称
    proctime: 1600      # 盘后工作时间
FD0915:
    closetime: 1530
    inittime: 900
    name: 期白0915
    proctime: 1600
```

---

## CTA引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/1.CTA%E5%BC%95%E6%93%8E/

**Contents:**
- CTA引擎

CTA引擎，也叫同步策略引擎，一般适用于标的较少，计算逻辑较快的策略，事件+时间驱动。典型的应用场景包括单标的择时、中频以下的套利等。Demo中提供的DualThrust策略，单次重算平均耗时，Python实现版本约70多微秒，C++实现版本约4.5微秒。

---

## 上下文

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/3.SEL%E5%BC%95%E6%93%8E/%E4%B8%8A%E4%B8%8B%E6%96%87.html

**Contents:**
- 上下文

---

## 常见问题

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98/

**Contents:**
- 常见问题

---

## StrategyEngine

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/3.StrategyEngine.html

**Contents:**
- StrategyEngine

---

## 事件函数

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/4.UFT%E5%BC%95%E6%93%8E/%E4%BA%8B%E4%BB%B6%E5%87%BD%E6%95%B0.html

**Contents:**
- 事件函数

---

## 数据类

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/%E6%95%B0%E6%8D%AE%E7%B1%BB.html

**Contents:**
- 数据类
- WtTickRecords

Wt中的tick数据结构，调用方式类似dict，如tick["price"]，tick数据包括以下属性

---

## Trader

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/11.Trader.html

**Contents:**
- Trader

---

## 交易引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E4%BA%A4%E6%98%93%E5%BC%95%E6%93%8E/

**Contents:**
- 交易引擎

---

## 参考资料

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/1.%E5%89%8D%E8%A8%80/5.%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99.html

**Contents:**
- 参考资料
- wonderTrader相关
- cpp相关
- python相关

---
