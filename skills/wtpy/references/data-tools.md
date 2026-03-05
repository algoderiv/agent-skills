# Wtpy-Github - Wtpy

**Pages:** 3

---

## testDtHelper

**URL:** https://dumengru.github.io/docs_wondertrader/wtpy/folder3/folder01/file02.html

**Contents:**
- testDtHelper
- 简介
- 代码介绍
  - 时间处理
  - 函数调用
  - 数据转存示例
  - 成功示例
  - 附录: tick数据转存代码

source: wtpy/folder3/folder01/file02.md

该示例主要帮助进行数据类型转换, WTCPP常用数据类型有三类

示例中的 PickleReader 是专门用来处理Python专用数据类型 "pkl" 的, 暂时没用到, 不做介绍. 本节主要讨论 "csv" 和 "dsb" 互转

数据转存逻辑其实很简单, 因为所有逻辑都被封装在了底层, 上层直接调用WtDtHelper中的方法即可(话说好像wtpy所有模块都是如此…)

前两个函数很好用, 直接传入对应目录名即可, strFilter 参数是多余的.

后两个函数涉及到数据类型转换, 稍微复杂点, 但逻辑很简单, 将数据存放到连续的内存段中, 然后传入内存段首地址即可.

建议不要使用demo中给出的 CsvReader 函数读取csv文件, 因为它要求你的csv文件必须有固定的格式, 但是每个人的csv文件格式应该都不大一样, 所以最好自己写(直接pd.read_csv即可 …).

这里给出完整示例. 因为我对Python和C++混编也不是很熟, 所以参考了其他朋友给出的示例(原网址 https://gitee.com/panyunan/wt4elegantrl-doc)

1.原始数据, 接上篇文章 "test_datafact" 从baostock下载的日数据

填充方式和逻辑与bar相同, 注意事项, 底层默认数据精度3位小数, 如果你的品种数据精度大于3, 那也只保留3位

**Examples:**

Example 1 (unknown):
```unknown
# 读取csv文件
df = pd.read_csv("SZSE.399005_d.csv")
# 获取数据长度
count = df.shape[0]
# 创建结构体缓存
BUFFER = WTSBarStruct * count
buffer = BUFFER()
# 填充数据
for index, row in df.iterrows():
    curbar = buffer[index]
    curbar.date = strToDate(row["date"])
    curbar.open = float(row["open"])
    curbar.high = float(row["high"])
    curbar.low = float(row["low"])
    curbar.close = float(row["close"])
    curbar.vol = int(row["volume"])
    curbar.money = int(row["turnover"])
# csv->dsb
dtHelper = WtDataHelper()
dtHelper.store_bars(barFile="SZSE.399005_d.dsb", firstBar=buffer, count=count, period="d")
# dsb->csv
dtHelper.dump_bars(binFolder="./", csvFolder="dsb_csv")
```

Example 2 (unknown):
```unknown
# 读取csv文件
df = pd.read_csv("SZSE.399005_d.csv")
# 获取数据长度
count = df.shape[0]
# 创建结构体缓存
BUFFER = WTSBarStruct * count
buffer = BUFFER()
# 填充数据
for index, row in df.iterrows():
    curbar = buffer[index]
    curbar.date = strToDate(row["date"])
    curbar.open = float(row["open"])
    curbar.high = float(row["high"])
    curbar.low = float(row["low"])
    curbar.close = float(row["close"])
    curbar.vol = int(row["volume"])
    curbar.money = int(row["turnover"])
# csv->dsb
dtHelper = WtDataHelper()
dtHelper.store_bars(barFile="SZSE.399005_d.dsb", firstBar=buffer, count=count, period="d")
# dsb->csv
dtHelper.dump_bars(binFolder="./", csvFolder="dsb_csv")
```

Example 3 (python):
```python
from wtpy.WtCoreDefs import WTSBarStruct, WTSTickStruct, PriceQueueType


df = pd.read_csv("EURUSD_t.csv")
count = df.shape[0]
BUFFER = WTSTickStruct * count
buffer = BUFFER()
for index, row in df.iterrows():
    curbar = buffer[index]
    curbar.exchg = bytes(row["exchg"], encoding="utf-8")
    curbar.code = bytes(row["code"], encoding="utf-8")
    curbar.price = float(row["price"])
    curbar.open = float(row["open"])
    curbar.high = float(row["high"])
    curbar.low = float(row["low"])
    curbar.settle_price = float(row["settle_price"])
    curbar.trading_date = int(row["trading_date"])
    curbar.action_date = int(row["action_date"])
    curbar.action_time = int(row["action_time"])
    curbar.bid_prices = (PriceQueueType)(row["bid"])
    curbar.ask_prices = (PriceQueueType)(row["ask"])
dtHelper = WtDataHelper()
dtHelper.store_ticks(tickFile="EURUSD_t.dsb", firstTick=buffer, count=count)
dtHelper.dump_ticks(binFolder="./", csvFolder="dsb_csv")
```

Example 4 (python):
```python
from wtpy.WtCoreDefs import WTSBarStruct, WTSTickStruct, PriceQueueType


df = pd.read_csv("EURUSD_t.csv")
count = df.shape[0]
BUFFER = WTSTickStruct * count
buffer = BUFFER()
for index, row in df.iterrows():
    curbar = buffer[index]
    curbar.exchg = bytes(row["exchg"], encoding="utf-8")
    curbar.code = bytes(row["code"], encoding="utf-8")
    curbar.price = float(row["price"])
    curbar.open = float(row["open"])
    curbar.high = float(row["high"])
    curbar.low = float(row["low"])
    curbar.settle_price = float(row["settle_price"])
    curbar.trading_date = int(row["trading_date"])
    curbar.action_date = int(row["action_date"])
    curbar.action_time = int(row["action_time"])
    curbar.bid_prices = (PriceQueueType)(row["bid"])
    curbar.ask_prices = (PriceQueueType)(row["ask"])
dtHelper = WtDataHelper()
dtHelper.store_ticks(tickFile="EURUSD_t.dsb", firstTick=buffer, count=count)
dtHelper.dump_ticks(binFolder="./", csvFolder="dsb_csv")
```

---

## hft_fut_bt.runBT

**URL:** https://dumengru.github.io/docs_wondertrader/wtpy/folder3/folder02/file01.html

**Contents:**
- hft_fut_bt.runBT
- 简介
- 回测引擎
  - engine.init
    - 时间偏移
  - engine.configBacktest
  - engine.configBTStorage
  - engine.commitBTConfig
  - straInfo = HftStraDemo
  - engine.set_hft_strategy

source: wtpy/folder3/folder02/file01.md

这个示例主要是为了展示HFT策略的回测流程, 同目录下的 runTrain 是做强化学习的示例, 暂且搁置吧

回测主要用到 WtBtEngine, 该类中有几个重要的属性

这里解释下为何 sessions.json 文件最复杂, 因为WT为了控制夜盘和日盘统一, 用到了"时间偏移"的概念, 配置中的 offset 就是为了控制偏移的分钟数.

以这段配置为例, offset: 300, 所有传入的时间都会向后推移300分钟, 所以 2100 会变成 0200.

"why?" 2100是数字时间, 变成真正时间应该是 "21:00", 向后偏移300分钟即5小时, 即第二天凌晨"2:00", 再转为数字时间 0200.

所有时间向后偏移300分钟后, 夜盘的时间就会小于日盘, 即 2100 小于 0200, 900 变为 1400, 0200 < 1400

有人会问, 为什么是300, 而不是250? 其实无所谓, 自己算算账, 保证夜盘时间转换后小于日盘时间即可.

设置回测起始时间和结束时间, 注意是数字时间, 精确到分钟.

WTPY底层几乎全部用的是数字时间, "9:00", 变为 0900等等(这部分内容前面的文章应该提到过)

设置回测的数据模式和数据存储目录, 数据模型主要是 "csv" 和 "bin" 格式, 其实首先会从自定义的 extloader 加载数据, 如果加载不到, 再按 csv/bin 加载.

还要注意数据存放目录的格式, 如果是自己准备的数据, 数据文件名和文件夹名必须按照 "storage/" 模板来写, 如果是用wtpy接收的市场数据, 收盘之后会自动处理成标准格式.

提交配置, 将配置参数传入回测引擎 self.__wrapper__.config_backtest -> self.api.config_backtest

创建策略对象, HftStraDemo 就是策略示例文件, 编写策略按照这个模板就好

传入策略对象, 创建策略环境, 创建策略上下文管理器(即, HftStraDemo中的参数 context: HftContext)

如果你的运行结果是乱码, 需要修改日志输出的编码格式. 有以下几个地方需要改动

以上地方修改后应该就可以正常运行HFT策略的 “runBT.py” 文件了, 但是为了以后方便, 建议将其他地方编码一起改了吧

cta_log_text 和 sel_log_text 仿照上述 hft_log_text 修改

回测输出文件保存在 "outputs_bt" 中

**Examples:**

Example 1 (unknown):
```unknown
"FN0100":{
    "name":"期货夜盘0100",
    "offset": 300,
    "auction":{
        "from": 2059,
        "to": 2100
    },
    "sections":[
        {
            "from": 2100,
            "to": 100
        },
        {
            "from": 900,
            "to": 1015
        },
        {
            "from": 1030,
            "to": 1130
        },
        {
            "from": 1330,
            "to": 1500
        }
    ]
},
```

Example 2 (unknown):
```unknown
"FN0100":{
    "name":"期货夜盘0100",
    "offset": 300,
    "auction":{
        "from": 2059,
        "to": 2100
    },
    "sections":[
        {
            "from": 2100,
            "to": 100
        },
        {
            "from": 900,
            "to": 1015
        },
        {
            "from": 1030,
            "to": 1130
        },
        {
            "from": 1330,
            "to": 1500
        }
    ]
},
```

Example 3 (unknown):
```unknown
hft0:
    error_rate: 30
    # 策略工厂名称
    module: WzHftStraFact
    strategy:
        # 策略名称
        name: OrderImbalance
        # 策略参数
        params:
            active_sections:
            -   end: 1457
                start: 931
            beta_0: 0.01171
            # 交易品种, 注意格式
            code: CFFEX.IF.HOT
            ...
    # 是否使用最新价
    use_newpx: true
```

Example 4 (unknown):
```unknown
hft0:
    error_rate: 30
    # 策略工厂名称
    module: WzHftStraFact
    strategy:
        # 策略名称
        name: OrderImbalance
        # 策略参数
        params:
            active_sections:
            -   end: 1457
                start: 931
            beta_0: 0.01171
            # 交易品种, 注意格式
            code: CFFEX.IF.HOT
            ...
    # 是否使用最新价
    use_newpx: true
```

---

## test_datafact

**URL:** https://dumengru.github.io/docs_wondertrader/wtpy/folder3/folder01/file01.html

**Contents:**
- test_datafact
- 简介
- 运行流程
  - 代码解析
- 成功示例

source: wtpy/folder3/folder01/file01.md

该示例主要是从外部API接口获取相关数据并保存到本地,

这三个接口在使用前都需要安装对应的python包(pip install …)等, 实际上在不改源码的情况下你需要一次性把三个包都安装, 否则运行示例会提示缺少对应包.

API详情建议自己对应官网查看, 包括如何申请账号, 哪些数据直接免费, 哪些数据需要花钱等.

1.DHFactory 负责创建数据接口对象 2.创建完后真正执行程序的是 "wtpy/apps/datahelper/"下对应的py文件

---
