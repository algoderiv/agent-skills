# Wondertrader-Rtd - Other

**Pages:** 11

---

## WonderTrader官方文档

**URL:** https://wtdocs.readthedocs.io/zh/latest/index.html

**Contents:**
- WonderTrader官方文档

© 版权所有 WonderTrader。 版本 280b46ec.

---

## 文档说明【必看】

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/preface.html

**Contents:**
- 文档说明【必看】
- 写在最前
- 文档介绍

如果你刚接触量化，或者IT技能相对薄弱，推荐使用WtStudio

如果你有一定的IT技能，对于交易机运维有一定的基础，但是对C++不熟悉，推荐使用wtpy

如果你有很丰富的IT技能，编程能力也很强，但是策略对滑点不敏感，推荐使用wtpy

如果你的策略对延迟很敏感，推荐使用wtcpp

整个WonderTrader包含了三大部分：wtcpp、wtpy和WtStudio，针对不同背景的用户

从功能的角度来说，wtpy覆盖了wtcpp全部功能（除了UFTEngine外），并且还有很多wtcpp没有的模块，所以整个文档的结构会根据wtpy的功能分类来组织

C++相关的开发会作为一个独立的栏目，这部分会介绍C++的策略开发以及基于wtcpp的二次开发的相关内容

WtStudio作为一个客户端，相对比较独立，所以这部分文档会单独作为一个栏目

© 版权所有 WonderTrader。 版本 280b46ec.

---

## WonderTrader官方文档

**URL:** https://wtdocs.readthedocs.io/zh/latest/

**Contents:**
- WonderTrader官方文档

© 版权所有 WonderTrader。 版本 280b46ec.

---

## WtDtServo

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/tools/wtdtservo.html

**Contents:**
- WtDtServo
- WtDtServo是什么
- 如何使用WtDtServo

通常情况下，一个策略从研发到实盘的流程都遵循以下几个步骤：

思路研究，这个阶段一般验证一些思路是否可行，比如一些技术指标的组合，或者一些简单的交易逻辑

策略回测，如果思路被验证是可行的以后，就可以基于这些思路实现策略，除了主要逻辑的实现之外，还要加入止盈止损、资金管理、频率控制等模块，然后进行回测，看看策略的表现

仿真跟踪，如果回测可行，收益指标符合预期，就可以进行仿真跟踪，跟踪一段时间，看看策略在样本外数据的表现

实盘上线，仿真跟踪一段时间以后，基本符合实盘的要求，就可以进行实盘交易了

WonderTrader在整个量化交易的环节中，提供了数据组件datakit，datakit会自动将实时的行情数据落地，并实时重采样成min1、min5两种基础周期的K线。策略在回测和实盘（仿真）的时候，就可以直接和datakit配合使用，通过数据读取模块WtDataReader就可以直接读取datakit落地的数据。

对于思路研究来说，一般就直接对数据进行向量化处理，而不是通过回测模拟的事件驱动机制来回放数据。WtDtServo就是针对这样的需求，向用户提供了随机访问datakit落地的数据的接口。概括起来，包含两类接口：

根据指定截止时间以及所需的数据量获取历史数据

实际上，除了用户层面的接口之外，WtDtServo底层也采用了WtDataReader一样的缓存机制，提供了和策略内无差别的访问效率和使用体验。

虽然WtDtServo最初的设计思路是针对投研需求，但是WtDtServo因为提供的是基础数据的访问能力，所以适用的场景是非常丰富的，wtpy内部很多组件都依赖WtDtServo提供的数据访问能力：

WonderTrader工作台WtStudio

在线回测管理器WtBtMonitor(该组件以后可能会取消，慎用)

对于用户来说，要使用WtDtServo，可以参考以下代码：

如果想了解更多详情，可以参考wtpy/demos/test_dtservo

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
from wtpy import WtDtServo

dtServo = WtDtServo()
# 这是基础数据文件
dtServo.setBasefiles(folder="../common/")
# 这里设置datakit配置的数据落地根目录
dtServo.setStorage(path='../storage/')

# 读取IF主力合约的前复权数据
bars = dtServo.get_bars("CFFEX.IF.HOT-", "m5", fromTime=202205010930, endTime=202205281500).to_df()
bars.to_csv("CFFEX.IF.HOT-.csv")

# 读取IF主力合约的后复权数据
bars = dtServo.get_bars("CFFEX.IF.HOT+", "m5", fromTime=202205010930, endTime=202205281500).to_df()
bars.to_csv("CFFEX.IF.HOT+.csv")

# 读取IF主力合约的原始拼接数据
bars = dtServo.get_bars("CFFEX.IF.HOT", "m5", fromTime=202205010930, endTime=202205281500).to_df()
bars.to_csv("CFFEX.IF.HOT.csv")

# 读取IF主力合约的tick数据
bars = dtServo.get_ticks("CFFEX.IF.HOT", fromTime=202207250930, endTime=202207291500).to_df()
bars.to_csv("CFFEX.IF.HOT_ticks.csv")
```

---

## 对接外部数据存储

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/levelup/extdata.html

**Contents:**
- 对接外部数据存储
- 为什么要对接外部数据存储
- ExtDataLoader简介
- ExtDataDumper简介

为了让WonderTrader更精简更高效，从架构设计之初，WonderTrader就否决了对第三方组件的依赖。所以WonderTrader的数据存储全部采用自研的数据结构，按照文件存储。这样的设计方案，除了不依赖第三方组件之外，数据读写的速度也是非常高的，而且可以节省数据格式转换的开销，加载到内存以后直接就是内部的数据结构。

虽然WonderTrader自身数据存储机制设计已经非常优秀了，对于很多用户来说都绝对能够满足需求。但是始终存在一类用户，他们可能是成熟的量化团队，一开始使用的其他量化平台，而数据的存储也是针对其他的量化平台搭建的。他们在迁移到WonderTrader的时候，要面对的第一件事情就是数据的转换。这种转换，除了历史数据的转换之外，还必须要考虑到新数据的及时同步，从实现的角度来说是非常繁琐的，而且还需要反复的测试才能稳定下来。

有没有一种方法，可以让WonderTrader直接从原有的数据存储引起加载数据，并且在datakit做收盘作业的时候能够再将新的数据存到原有的数据引擎呢？

答案是有！为了解决这种平台迁移的首要问题，WonderTrader专门提供了两个组件：

ExtDataLoader组件，是由回测框架和实盘框架调用的，允许用户自己实现从外部数据存储加载数据。如果用户向底层核心注册了自定义的ExtDataLoader，那么系统会在加载数据的时候，主动调用ExtDataLoader的数据加载接口。ExtDataLoader支持的接口如下：

如果要使用ExtDataLoader，可以参考以下代码：

更多详细信息可以参考demo: wtpy/demos/test_dataexts/testDtLoader.py

ExtDataDumper，是由datakit调用的，允许用户自己转储到其他存储引擎。如果用户向datakit注册了一个自定义的数据转储器，datakit会在收盘作业的时候将底层数据块传递出来，由ExtDataDumper将历史行情数据转储。如果datakit配置的是allday模式，那么转储的接口会在每条基础K线闭合的时候调用。ExtDataDumper支持的接口如下：

如果要使用ExtDataDumper，可以参考以下代码：

更多详细信息可以参考demo: wtpy/demos/test_dataexts/testDtDumper.py

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
class BaseExtDataLoader:

    def __init__(self):
        pass

    def load_final_his_bars(self, stdCode:str, period:str, feeder) -> bool:
        '''
        加载最终历史K线（回测、实盘）
        该接口一般用于加载外部处理好的复权数据、主力合约数据

        @stdCode    合约代码，格式如CFFEX.IF.2106
        @period     周期，m1/m5/d1
        @feeder     回调函数，feed_raw_bars(bars:POINTER(WTSBarStruct), count:int)
        '''
        return False

    def load_raw_his_bars(self, stdCode:str, period:str, feeder) -> bool:
        '''
        加载未加工的历史K线（回测、实盘）
        该接口一般用于加载原始的K线数据，如未复权数据和分月合约数据

        @stdCode    合约代码，格式如CFFEX.IF.2106
        @period     周期，m1/m5/d1
        @feeder     回调函数，feed_raw_bars(bars:POINTER(WTSBarStruct), count:int)
        '''
        return False

    def load_his_ticks(self, stdCode:str, uDate:int, feeder) -> bool:
        '''
        加载历史K线（只在回测有效，实盘只提供当日落地的）
        @stdCode    合约代码，格式如CFFEX.IF.2106
        @uDate      日期，格式如yyyymmdd
        @feeder     回调函数，feed_raw_bars(bars:POINTER(WTSTickStruct), count:int)
        '''
        return False

    def load_adj_factors(self, stdCode:str = "", feeder = None) -> bool:
        '''
        加载的权因子
        @stdCode    合约代码，格式如CFFEX.IF.2106，如果stdCode为空，则是加载全部除权数据，如果stdCode不为空，则按需加载
         @feeder     回调函数，feed_adj_factors(stdCode:str, dates:list, factors:list)
        '''
        return False
```

Example 2 (python):
```python
from wtpy.ExtModuleDefs import BaseExtDataLoader
from ctypes import POINTER
from wtpy.WtCoreDefs import WTSBarStruct, WTSTickStruct

import pandas as pd

import random

from wtpy import WtEngine,WtBtEngine,EngineType
from wtpy.apps import WtBtAnalyst
from Strategies.DualThrust import StraDualThrust

class MyDataLoader(BaseExtDataLoader):
    
    def load_final_his_bars(self, stdCode:str, period:str, feeder) -> bool:
        '''
        加载历史K线（回测、实盘）
        @stdCode    合约代码，格式如CFFEX.IF.2106
        @period     周期，m1/m5/d1
        @feeder     回调函数，feed_raw_bars(bars:POINTER(WTSBarStruct), count:int, factor:double)
        '''
        print("loading %s bars of %s from extended loader" % (period, stdCode))

        df = pd.read_csv('../storage/csv/CFFEX.IF.HOT_m5.csv')
        df = df.rename(columns={
            '<Date>':'date',
            ' <Time>':'time',
            ' <Open>':'open',
            ' <High>':'high',
            ' <Low>':'low',
            ' <Close>':'close',
            ' <Volume>':'vol',
            })
        df['date'] = df['date'].astype('datetime64').dt.strftime('%Y%m%d').astype('int64')
        df['time'] = (df['date']-19900000)*10000 + df['time'].str.replace(':', '').str[:-2].astype('int')

        BUFFER = WTSBarStruct*len(df)
        buffer = BUFFER()

        def assign(procession, buffer):
            tuple(map(lambda x: setattr(buffer[x[0]], procession.name, x[1]), enumerate(procession)))


        df.apply(assign, buffer=buffer)
        print(df)
        print(buffer[0].to_dict)
        print(buffer[-1].to_dict)

        feeder(buffer, len(df))
        return True

    def load_his_ticks(self, stdCode:str, uDate:int, feeder) -> bool:
        '''
        加载历史K线（只在回测有效，实盘只提供当日落地的）
        @stdCode    合约代码，格式如CFFEX.IF.2106
        @uDate      日期，格式如yyyymmdd
        @feeder     回调函数，feed_raw_ticks(ticks:POINTER(WTSTickStruct), count:int)
        '''
        print("loading ticks on %d of %s from extended loader" % (uDate, stdCode))

        df = pd.read_csv('../storage/csv/rb主力连续_20201030.csv')
        BUFFER = WTSTickStruct*len(df)
        buffer = BUFFER()

        tags = ["一","二","三","四","五"]

        for i in range(len(df)):
            curTick = buffer[i]

            curTick.exchg = b"SHFE"
            curTick.code = b"SHFE.rb.HOT"

            curTick.price = float(df[i]["最新价"])
            curTick.open = float(df[i]["今开盘"])
            curTick.high = float(df[i]["最高价"])
            curTick.low = float(df[i]["最低价"])
            curTick.settle = float(df[i]["本次结算价"])
            
            curTick.total_volume = float(df[i]["数量"])
            curTick.total_turnover = float(df[i]["成交额"])
            curTick.open_interest = float(df[i]["持仓量"])

            curTick.trading_date = int(df[i]["交易日"])
            curTick.action_date = int(df[i]["业务日期"])
            curTick.action_time = int(df[i]["最后修改时间"].replace(":",""))*1000 + int(df[i]["最后修改毫秒"])

            curTick.pre_close = float(df[i]["昨收盘"])
            curTick.pre_settle = float(df[i]["上次结算价"])
            curTick.pre_interest = float(df[i]["昨持仓量"])

            for x in range(5):
                curTick.bid_prices[x] = float(df[i]["申买价"+tags[x]])
                curTick.bid_qty[x] = float(df[i]["申买量"+tags[x]])
                curTick.ask_prices[x] = float(df[i]["申卖价"+tags[x]])
                curTick.ask_qty[x] = float(df[i]["申卖量"+tags[x]])

        feeder(buffer, len(df))

def test_in_bt():
    engine = WtBtEngine(EngineType.ET_CTA)

    # 初始化之前，向回测框架注册加载器
    engine.set_extended_data_loader(loader=MyDataLoader(), bAutoTrans=False)

    engine.init('../common/', "configbt.yaml")

    engine.configBacktest(201909100930,201912011500)
    engine.configBTStorage(mode="csv", path="../storage/")
    engine.commitBTConfig()

    straInfo = StraDualThrust(name='pydt_IF', code="CFFEX.IF.HOT", barCnt=50, period="m5", days=30, k1=0.1, k2=0.1, isForStk=False)
    engine.set_cta_strategy(straInfo)

    engine.run_backtest()

    analyst = WtBtAnalyst()
    analyst.add_strategy("pydt_IF", folder="./outputs_bt/pydt_IF/", init_capital=500000, rf=0.02, annual_trading_days=240)
    analyst.run()

    kw = input('press any key to exit\n')
    engine.release_backtest()

def test_in_rt():
    engine = WtEngine(EngineType.ET_CTA)

    # 初始化之前，向实盘框架注册加载器
    engine.set_extended_data_loader(MyDataLoader())

    engine.init('../common/', "config.yaml")
    
    straInfo = StraDualThrust(name='pydt_au', code="SHFE.au.HOT", barCnt=50, period="m5", days=30, k1=0.2, k2=0.2, isForStk=False)
    engine.add_cta_strategy(straInfo)

    engine.run()

    kw = input('press any key to exit\n')

test_in_bt()
```

Example 3 (python):
```python
class BaseExtDataDumper:

    def __init__(self, id:str):
        self.__id__ = id

    def id(self):
        return self.__id__

    def dump_his_bars(self, stdCode:str, period:str, bars, count:int) -> bool:
        '''
        加载历史K线（回测、实盘）
        @stdCode    合约代码，格式如CFFEX.IF.2106
        @period     周期，m1/m5/d1
        @bars       回调函数，WTSBarStruct的指针
        @count      数据条数
        '''
        return True

    def dump_his_ticks(self, stdCode:str, uDate:int, ticks, count:int) -> bool:
        '''
        加载历史K线（只在回测有效，实盘只提供当日落地的）
        @stdCode    合约代码，格式如CFFEX.IF.2106
        @uDate      日期，格式如yyyymmdd
        @ticks      回调函数，WTSTickStruct的指针
        @count      数据条数
        '''
        return True
```

Example 4 (python):
```python
from wtpy import WtDtEngine

from wtpy.ExtModuleDefs import BaseExtDataDumper

class MyDataDumper(BaseExtDataDumper):
    def __init__(self, id:str):
        BaseExtDataDumper.__init__(self, id)

    def dump_his_bars(self, stdCode:str, period:str, bars, count:int) -> bool:
        '''
        加载历史K线（回测、实盘）
        @stdCode    合约代码，格式如CFFEX.IF.2106
        @period     周期，m1/m5/d1
        @bars       回调函数，WTSBarStruct的指针
        @count      数据条数
        '''
        print("dumping %s bars of %s via extended dumper" % (period, stdCode))
        return True

    def dump_his_ticks(self, stdCode:str, uDate:int, ticks, count:int) -> bool:
        '''
        加载历史K线（只在回测有效，实盘只提供当日落地的）
        @stdCode    合约代码，格式如CFFEX.IF.2106
        @uDate      日期，格式如yyyymmdd
        @ticks      回调函数，WTSTickStruct的指针
        @count      数据条数
        '''
        print("dumping ticks on %d of %s via extended dumper" % (uDate, stdCode))
        return True

def test_ext_dumper():
    #创建一个运行环境，并加入策略
    engine = WtDtEngine()
    engine.add_extended_data_dumper(MyDataDumper("dumper"))
    engine.initialize("dtcfg.yaml", "logcfgdt.yaml")
    
    engine.run()

    kw = input('press any key to exit\n')
```

---

## 历史数据处理

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/usage/histdata.html

**Contents:**
- 历史数据处理
- WonderTrader数据存储方式
  - 文件存储
  - csv历史数据
- datahelper模块
  - 数据源
  - 数据接口
- 数据下载
  - 模块初始化
  - 下载数据到文件中

环境准备好了以后，还需要确定我们准备使用什么方式存储数据。WonderTrader实盘环境下只支持：自有文件存储。而回测环境下，还支持直接从csv读取数据（仅限于历史K线数据）。

历史K线数据文件，采用zstd压缩后存放。高频历史数据，包括tick数据，股票level2的委托明细、成交明细、委托队列，也采用压缩存放的方式。A股全市场一天的level2数据，压缩以后也就是大概2G不到的样子，对于硬盘来说是相当友好的。 实时数据文件，因为需要实时读写，所以不压缩数据结构，并采用mmap的方式映射到内存中，直接对文件进行读写，提高读写效率。

很多用户通过各种渠道获取到的历史数据，供应商为了便于用户直接查看数据，一般都会提供csv格式的。但是csv文件格式的数据，占用空间非常大，而且直接从csv文件读取数据的开销也是非常大的。 WonderTrader的回测框架为了尽量减少这种不必要的开销，在处理csv文件时，第一次会直接从csv文件读取，将读取的数据转成WonderTrader内部数据结构之后，会将数据转储为WonderTrader自有的压缩存放格式。这样下次在使用该数据的时候，读取压缩存放的数据以后，直接解压就可以得到结构化的历史数据，这样就可以直接进行访问了。

datahelper模块位于wtpy.apps子模块下，采用工厂模式进行封装，最大限度的降低了使用难度，将各种API的差异全部封装起来，用户在使用的时候只需要调用工厂创建即可，而不用担心因为每个数据源API不同而导致的各种问题。

datahelper模块目前已封装的数据源包括tushare、baostock、RQData。

tushare是知名度较高的免费数据源，数据比较全，使用的人也很多。但是tushare有些数据需要积分才能下载，下载速度也较慢

baostock是一个免费、开源的证券数据平台，无需注册，并且下载速度也非常快，可以拿到5分钟线数据

RQData是米筐开发的一个基于Python的金融数据工具包，是一个收费数据源，数据很全，数据质量也很高。对于一些有1分钟线甚至更高频数据的需求，免费的数据源就无法提供了，RQData可能也是一个不错的选择。

datahelper模块主要帮助用户进行历史数据的下载，以及一些基础数据的获取。主要包括3种接口：

对于财务数据，WonderTrader暂时没有从平台层面做标准化的工作。一方面财务数据相对静态，可以相对容易的从不同的渠道拿到。另一方面只有股票才需要财务数据，而现在最流行的选股框架还是多因子框架。相对WonderTrader而言，多因子框架几乎是另一个维度的，所以WonderTrader暂时就不涉及财务数据这块了。

本文将以tushare数据源为例，演示一下数据辅助模块的基本用法。

首先，创建tushare对应的数据辅助模块：

值得一提的是，上面的代码中没有一个参数use_pro，该参数不是tushare认证需要的，而是用于控制tushare调用的接口的，如果use_pro为True，那么就调用tushare的pro_bar接口读取历史K线数据，否则就调用老版本的接口get_k_data读取历史K线数据。之所以这样，是因为pro_bar接口获取分钟数据的时候需要积分的，但是老的接口是不需要积分的。

然后调用不同的接口获取数据，下面的代码演示了将数据下载到指定的文件中：

上面演示了datahelper模块的用法，该模块能够帮助用户快速拉取WonderTrader可以直接使用的历史数据，可以有效的降低用户初次使用WonderTrader进行策略回测的门槛。

不过在实盘的过程中，还有很多实施的细节，本文也做一个大概的梳理。

期货合约代码，标准格式为CFFEX.IF.2103，其中郑商所的合约，月份也要扩展为4位

期货主力合约，标准格式为CFFEX.IF.HOT，WonderTrader会根据一个主力合约规则文件自动映射到分月合约

证券代码，股票的标准格式为SSE.STK.600000，指数的标准格式为SZSE.IDX.399001

WonderTrader需要定期维护品种和合约信息，即commodities.json和contracts.json文件，来更新至最新期货品种。 对此，WonderTrader提供了ctp_loader，从simnow拉取品种合约信息。 注意：如果出现新的品种，需要自己在map_future.ini中更新相对应的键值，否则会导致更新的基础文件出现对应问题，造成运行引擎WtEngine初始化时报错！（同时需要wtpy==0.9.8）

主力合约映射的规则，需要每天维护，即hots.json文件，WonderTrader会根据规则自动处理映射，用户只需要使用.HOT代码就可以了。

对于主力合约规则文件的更新，WonderTrader提供了hotpicker工具，有两种方式更新：

WonderTrader在读取主力合约的历史数据时，会优先读取直接对应的历史数据文件。如存储模式为文件时会先读取名为CFFEX.IF_HOT.dsb的文件，然后再根据主力合约规则读取分月合约的数据进行拼接。而如果存储模式为数据库，则会优先读取代码为xx.HOT的数据，然后再根据主力合约规则读取分月合约的数据。

实时录制：datakit负责在实盘中录制实时行情数据，存储在指定的数据目录中，同时通知策略进行接收

收盘作业：在每个交易日结束以后，会对实时行情数据做一个盘后处理，默认是在每天的16点

将实时高频数据按天按代码压缩存放(tick和level2高频数据)

将当日的K线数据(min1和min5)合并到历史K线数据中

根据当天最新的tick数据，生成当天的日K线数据并合并到历史日K线数据中

正是因为有收盘作业这么一个机制，所以WonderTrader目前还不能很好的适应7×24小时交易的品种，如数字货币。所以WonderTrader对于数据货币的支持的最大的问题，还是7×24小时交易机制的数据处理问题。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
class BaseDataHelper:

    def __init__(self):
        self.isAuthed = False
        pass

    def __check__(self):
        if not self.isAuthed:
            raise Exception("This module has not authorized yet!")

    def auth(self, **kwargs):
        '''
        模块认证
        '''
        pass

    def dmpCodeListToFile(self, filename:str, hasIndex:bool=True, hasStock:bool=True):
        '''
        将代码列表导出到文件\n
        @filename   要输出的文件名，json格式\n
        @hasIndex   是否包含指数\n
        @hasStock   是否包含股票\n
        '''
        pass

    def dmpAdjFactorsToFile(self, codes:list, filename:str):
        '''
        将除权因子导出到文件\n
        @codes  股票列表，格式如["SSE.600000","SZSE.000001"]\n
        @filename   要输出的文件名，json格式
        '''
        pass

    def dmpBarsToFile(self, folder:str, codes:list, start_date:datetime=None, end_date:datetime=None, period:str="day"):
        '''
        将K线导出到指定的目录下的csv文件，文件名格式如SSE.600000_d.csv\n
        @folder 要输出的文件夹\n
        @codes  股票列表，格式如["SSE.600000","SZSE.000001"]\n
        @start_date 开始日期，datetime类型，传None则自动设置为1990-01-01\n
        @end_date   结束日期，datetime类型，传None则自动设置为当前日期\n
        @period K线周期，支持day、min1、min5\n
        '''
        pass

    def dmpAdjFactorsToDB(self, dbHelper:DBHelper, codes:list):
        '''
        将除权因子导出到数据库\n
        @codes  股票列表，格式如["SSE.600000","SZSE.000001"]\n
        @dbHelper   数据库辅助模块
        '''
        pass

    def dmpBarsToDB(self, dbHelper:DBHelper, codes:list, start_date:datetime=None, end_date:datetime=None, period:str="day"):
        '''
        将K线导出到数据库\n
        @dbHelper 数据库辅助模块\n
        @codes  股票列表，格式如["SSE.600000","SZSE.000001"]\n
        @start_date 开始日期，datetime类型，传None则自动设置为1990-01-01\n
        @end_date   结束日期，datetime类型，传None则自动设置为当前日期\n
        @period K线周期，支持day、min1、min5\n
        '''
        pass
```

Example 2 (python):
```python
from **wtpy**.apps.datahelper import DHFactory as DHF

hlper = DHF.createHelper("tushare")
```

Example 3 (unknown):
```unknown
hlper.auth(**{"token":"your token of tushare","use_pro":True})
```

Example 4 (unknown):
```unknown
# 将代码列表下载到文件中
hlper.dmpCodeListToFile(filename = 'codes.json', hasStock = True, hasIndex = True)

# 将除权因子下载到文件中
hlper.dmpAdjFactorsToFile(codes=['SSE.600000','SZSE.000001'], filename="./adjfactors.json")

# 将K线下载到指定目录
hlper.dmpBarsToFile("./", codes = ['SSE.600000','SZSE.000001'], period="day")
```

---

## WtDHFactory

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/tools/wtdhfactory.html

**Contents:**
- WtDHFactory
- WtDHFactory是什么
- WtDHFactory怎么用

无论是策略研究还是实盘交易，都离不开数据。使用WonderTrader做量化的第一阶段，需要先把历史数据准备好。

但是我们在处理历史数据的时候，一开始要面临的问题就是数据源的选择。市面上的数据源有免费的，也有收费的。当然收费的数据源质量相对要高一些，但是对于初学者来说，免费的数据源也是足够的。

WonderTrader为了方便用户使用，在wtpy中提供了一个数据辅助模块WtDHFactory，可以帮助用户从不同的数据源中获取数据，然后统一导出到文件或者数据库中，方便后续使用。

目前WtDHFactory支持的数据源有：

其实在基本用法/历史数据处理中，已经介绍了如何使用WtDHFactory来准备历史数据了。

WtDHFactory采用工厂模式，使用的时候只需要传入对应的数据源名称，就可以获取到对应的数据源对象，然后调用对应的接口即可。具体用法可以参考以下代码：

更多详细用法，可以参考demo: wtpy/demos/test_datafactory

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
class BaseDataHelper:

    def dmpCodeListToFile(self, filename:str, hasIndex:bool=True, hasStock:bool=True):
        '''
        将代码列表导出到文件
        @filename   要输出的文件名，json格式
        @hasIndex   是否包含指数
        @hasStock   是否包含股票
        '''
        pass

    def dmpAdjFactorsToFile(self, codes:list, filename:str):
        '''
        将除权因子导出到文件
        @codes  股票列表，格式如["SSE.600000","SZSE.000001"]
        @filename   要输出的文件名，json格式
        '''
        pass

    def dmpBarsToFile(self, folder:str, codes:list, start_date:datetime=None, end_date:datetime=None, period:str="day"):
        '''
        将K线导出到指定的目录下的csv文件，文件名格式如SSE.600000_d.csv
        @folder 要输出的文件夹
        @codes  股票列表，格式如["SSE.600000","SZSE.000001"]
        @start_date 开始日期，datetime类型，传None则自动设置为1990-01-01
        @end_date   结束日期，datetime类型，传None则自动设置为当前日期
        @period K线周期，支持day、min1、min5
        '''
        pass

    def dmpBars(self, codes:list, cb, start_date:datetime=None, end_date:datetime=None, period:str="day"):
        '''
        将K线导出到指定的目录下的csv文件，文件名格式如SSE.600000_d.csv
        @cb     回调函数，格式如cb(exchg:str, code:str, firstBar:POINTER(WTSBarStruct), count:int, period:str)
        @codes  股票列表，格式如["SSE.600000","SZSE.000001"]
        @start_date 开始日期，datetime类型，传None则自动设置为1990-01-01
        @end_date   结束日期，datetime类型，传None则自动设置为当前日期
        @period K线周期，支持day、min1、min5
        '''
        pass
```

Example 2 (python):
```python
from wtpy.apps.datahelper import DHFactory as DHF

hlper = DHF.createHelper("baostock")
hlper.auth()

# tushare
# hlper = DHF.createHelper("tushare")
# hlper.auth(**{"token":"xxxxxxxxxxx", "use_pro":True})

# rqdata
# hlper = DHF.createHelper("rqdata")
# hlper.auth(**{"username":"00000000", "password":"0000000"})

# 落地股票列表
# hlper.dmpCodeListToFile("stocks.json")

# 下载K线数据
# hlper.dmpBarsToFile(folder='./', codes=["CFFEX.IF.HOT","CFFEX.IC.HOT"], period='min1')
# hlper.dmpBarsToFile(folder='./', codes=["CFFEX.IF.HOT","CFFEX.IC.HOT"], period='min5')
hlper.dmpBarsToFile(folder='./', codes=["SZSE.399005","SZSE.399006","SZSE.399303"], period='day')

# 下载复权因子
# hlper.dmpAdjFactorsToFile(codes=["SSE.600000",'SZSE.000001'], filename="./adjfactors.json")

# 将数据直接落地成dsb
def on_bars_block(exchg:str, stdCode:str, firstBar:POINTER(WTSBarStruct), count:int, period:str):
    from wtpy.wrapper import WtDataHelper
    dtHelper = WtDataHelper()
    if stdCode[-4:] == '.HOT':
        stdCode = stdCode[:-4] + "_HOT"
    else:
        ay = stdCode.split(".")
        if exchg == 'CZCE':
            stdCode = ay[1] + ay[2][1:]
        else:
            stdCode = ay[1] + ay[2]

    filename = f"../storage/his/{period}/{exchg}/"
    if not os.path.exists(filename):
        os.makedirs(filename)
    filename += f"{stdCode}.dsb"
    if period == "day":
        period = "d"
    elif period == "min1":
        period = "m1"
    else:
        period = "m5"
    dtHelper.store_bars(filename, firstBar, count, period)
    pass

hlper.dmpBars(codes=["CFFEX.IF.2103"], cb=on_bars_block, start_date=20201201, end_date=20210316, period="min5")
```

---

## 基础文件详解

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/usage/common_files.html

**Contents:**
- 基础文件详解
- commodities.json
- stk_comms.json
- sopt_comms.json
- contracts.json
- stocks.json
- stk_options.json
- fee.json
- holidays.json
- hots.json

一般放在common文件夹中，这里感谢@ZzzzHeJ的整理

品种信息表，记录不同交易所中不同品种的相关信息，基本格式如下：(由于json无法使用注释，因此这里采用python的格式写，如果要使用请把注释删除或直接使用demo中的文件)

股票信息表，格式同commodities.json

股票期权信息表，格式同commodities.json

合约信息表，记录合约的具体信息，基本格式如下：

股票相关信息，格式同contracts.json

股票期权信息，格式同contracts.json

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
{
    "CFFEX": {  # 交易所
        "IC": {  # 品种名
            "covermode": 0,  # 平仓类型，表示该品种支持的开平模式
                             # 0=开平，1=开平昨平今，2=平未了结的，3=不区分开平
            "pricemode": 0,  # 价格模式 0=市价限价 1=仅限价 2=仅市价
            "category": 1,  # 分类，参考CTP
                            # 0=股票 1=期货 2=期货期权 3=组合 4=即期
                            # 5=期转现 6=现货期权(股指期权) 7=个股期权(ETF期权)
                            # 20=数币现货 21=数币永续 22=数币期货 23=数币杠杆 24=数币期权
            "trademode": 0,  # 交易模式，0=多空都支持 1=只支持做多 2=只支持做多且T+1
            "precision": 1,  # 价格小数点位数
            "pricetick": 0.2,  # 最小价格变动单位
            "volscale": 200,  # 合约倍数
            "name": "中证",  # 名称
            "exchg": "CFFEX",  # 所属交易所
            "session": "SD0930",  # 交易时间，具体参考session配置文件
            "holiday": "CHINA"  # 节假日，具体参考holiday.json
        },
        "IF": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "trademode": 0,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 300,
            "name": "沪深",
            "exchg": "CFFEX",
            "session": "SD0930",
            "holiday": "CHINA"
        },
    }
    "CZCE": {
        "AP": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "trademode": 0,
            "precision": 0,
            "pricetick": 1.0,
            "volscale": 10,
            "name": "苹果",
            "exchg": "CZCE",
            "session": "FD0900",
            "holiday": "CHINA"
        },
    }
}
```

Example 2 (unknown):
```unknown
{
    "SSE" : {
        "STK" : {
            "category" : 0,
            "covermode" : 0,
            "exchg" : "SSE",
            "holiday" : "CHINA",
            "name" : "上证股票",
            "precision" : 2,
            "pricemode" : 1,
            "pricetick" : 0.01,
            "session" : "SD0930",
            "volscale" : 1,
            "trademode": 2
        },
        "IDX" : {
            "category" : 0,
            "covermode" : 0,
            "exchg" : "SSE",
            "holiday" : "CHINA",
            "name" : "上证指数",
            "precision" : 2,
            "pricemode" : 1,
            "pricetick" : 0.01,
            "session" : "SD0930",
            "volscale" : 1
        }
    },
}
```

Example 3 (unknown):
```unknown
{
    "SSE": {
        "ETFO": {
            "covermode": 0,
            "pricemode": 0,
            "category": 7,
            "precision": 4,
            "pricetick": 0.0001,
            "volscale": 10000,
            "name": "上证ETF期权",
            "exchg": "SSE",
            "session": "SD0930",
            "holiday": "CHINA"
        }
    },
    "SZSE": {
        "ETFO": {
            "covermode": 0,
            "pricemode": 0,
            "category": 7,
            "precision": 4,
            "pricetick": 0.0001,
            "volscale": 10000,
            "name": "深证ETF期权",
            "exchg": "SZSE",
            "session": "SD0930",
            "holiday": "CHINA"
        }
    }
}
```

Example 4 (unknown):
```unknown
{
    "CFFEX": {
        "IC2108": {
            "name": "中证2108",     # 名称
            "code": "IC2108",       # 代码
            "exchg": "CFFEX",       # 交易所
            "product": "IC",        # 品种
            "maxlimitqty": 20,      # 限价单单笔最大委托数量
            "maxmarketqty": 10      # 市价单单笔最大委托数量
        },
        "IC2109": {
            "name": "中证2109",
            "code": "IC2109",
            "exchg": "CFFEX",
            "product": "IC",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
    }
}
```

---

## 延迟优化日记

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/utils/latency.html

**Contents:**
- 延迟优化日记
- 测试环境
- 第一阶段 UFT拆分，完成后系统延迟在1us
- 第二阶段 时间函数，完成后系统延迟在500ns
- 第三阶段 字符串优化，完成以后系统延迟在250ns
- 第四阶段 内存分配优化，完成以后系统延迟在185ns
- 第五阶段 hash优化，完成以后系统延迟在175ns

测试环境 Intel i9-10980 XE，未超频，未关闭超线程

弃用内部标准代码SHFR.rb.2205，全部改成SHFE.rb2205，减少转换的开销

去掉对主力次主力的支持，减少tick数据进来以后，对主力合约判断的开销

下单接口去掉usertag，不在TraderAdapter做任何数据落地，减少策略下单接口的开销

将WTSContractInfo和WTSCommodityInfo以及WTSSessionInfo在初始化的之后做直接关联，在使用的过程中不用再去查找，减少查找的开销

将接口相关的数据类型，新增一个WTSContractInfo指针成员变量，每次构造的时候设置该指针，减少查找的开销

延迟测试工具中，没次模拟tick都TimeUtils获取最新的时间，但是时间函数开销很大，而且win下调用TimeUtils::getDateTime比linux下快，导致linux测试的速度在2us左右

因为实盘情况下，ontick触发时间，行情接口已经把处理好的数据给parser了，所以parser只需要做数据转换就可以了，不用自己去读取数据，测试工具去掉获取时间的逻辑

开始性能探查器进行分析，发现StrUtils::split()、sprintf开销很大

关键路径上调用StrUtil::split的地方改成自己查找，不用split

sprintf改成预分配的char[]，调用fmt::format_to进行格式化处理提高效率

将WTSDataDef中所有的std::string成员全部改成char[]，并提供直接访问内存的接口，减少赋值过程中的二次构造

继续分析发现，瓶颈集中在new/delete，调研以后决定用boost的object_pool

新增一个模板类WTSPoolObject，内部用boost::object_pool做一个对象池，管理对象的分配，派生类将类型作为模板参数传入

将频繁分配的对象都从WTSPoolObject进行派生，这样频繁创建和释放的对象都放在池子里进行管理，减少了new/delete的开销

继续分析发现，瓶颈变成了WTSBaseDataMgr的getContract，这下面使用robin_hashmap作为容器，已经是性能最高的容器了，再优化就需要从key着手了

于是将key改成int64[]，然后自己实现hash函数，处理key中的整数（整数计算会快），提升hashmap查找的效率，减少了约10ns的开销

© 版权所有 WonderTrader。 版本 280b46ec.

---

## WtBtSnooper

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/tools/wtbtsnooper.html

**Contents:**
- WtBtSnooper
- WtBtSnooper是什么
- WtBtSnooper的实现
- WtBtSnooper怎么用

我们在做策略回测的时候，除了看绩效指标风控指标以外，还有一个很重要的一点，就是查看一下交易信号。WonderTrader回测的时候会自动生成交易信号，可以通过trades.csv和closes.csv进行查看。

虽然通过调用WtBtAnalyst可以生成回测的excel绩效报告，但是我们还是希望能够通过图形化的方式来查看回测的绩效报告，这样更加直观。WtBtSnooper就是为了解决这个问题的，不光是提供了图形化的界面，更为重要的是，WtBtSnooper结合WtDtServo，提供了回测交易信号的展示。

整个WtBtSnooper采用的是b/s结构，wtpy.monitor.WtBtSnooper利用fastapi实现一个http的server，这个server包含html的文件服务，通过url就可以访问一个web页面，在页面上进行配置就可以查看。

要使用WtBtSnooper，只需要启动一下服务即可，代码如下：

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
from wtpy.monitor import WtBtSnooper
from wtpy import WtDtServo

def testBtSnooper():    

    dtServo = WtDtServo()
    # 这里配置的是基础数据文件目录
    dtServo.setBasefiles(folder="E:\\gitlocal\\MyStras\\CTA\\common\\")

    # 这里配置的是datakit落地的数据目录
    dtServo.setStorage(path='E:/storage/')

    snooper = WtBtSnooper(dtServo)
    snooper.run_as_server(port=8081)

testBtSnooper()
# 运行了服务以后，在浏览器打开以下网址即可使用
# http://127.0.0.1:8081/backtest/backtest.html
```

---

## 其他资料

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/utils/index.html

**Contents:**
- 其他资料

© 版权所有 WonderTrader。 版本 280b46ec.

---
