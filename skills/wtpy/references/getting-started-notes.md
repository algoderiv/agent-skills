# Wtpy-Zz - Getting Started

**Pages:** 10

---

## 3.开始使用

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/3.%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/

**Contents:**
- 3.开始使用

---

## 策略配置文件

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/%E7%AD%96%E7%95%A5%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6.html

**Contents:**
- 策略配置文件
- configbt.yaml
- config.yaml
- actpolicy.yaml
- executers.yaml
- filters.yaml
- logcfg.yaml
- logcfgbt.yaml
- tdparsers.yaml
- tdtraders.yaml

使用不同引擎的策略需要的策略配置文件不同，回测与仿真/实盘也有所区别，可以在对应的demo中，获取对应的配置文件信息

策略配置文件可以采用yaml格式或者json格式，由于json格式无法添加注释，因此现在已经逐渐替换为yaml格式了

策略配置文件一般与策略可执行程序或run.py位于同级目录中

WT的一个重要优势，就是净持仓交易，对于期货这种交易流程较为复杂的品种来说尤其方便。策略将生成多或空信号后，交给执行器转换为实际的开平指令，发给交易所。而开平指令的生成就是根据actpolicy.yaml文件中一个个执行策略产生的。

如果使用的品种不支持平今平昨，wt内部会自动转为普通的平仓

执行器，主要用于cta引擎，通过配置执行器，将信号的执行细节从策略中剥离出来，同时可以将同一个策略的信号，分发到多个交易通道（账户）中，实现多账户。

当盘中或策略出现突发情况时，有时需要针对某个策略，甚至品种进行人工干预，这就需要暂时过滤掉这个品种或者策略的信号，这一功能是由filters.yaml实现的。

logcfg.yaml中规定了各类日志的输出格式以及目录，包括以下几类日志

每个日志配置需包括是否异步、日志等级、槽设置。槽设置包括日志保存路径、格式、是否截断保存、日志类型等

与logcfg.yaml同样是配置日志文件，区别在于logcfgbt.yaml用于回测日志的输出

tdparsers.yaml用于配置行情服务，通常可以向自己所用的券商申请，或者通过simnow注册获取

tdtraders.yaml用于配置交易服务，与tdparsers一样，通过券商或者simnow获取

**Examples:**

Example 1 (unknown):
```unknown
replayer:                           # 数据回放设置
    basefiles:                      # 各个基础配置文件所在目录
        commodity: ../common/commodities.json
        contract: ../common/contracts.json
        holiday: ../common/holidays.json
        hot: ../common/hots.json
        second: ../common/seconds.json
        session: ../common/sessions.json
    fees: ../common/fees.json
    stime: 202101040900             # 回测开始时间
    etime: 202101061500             # 回测结束时间
    mode: csv                       # 数据回放模式，包括csv与bin
                                    # 不管是何种模式，首先回去his目录下查找
                                    # 如果his没有的话，csv模式会去csv模式下查找对应的csv文件，
                                    # 并转换为his目录下的dsb文件。
    path: ../storage/               # 数据存放路径
    tick: true                      # 是否开启tick回测，HFT回测时必须开启
env:
    mocker: cta                     # 回测引擎，cta/hft/sel/uft
    slippage: 1                     # 滑点，仅cta sel引擎的滑点有效

# CTA策略配置，当mocker为cta时会读取该配置项
cta:
    module: WtCtaStraFact.dll       # 模块名，linux下为xxxx.so
    strategy:                       # 策略信息
        id: dt_if                   # 策略ID，自定义的
        name: DualThrust            # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供，不要被示例中所限制
            code: CFFEX.IF.HOT
            count: 50
            days: 30
            k1: 0.6
            k2: 0.6
            period: m5
            stock: false

# HFT策略配置，当mocker为hft时会读取该配置项
hft:
    module: WtHftStraFact.dll       # 模块名，linux下为xxxx.so
    use_newpx: true                # hft与uft中特有的选项，是否仅用最新价撮合，为false则会采用盘口撮合
    agent: true                     # 允许hft想cta一样记录成交信息
    strategy:                       # 策略信息
        id: hft_if                  # 策略ID，自定义的
        name: SimpleHft             # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供
            code: CFFEX.IF.HOT
            second: 5
            freq: 10
            offset: 0
            stock: false

# UFT策略配置，当mocker为uft时会读取该配置项
uft:
    module: WtUftStraFact.dll       # 模块名，linux下为xxxx.so
    use_newpx: true                # hft与uft中特有的选项，是否仅用最新价撮合，为false则会采用盘口撮合
    strategy:                       # 策略信息
        id: uft_if                  # 策略ID，自定义的
        name: SimpleUft             # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供
            code: CFFEX.IF.HOT
            second: 5
            freq: 10
            offset: 0
            lots: 1
```

Example 2 (unknown):
```unknown
replayer:                           # 数据回放设置
    basefiles:                      # 各个基础配置文件所在目录
        commodity: ../common/commodities.json
        contract: ../common/contracts.json
        holiday: ../common/holidays.json
        hot: ../common/hots.json
        second: ../common/seconds.json
        session: ../common/sessions.json
    fees: ../common/fees.json
    stime: 202101040900             # 回测开始时间
    etime: 202101061500             # 回测结束时间
    mode: csv                       # 数据回放模式，包括csv与bin
                                    # 不管是何种模式，首先回去his目录下查找
                                    # 如果his没有的话，csv模式会去csv模式下查找对应的csv文件，
                                    # 并转换为his目录下的dsb文件。
    path: ../storage/               # 数据存放路径
    tick: true                      # 是否开启tick回测，HFT回测时必须开启
env:
    mocker: cta                     # 回测引擎，cta/hft/sel/uft
    slippage: 1                     # 滑点，仅cta sel引擎的滑点有效

# CTA策略配置，当mocker为cta时会读取该配置项
cta:
    module: WtCtaStraFact.dll       # 模块名，linux下为xxxx.so
    strategy:                       # 策略信息
        id: dt_if                   # 策略ID，自定义的
        name: DualThrust            # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供，不要被示例中所限制
            code: CFFEX.IF.HOT
            count: 50
            days: 30
            k1: 0.6
            k2: 0.6
            period: m5
            stock: false

# HFT策略配置，当mocker为hft时会读取该配置项
hft:
    module: WtHftStraFact.dll       # 模块名，linux下为xxxx.so
    use_newpx: true                # hft与uft中特有的选项，是否仅用最新价撮合，为false则会采用盘口撮合
    agent: true                     # 允许hft想cta一样记录成交信息
    strategy:                       # 策略信息
        id: hft_if                  # 策略ID，自定义的
        name: SimpleHft             # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供
            code: CFFEX.IF.HOT
            second: 5
            freq: 10
            offset: 0
            stock: false

# UFT策略配置，当mocker为uft时会读取该配置项
uft:
    module: WtUftStraFact.dll       # 模块名，linux下为xxxx.so
    use_newpx: true                # hft与uft中特有的选项，是否仅用最新价撮合，为false则会采用盘口撮合
    strategy:                       # 策略信息
        id: uft_if                  # 策略ID，自定义的
        name: SimpleUft             # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供
            code: CFFEX.IF.HOT
            second: 5
            freq: 10
            offset: 0
            lots: 1
```

Example 3 (unknown):
```unknown
replayer:                           # 数据回放设置
    basefiles:                      # 各个基础配置文件所在目录
        commodity: ../common/commodities.json
        contract: ../common/contracts.json
        holiday: ../common/holidays.json
        hot: ../common/hots.json
        second: ../common/seconds.json
        session: ../common/sessions.json
    bspolicy: actpolicy.yaml        # 执行配置文件名
    data:                       
        store: 
            path: ../storage/       # 数据存放目录
            his_path: ./storage/his # 历史数据存放路径，如果历史数据单独存放，可以设置该选项
env:
    fees: \\WIN-52AMQLH0TIA\WonderTrader\common\fees.json
    filters: filters.json
    mode: product                   # 模式
    name: hft
    product:
        session: TRADING            # 交易时段，TRADING能够覆盖国内主要的交易时段
    mocker: cta                     # 回测引擎，cta/hft/sel/uft
    slippage: 1                     # 滑点，仅cta sel引擎的滑点有效

# CTA策略配置，当mocker为cta时会读取该配置项
cta:
    module: WtCtaStraFact.dll       # 模块名，linux下为xxxx.so
    strategy:                       # 策略信息
        id: dt_if                   # 策略ID，自定义的
        name: DualThrust            # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供，不要被示例中所限制
            code: CFFEX.IF.HOT
            count: 50
            days: 30
            k1: 0.6
            k2: 0.6
            period: m5
            stock: false

# HFT策略配置，当mocker为hft时会读取该配置项
hft:
    module: WtHftStraFact.dll       # 模块名，linux下为xxxx.so
    use_newpx: true                # hft与uft中特有的选项，是否仅用最新价撮合，为false则会采用盘口撮合
    strategy:                       # 策略信息
        id: hft_if                  # 策略ID，自定义的
        name: SimpleHft             # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供
            code: CFFEX.IF.HOT
            second: 5
            freq: 10
            offset: 0
            stock: false

# UFT策略配置，当mocker为uft时会读取该配置项
uft:
    module: WtUftStraFact.dll       # 模块名，linux下为xxxx.so
    use_newpx: true                # hft与uft中特有的选项，是否仅用最新价撮合，为false则会采用盘口撮合
    strategy:                       # 策略信息
        id: uft_if                  # 策略ID，自定义的
        name: SimpleUft             # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供
            code: CFFEX.IF.HOT
            second: 5
            freq: 10
            offset: 0
            lots: 1
```

Example 4 (unknown):
```unknown
replayer:                           # 数据回放设置
    basefiles:                      # 各个基础配置文件所在目录
        commodity: ../common/commodities.json
        contract: ../common/contracts.json
        holiday: ../common/holidays.json
        hot: ../common/hots.json
        second: ../common/seconds.json
        session: ../common/sessions.json
    bspolicy: actpolicy.yaml        # 执行配置文件名
    data:                       
        store: 
            path: ../storage/       # 数据存放目录
            his_path: ./storage/his # 历史数据存放路径，如果历史数据单独存放，可以设置该选项
env:
    fees: \\WIN-52AMQLH0TIA\WonderTrader\common\fees.json
    filters: filters.json
    mode: product                   # 模式
    name: hft
    product:
        session: TRADING            # 交易时段，TRADING能够覆盖国内主要的交易时段
    mocker: cta                     # 回测引擎，cta/hft/sel/uft
    slippage: 1                     # 滑点，仅cta sel引擎的滑点有效

# CTA策略配置，当mocker为cta时会读取该配置项
cta:
    module: WtCtaStraFact.dll       # 模块名，linux下为xxxx.so
    strategy:                       # 策略信息
        id: dt_if                   # 策略ID，自定义的
        name: DualThrust            # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供，不要被示例中所限制
            code: CFFEX.IF.HOT
            count: 50
            days: 30
            k1: 0.6
            k2: 0.6
            period: m5
            stock: false

# HFT策略配置，当mocker为hft时会读取该配置项
hft:
    module: WtHftStraFact.dll       # 模块名，linux下为xxxx.so
    use_newpx: true                # hft与uft中特有的选项，是否仅用最新价撮合，为false则会采用盘口撮合
    strategy:                       # 策略信息
        id: hft_if                  # 策略ID，自定义的
        name: SimpleHft             # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供
            code: CFFEX.IF.HOT
            second: 5
            freq: 10
            offset: 0
            stock: false

# UFT策略配置，当mocker为uft时会读取该配置项
uft:
    module: WtUftStraFact.dll       # 模块名，linux下为xxxx.so
    use_newpx: true                # hft与uft中特有的选项，是否仅用最新价撮合，为false则会采用盘口撮合
    strategy:                       # 策略信息
        id: uft_if                  # 策略ID，自定义的
        name: SimpleUft             # 策略名，要和factory中的匹配
        params:                     # 策略初始化参数，这个根据策略的需要提供
            code: CFFEX.IF.HOT
            second: 5
            freq: 10
            offset: 0
            lots: 1
```

---

## 环境部署

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/3.%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/4.%E7%8E%AF%E5%A2%83%E9%83%A8%E7%BD%B2.html

**Contents:**
- 环境部署
- 定时调度工具
- 行情录制工具
- 合约信息更新
- 主力更新

Wt不像掘金这类商务平台会帮忙维护信息。WT的用户需要自己来维护合约信息、主力换月等。WT提供了多个工具来帮助：

见WtMonSvr 通过WtMonSvr设置上述两个工具的定时运行，达到维护数据的效果。

见datakit 接受交易的实时数据，并盘后保存为历史数据，供回测研究使用

---

## Backtest 回测框架

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/1.Backtest.html

**Contents:**
- Backtest 回测框架
- WtBtCore
  - 代码结构

---

## 策略回测

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/3.%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/5.%E7%AD%96%E7%95%A5%E5%9B%9E%E6%B5%8B.html

**Contents:**
- 策略回测
- 历史数据
- 回测配置
- 回测环境

WT不提供历史行情，需要自行从其他数据源中获取，并转换为WT使用的DSB格式，笔者这里提供了从米筐获取数据源的代码，供大家参考使用,rqdata。demo中定义了一个Ifeed基类，其他数据源通过继承Ifeed，并实现get_tick与get_bar即可实现。

通过运行store_his_bar与store_his_tick可以将获取的数据进行转换，并按照特定的结构组织存储。

创建一个runBT.py文件，包含以下内容

wt支持实盘时添加多个策略，但回测时仅允许同时回测一个策略。

**Examples:**

Example 1 (python):
```python
from wtpy import WtBtEngine,EngineType              # 引入wtpy的回测引擎
from wtpy.apps import WtBtAnalyst                   # 引入分析工具

from Strategies.DualThrust import StraDualThrust    # 引入自己的策略

import os
import sys
os.chdir(sys.path[0])
if __name__ == "__main__":
    #创建一个运行环境，制定回测引擎为CTA，并制定日志配置文件
    engine = WtBtEngine(EngineType.ET_CTA,logCfg="logcfgbt.yaml")    
    # 初始化引擎，指定基础配置文件目录，以及策略配置文件
    engine.init('../common/', "configbt.yaml")
    # 确定回测时间，也可以在策略配置文件中指定
    engine.configBacktest(202201100930,202202011500)
    # 提交配置
    engine.commitBTConfig()

    # 初始化策略类
    straInfo1 = StraDualThrust(name='pydt_cu', code="SHFE.cu.HOT", barCnt=50, period="m5", days=30, k1=0.1, k2=0.1, isForStk=False)
    # 将策略加入到回测环境中
    engine.set_cta_strategy(straInfo1)
    # 开启回测
    engine.run_backtest()

    # 初始化分析器
    analyst = WtBtAnalyst()
    # 添加需要分析的策略，指定策略回测结果所在目录
    analyst.add_strategy("pydt_cu", folder="./outputs_bt/pydt_cu/", init_capital=500000, rf=0.02, annual_trading_days=240)
    # 开始分析
    analyst.run()

    kw = input('press any key to exit\n')
    # 结束，释放资源
    engine.release_backtest()
```

Example 2 (python):
```python
from wtpy import WtBtEngine,EngineType              # 引入wtpy的回测引擎
from wtpy.apps import WtBtAnalyst                   # 引入分析工具

from Strategies.DualThrust import StraDualThrust    # 引入自己的策略

import os
import sys
os.chdir(sys.path[0])
if __name__ == "__main__":
    #创建一个运行环境，制定回测引擎为CTA，并制定日志配置文件
    engine = WtBtEngine(EngineType.ET_CTA,logCfg="logcfgbt.yaml")    
    # 初始化引擎，指定基础配置文件目录，以及策略配置文件
    engine.init('../common/', "configbt.yaml")
    # 确定回测时间，也可以在策略配置文件中指定
    engine.configBacktest(202201100930,202202011500)
    # 提交配置
    engine.commitBTConfig()

    # 初始化策略类
    straInfo1 = StraDualThrust(name='pydt_cu', code="SHFE.cu.HOT", barCnt=50, period="m5", days=30, k1=0.1, k2=0.1, isForStk=False)
    # 将策略加入到回测环境中
    engine.set_cta_strategy(straInfo1)
    # 开启回测
    engine.run_backtest()

    # 初始化分析器
    analyst = WtBtAnalyst()
    # 添加需要分析的策略，指定策略回测结果所在目录
    analyst.add_strategy("pydt_cu", folder="./outputs_bt/pydt_cu/", init_capital=500000, rf=0.02, annual_trading_days=240)
    # 开始分析
    analyst.run()

    kw = input('press any key to exit\n')
    # 结束，释放资源
    engine.release_backtest()
```

---

## 3.开始使用

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/3.开始使用/

**Contents:**
- 3.开始使用

---

## 仿真与实盘

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/3.%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/6.%E4%BB%BF%E7%9C%9F%E4%B8%8E%E5%AE%9E%E7%9B%98.html

**Contents:**
- 仿真与实盘
- 行情工具
- 实盘配置
- 实盘环境

仿真或实盘中的实时行情，需要通过datakit工具实时接收，datakit将接受到的行情分发给策略。

与回测环境功能类似，创建一个run.py文件，包括以下内容

与回测环境不同的时，实盘允许通过多次调用add_cta_strategy添加多个策略

**Examples:**

Example 1 (python):
```python
from wtpy import WtEngine,EngineType                # 引入引擎
from Strategies.DualThrust import StraDualThrust    # 引入策略

from ConsoleIdxWriter import ConsoleIdxWriter       # 引入输出器
import os
import sys
os.chdir(sys.path[0])
if __name__ == "__main__":
    #创建一个运行环境，并加入策略
    env = WtEngine(EngineType.ET_CTA)               # 创建引擎
    env.init('../common/', "config.yaml")           # 初始化引擎，制定配置文件
                                                    # 初始化策略
    straInfo = StraDualThrust(name='pydt_au', code="SHFE.au.HOT", barCnt=50, period="m5", days=30, k1=0.2, k2=0.2, isForStk=False)        
    env.add_cta_strategy(straInfo)                  # 添加策略
    
    idxWriter = ConsoleIdxWriter()                  # 创建输出器，可选
    env.set_writer(idxWriter)                       # 设置输出器

    env.run()                                       # 启动

    kw = input('press any key to exit\n')
```

Example 2 (python):
```python
from wtpy import WtEngine,EngineType                # 引入引擎
from Strategies.DualThrust import StraDualThrust    # 引入策略

from ConsoleIdxWriter import ConsoleIdxWriter       # 引入输出器
import os
import sys
os.chdir(sys.path[0])
if __name__ == "__main__":
    #创建一个运行环境，并加入策略
    env = WtEngine(EngineType.ET_CTA)               # 创建引擎
    env.init('../common/', "config.yaml")           # 初始化引擎，制定配置文件
                                                    # 初始化策略
    straInfo = StraDualThrust(name='pydt_au', code="SHFE.au.HOT", barCnt=50, period="m5", days=30, k1=0.2, k2=0.2, isForStk=False)        
    env.add_cta_strategy(straInfo)                  # 添加策略
    
    idxWriter = ConsoleIdxWriter()                  # 创建输出器，可选
    env.set_writer(idxWriter)                       # 设置输出器

    env.run()                                       # 启动

    kw = input('press any key to exit\n')
```

---

## 版本选择

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/3.%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/1.%E7%89%88%E6%9C%AC%E9%80%89%E6%8B%A9.html

**Contents:**
- 版本选择

WT包括CPP版本，与封装得到的Python版本。那么，我们该如何选择哪个版本来使用呢？

综上所述，如果有以下需求，请选择CPP，否则使用Python版本即可：

本文使用教程部分主要以Python版本为主，但会配合CPP版本来讲解底层原理。

使用的所有代码都可以在wtpy_learning_demo中获得

---

## 编写策略

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/3.%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/3.%E7%BC%96%E5%86%99%E7%AD%96%E7%95%A5.html

**Contents:**
- 编写策略

使用Wt编写策略前，首先需要确定使用的引擎，为了应对不同的交易情景，WT提供了多种交易引擎供大家选择

根据自己的需求选择合适的引擎后，便可以开始编写策略了。CTA引擎是WT中最具有特色的引擎，也是有别于其他交易系统的引擎，后续的讲解主要以CTA引擎为例。

同大多数的量化框架一样，WT中的策略也采用事件驱动的方式。所谓事件驱动的意思是，策略的中逻辑的执行，是由一个个事件推动的，当一个事件发生时，策略便执行对应事件的回调函数，从而推动策略运行。

WT通过各种事件驱动策略执行，而策略则通过上下文对象主动与WT交互，不同的交易引擎提供的事件以及上下文有所不同，具体可以查阅对应交易引擎部分

策略继承自基础策略类BaseCtaStrategy，通过重写基类中的各个回调函数实现，具体流程如下：

**Examples:**

Example 1 (python):
```python
from wtpy import BaseCtaStrategy
from wtpy import CtaContext
import numpy as np

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

    def on_init(self, context:CtaContext):
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

    
    def on_calculate(self, context:CtaContext):
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

        lastBar = df_bars.get_bar(-1)
        timePx = lastBar["bartime"]

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
        now = context.stra_get_date()*10000 + timePx%10000
        
        # 向外输出指标
        context.write_indicator(tag=self.__period__, time=int(now), data={
            "highpx":highpx,
            "lowpx": lowpx,
            "upper_bound":upper_bound,
            "lower_bound":lower_bound,
            "current_position": curPos
        })

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
                # context.stra_log_text("向下突破%.2f<=%.2f，多仓出场" % (lowpx, lower_bound))
                #raise Exception("except on purpose")
                return
        else:
            if highpx >= upper_bound and not self.__is_stk__:
                context.stra_exit_short(code, 1*trdUnit, 'exitshort')
                context.stra_log_text("向上突破%.2f>=%.2f，空仓出场" % (highpx, upper_bound))
                return


    def on_tick(self, context:CtaContext, stdCode:str, newTick:dict):
        #context.stra_log_text ("on tick fired")
        return
```

Example 2 (python):
```python
from wtpy import BaseCtaStrategy
from wtpy import CtaContext
import numpy as np

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

    def on_init(self, context:CtaContext):
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

    
    def on_calculate(self, context:CtaContext):
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

        lastBar = df_bars.get_bar(-1)
        timePx = lastBar["bartime"]

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
        now = context.stra_get_date()*10000 + timePx%10000
        
        # 向外输出指标
        context.write_indicator(tag=self.__period__, time=int(now), data={
            "highpx":highpx,
            "lowpx": lowpx,
            "upper_bound":upper_bound,
            "lower_bound":lower_bound,
            "current_position": curPos
        })

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
                # context.stra_log_text("向下突破%.2f<=%.2f，多仓出场" % (lowpx, lower_bound))
                #raise Exception("except on purpose")
                return
        else:
            if highpx >= upper_bound and not self.__is_stk__:
                context.stra_exit_short(code, 1*trdUnit, 'exitshort')
                context.stra_log_text("向上突破%.2f>=%.2f，空仓出场" % (highpx, upper_bound))
                return


    def on_tick(self, context:CtaContext, stdCode:str, newTick:dict):
        #context.stra_log_text ("on tick fired")
        return
```

---

## 获取WTPY

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/3.%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/2.%E8%8E%B7%E5%8F%96wtpy.html

**Contents:**
- 获取WTPY

如果出现itsdangerous模块中无法获得json的问题，需要对itsdangerous进行降级执行pip install itsdangerous == 2.0.1

如果为设置代理的话，国内使用pip速度较慢，可以使用pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/设置pip的代理

如果安装后，使用报错，运行以下命令：pip install python-socketio==4.6.0 –upgradepip install python-engineio==3.13.2 –upgradepip install flask-socketio==4.3.1 –upgrade

pip install werkzeug==1.0.1 –upgradepip install itsdangerous==1.1.0 –upgradepip install MarkupSafe==1.1.1 –upgradepip install Jinja2==2.11.2 –upgrade

**Examples:**

Example 1 (cmd):
```cmd
pip install wtpy
```

---
