# Usage - Part 1

**Topics:** 策略程序回测, 合约, 行情和历史数据, TqSdk整体结构

---

## 策略程序回测

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/backtest.html

**Contents:**
- 策略程序回测
- 执行策略回测
- 在回测结束时获取回测详细信息
- 回测结束在浏览器中查看绘图结果
- 回测时获取主连合约标的
- 回测时的成交规则和推进
- 对股票合约进行回测
- 回测使用多行情序列的策略程序
- 了解更多

策略程序回测能让用户在不改变代码的情况下去回测自己的策略在历史行情的表现

用户也可以使用快期模拟账户来模拟运行来检验策略 模拟交易和论坛

使用 TqSdk 编写的策略程序，不需要修改策略代码，只需要在创建 api 实例时给backtest参数传入 TqBacktest , 策略就会进入历史回测模式:

使用tqsdk在回测结束后会输出交易记录和每日收盘时的账户资金情况，以及最大回撤、夏普比率等指标，这些数据可以导入到 excel 中或使用其他分析工具进一步处理。

要在回测结束时调用您自己写的代码, 可以使用 try/except 机制捕获回测结束信号 BacktestFinished, 像这样:

回测的详细信息保存在回测所用的模拟账户 TqSim 中, 可以直接访问它的成员变量 trade_log(格式为 日期->交易记录及收盘时的权益及持仓).

同时我们也提供简单的图形化的回测报告功能供大家使用 策略程序图形化界面 ，使用效果参考下图

要在回测结束时，如果依然需要在浏览器中查看绘图结果，同时又需要打印回测信息，您应该这样做:

在天勤中回测时，对于主连合约天勤不支持直接进行交易，如果用户要交易主连，我们推荐用户使用 quote.underlying_symbol 获取回测当时主连对应的主力合约来进行交易。

在天勤中回测时，除了期货、期权合约以外，我们还支持使用 指数 进行回测和在回测中交易，指数合约代码格式参见 合约, 行情和历史数据

策略回测时使用内置模拟账户 TqSim , 默认回测资金为1000w , 如果需要修改初始回测资金，只需给 TqSim 传入需要设定的金额即可:

撮合成交规则为对价成交. 即限价单的价格达到对手盘价格时判定为成交. 不会出现委托单部分成交的情况.

回测时策略程序报单, 会立即做一次成交判定.

回测框架的规则是当没有新的事件需要用户处理时才推进到下一个行情, 也就是这样:

TqSdk 在 3.2.0 版本后支持了对股票合约进行回测功能，在回测过程中用户需要初始化 TqSimStock 类，且该类只能支持股票模拟交易

由于股票市场 T+1 的规则, TargetPosTask 函数目前还不支持在股票交易中使用，股票合约交易时只支持使用 insert_order

如果您想要在回测中同时交易期货和股票合约，则可以使用 TqMultiAccount 来实现该需求:

TqSdk 允许一个策略程序中使用多个行情序列, 比如这样:

TqSdk回测框架使用一套复杂的规则来推进行情：

规则1: tick 序列(例如上面例子中的tsa) 总是按逐 tick 推进:

规则2: K线序列 (例如上面例子中的ka1, ka2) 总是按周期推进. 每根K线在创建时和结束时各更新一次:

规则4: 策略程序中的多个序列的更新, 按时间顺序合并推进. 每次 wait_update 时, 优先处理用户事件, 当没有用户事件时, 从各序列中选择下一次更新时间最近的, 更新到这个时间:

注意 ：如果未订阅 quote，模拟交易在下单时会自动为此合约订阅 quote ，根据回测时 quote 的更新规则，如果此合约没有订阅K线或K线周期大于分钟线 则会自动订阅一个分钟线 。

另外，对 组合合约 进行回测时需注意：只能通过订阅 tick 数据来回测，不能订阅K线，因为K线是由最新价合成的，而交易所发回的组合合约数据中无最新价。

如果你要做大量回测, 或者试图做参数优化/参数搜索, 请看 批量回测, 参数搜索及其它

如果你在回测时需要图形化界面支持，我们提供 TqSdk 内置强大的图形化界面解决方案 策略程序图形化界面

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from datetime import date
from tqsdk import TqApi, TqAuth, TqSim, TqBacktest

api = TqApi(TqSim(), backtest=TqBacktest(start_dt=date(2018, 5, 1), end_dt=date(2018, 10, 1)), auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
from tqsdk import BacktestFinished

acc = TqSim()

try:
  api = TqApi(acc, backtest=TqBacktest(start_dt=date(2018, 5, 1), end_dt=date(2018, 10, 1)), auth=TqAuth("快期账户", "账户密码"))
  #策略代码在这里
  #...

except BacktestFinished as e:
  # 回测结束时会执行这里的代码
  api.close()
  print(acc.trade_log)  # 回测的详细信息

  print(acc.tqsdk_stat)  # 回测时间内账户交易信息统计结果，其中包含以下字段
  # init_balance 起始资金
  # balance 结束资金
  # max_drawdown 最大回撤
  # profit_loss_ratio 盈亏额比例
  # winning_rate 胜率
  # ror 收益率
  # annual_yield 年化收益率
  # sharpe_ratio 年化夏普率
  # tqsdk_punchline 天勤点评
```

Example 3 (python):
```python
from tqsdk import BacktestFinished

acc = TqSim()

try:
  api = TqApi(acc, backtest=TqBacktest(start_dt=date(2018, 5, 1), end_dt=date(2018, 10, 1)), auth=TqAuth("快期账户", "账户密码"))
  #策略代码在这里
  #...
except BacktestFinished as e:
  print(acc.tqsdk_stat)  # 回测时间内账户交易信息统计结果，其中包含以下字段
  # 由于需要在浏览器中查看绘图结果，因此程序不能退出
  while True:
      api.wait_update()
```

Example 4 (python):
```python
from datetime import date
from tqsdk import TqApi, TqAuth, TqBacktest, BacktestFinished

api = TqApi(backtest=TqBacktest(start_dt=date(2020, 1, 1), end_dt=date(2020, 10, 1)), auth=TqAuth("快期账户", "账户密码"))

quote = api.get_quote("KQ.m@CFFEX.T")
print(quote.datetime, quote.underlying_symbol)
try:
    while True:
        api.wait_update()
        if api.is_changing(quote, "underlying_symbol"):
            print(quote.datetime, quote.underlying_symbol)
except BacktestFinished:
    api.close()

# 预期输出：
# 2019-12-31 15:14:59.999999 CFFEX.T2003
# 2020-02-19 09:15:00.000000 CFFEX.T2006
# 2020-05-14 09:15:00.000000 CFFEX.T2009
# 2020-08-19 09:30:00.000000 CFFEX.T2012
```

---


## 合约, 行情和历史数据

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/mddatas.html

**Contents:**
- 合约, 行情和历史数据
- 合约代码
- 实时行情
- K线数据
- Tick序列
- 关于合约及行情的一些常见问题

TqSdk中的合约代码, 统一采用 交易所代码.交易所内品种代码 的格式. 交易所代码为全大写字母, 交易所内品种代码的大小写规范, 遵从交易所规定, 大小写敏感.

其中 TqSdk 免费版本提供全部的期货、商品/金融期权和上证50、沪深300、中证500和中证1000的实时行情

购买或申请 TqSdk 专业版试用后可提供A股股票的实时和历史行情，具体免费版和专业版的区别，请点击 天勤量化专业版

快期 (所有主连合约, 指数都归属在这里)

需要注意郑商所的期货合约格式为合约字母大写，并且只有三位数字位，同时不同家交易所的期权代码格式也各不相同

天勤指数的计算方式为根据在市期货合约的昨持仓量加权平均

天勤主力的选定标准为该合约持仓量和成交量均为最大后，在下一个交易日开盘后进行切换，且切换后不会再切换到之前的合约

get_quote() 函数提供实时行情和合约信息:

对于每个合约, 只需要调用一次 get_quote 函数. 如果需要监控数据更新, 可以使用 wait_update():

get_kline_serial() 函数获取指定合约和周期的K线序列数据:

详细使用方法及说明请见 get_kline_serial() 函数使用说明。

get_kline_serial() 的返回值是一个 pandas.DataFrame, 包含以下列:

要使用K线数据, 请使用 pandas.DataFrame 的相关函数. 常见用法示例如下:

TqSdk中, K线周期以秒数表示，支持不超过1日的任意周期K线，例如:

TqSdk中最多可以获取每个K线序列的最后8000根K线，无论哪个周期。也就是说，你如果提取小时线，最多可以提取最后8000根小时线，如果提取分钟线，最多也是可以提取最后8000根分钟线。

对于每个K线序列, 只需要调用一次 get_kline_serial() . 如果需要监控数据更新, 可以使用 wait_update()

如果只想在新K线出现时收到信号, 可以配合使用 is_changing():

get_tick_serial() 函数获取指定合约的Tick序列数据:

get_tick_serial() 的返回值是一个 pandas.DataFrame, 常见用法示例如下:

tick序列的更新监控, 与K线序列采用同样的方式.

TqSdk可以订阅任意多个行情和K线, 并在一个wait_update中等待更新. 像这样:

关于 wait_update() 和 is_changing() 的详细说明, 请见 策略程序结构

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
SHFE.cu1901  -  上期所 cu1901 期货合约
DCE.m1901    -  大商所 m1901 期货合约
CZCE.SR901   -  郑商所 SR901 期货合约
CFFEX.IF1901 -  中金所 IF1901 期货合约
INE.sc2109   -  上期能源 sc2109 期货合约
GFEX.si2301  -  广期所 si2301 期货合约

CZCE.SPD SR901&SR903  - 郑商所 SR901&SR903 跨期合约
DCE.SP a1709&a1801    - 大商所 a1709&a1801 跨期合约
GFEX.SP si2308&si2309 - 广期所 si2308&si2309 跨期组合

DCE.m1807-C-2450    - 大商所豆粕期权
CZCE.CF003C11000    - 郑商所棉花期权
SHFE.au2004C308     - 上期所黄金期权
CFFEX.IO2002-C-3550 - 中金所沪深300股指期权
INE.sc2109C450      - 上期能源原油期权
GFEX.si2308-C-5800  - 广期所硅期权


KQ.m@CFFEX.IF - 中金所IF品种主连合约
KQ.i@SHFE.bu - 上期所bu品种指数

KQD.m@CBOT.ZS - 美黄豆主连

SSWE.CUH - 上期所仓单铜现货数据

SSE.600000 - 上交所浦发银行股票编码
SZSE.000001 - 深交所平安银行股票编码
SSE.000016 - 上证50指数
SSE.000300 - 沪深300指数
SSE.000905 - 中证500指数
SSE.000852 - 中证1000指数
SSE.588000 - 上交所华夏科创50
SSE.588080 - 上交所易方达科创50
SZSE.159915 - 深交所创业板
SSE.510050 - 上交所上证50ETF
SSE.510300 - 上交所沪深300ETF
SZSE.159919 - 深交所沪深300ETF
SSE.10002513 - 上交所上证50ETF期权
SSE.10002504 - 上交所沪深300ETF期权
SZSE.90000097 - 深交所沪深300ETF期权
SZSE.159915 - 易方达创业板ETF
SZSE.90001277 - 创业板ETF期权
SZSE.159922 - 深交所中证500ETF
SZSE.90001355 - 深交所中证500ETF期权
SSE.510500 - 上交所中证500ETF
SSE.10004497 - 上交所中证500ETF期权
SZSE.159901 - 深交所100ETF
```

Example 2 (unknown):
```unknown
q = api.get_quote("SHFE.cu2201")
```

Example 3 (json):
```json
{
    "datetime": "2021-08-17 14:59:59.000001",  # 行情从交易所发出的时间(北京时间)
    "ask_price1": 69750.0,  # 卖一价
    "ask_volume1": 1,  # 卖一量
    "bid_price1": 69600.0,  # 买一价
    "bid_volume1": 2,  # 买一量
    "ask_price2": 69920.0,  # 卖二价
    "ask_volume2": 3,  # 卖二量
    "bid_price2": 69500.0,  # 买二价
    "bid_volume2": 3,  # 买二量
    "ask_price3": 69940.0,  # 卖三价
    "ask_volume3": 1,  # 卖三量
    "bid_price3": 69450.0,  # 买三价
    "bid_volume3": 1,  # 买三量
    "ask_price4": 70010.0,  # 卖四价
    "ask_volume4": 1,  # 卖四量
    "bid_price4": 69400.0,  # 买四价
    "bid_volume4": 1,  # 买四量
    "ask_price5": 70050.0,  # 卖五价
    "ask_volume5": 1,  # 卖五量
    "bid_price5": 69380.0,  # 买五价
    "bid_volume5": 1,  # 买五量
    "last_price": 69710.0,  # 最新价
    "highest": 70050.0,  # 当日最高价
    "lowest": 69520.0,  # 当日最低价
    "open": 69770.0,  # 开盘价
    "close": 69710.0,  # 收盘价
    "average": 69785.019711,  # 当日均价
    "volume": 761,  # 成交量
    "amount": 265532000.0,  # 成交额
    "open_interest": 8850,  # 持仓量
    "settlement": 69780.0,  # 结算价
    "upper_limit": 75880.0,  # 涨停价
    "lower_limit": 64630.0,  # 跌停价
    "pre_open_interest": 8791,  # 昨持仓量
    "pre_settlement": 70260.0,  # 昨结算价
    "pre_close": 69680.0,  # 昨收盘价
    "price_tick": 10.0,  # 合约价格变动单位
    "price_decs": 0,  # 合约价格小数位数
    "volume_multiple": 5.0,  # 合约乘数
    "max_limit_order_volume": 500,  # 最大限价单手数
    "max_market_order_volume": 0,  # 最大市价单手数
    "min_limit_order_volume": 0,  # 最小限价单手数
    "min_market_order_volume": 0,  # 最小市价单手数
    "underlying_symbol": "",  # 标的合约
    "strike_price": NaN,  # 行权价
    "ins_class": "FUTURE",  # 合约类型
    "instrument_id": "SHFE.cu2201",  # 合约代码
    "instrument_name": "沪铜2201",  # 合约中文名
    "exchange_id": "SHFE",  # 交易所代码
    "expired": false,  # 合约是否已下市
    "trading_time": "{'day': [['09:00:00', '10:15:00'], ['10:30:00', '11:30:00'], ['13:30:00', '15:00:00']], 'night': [['21:00:00', '25:00:00']]}",  # 交易时间段
    "expire_datetime": 1642402800.0,  # 到期具体日，以秒为单位的 timestamp 值
    "delivery_year": 2022,  # 期货交割日年份，只对期货品种有效。期权推荐使用最后行权日年份
    "delivery_month": 1,  # 期货交割日月份，只对期货品种有效。期权推荐使用最后行权日月份
    "last_exercise_datetime": NaN,  # 期权最后行权日，以秒为单位的 timestamp 值
    "exercise_year": 0,  # 期权最后行权日年份，只对期权品种有效。
    "exercise_month": 0,  # 期权最后行权日月份，只对期权品种有效。
    "option_class": "",  # 期权行权方式，看涨:'CALL'，看跌:'PUT'
    "exercise_type": "",  # 期权行权方式，美式:'A'，欧式:'E'
    "product_id": "cu",  # 品种代码
    "iopv": NaN,  # ETF实时单位基金净值
    "public_float_share_quantity": 0,  # 日流通股数，只对证券产品有效。
    "stock_dividend_ratio": [],  # 除权表 ["20190601,0.15","20200107,0.2"…]
    "cash_dividend_ratio": [],  # 除息表 ["20190601,0.15","20200107,0.2"…]
    "expire_rest_days": 153,   # 距离到期日的剩余天数（自然日天数），正数表示距离到期日的剩余天数，0表示到期日当天，负数表示距离到期日已经过去的天数
    "commission": 17.565,
    "margin": 31617.0
}
```

Example 4 (python):
```python
q = api.get_quote("SHFE.cu1812")  # 获取SHFE.cu1812合约的行情

while api.wait_update():
  print(q.last_price)    # 收到新行情时都会执行这行
```

---


## TqSdk整体结构

**URL:** https://doc.shinnytech.com/tqsdk/latest/dev_framework.html

**Contents:**
- TqSdk整体结构
- 文件结构
- 数据流
- 内存数据存储与更新
- 异步任务调度

TqSdk中以数据流的方式连接各组件。TqChan(本质是一个asyncio.Queue)被用作两个组件间的单向数据流管道，一个组件向 TqChan 中放入数据包，另一个组件从 TqChan 中依次取出数据包。

用户程序调用 TqApi 中的某些需要发出数据包的功能函数, 以 TqApi.insert_order 为例

TqApi.insert_order 函数生成一个需要发出的数据包, 将此数据包放入 api_send_chan

TqAccount 从 api_send_chan 中取出此数据包，根据 aid 字段，决定将此数据包放入 td_send_chan

连接到交易网关的 websocket client 从 td_send_chan 中取出此数据包，通过网络发出

连接到行情网关的 websocket client 从网络收到一个数据包，将其放入 md_recv_chan

TqAccount 从md_recv_chan中取出此数据包，将它放入 api_recv_chan

TqApi 从api_recv_chan中取出此数据包，将数据包中携带的行情数据合并到内存存储区中

基于这样的数据流结构，可以通过简单更换部分组件的方式实现不同工作模式。例如模拟交易时，我们用 TqSim 替换 TqAccount:

按照 DIFF 协议推荐的客户端最佳实践，TqApi 使用单一变量(TqApi._data)存储所有业务数据, 它的结构如下:

在每次收到数据包时，TqApi都会将数据包内容合并到 TqApi._data 中. 具体的代码流程如下:

websocket client 收到数据包, 放入 TqApi._pending_diffs

wait_update 函数发现 TqApi._pending_diffs 有待处理数据包, 中止异步循环以处理此数据包:

wait_update 调用 self._merge_diff 函数:

TqApi._merge_diff 函数将收到的数据包并入本地存储.

对于k线之类的序列数据, 后续继续将更新的数据复制到 pandas dataframe 中

TqApi 在 wait_update 函数中完成所有异步任务的调度执行. 每当用户程序执行 api.wait_update 函数时, 会调度所有 task 运行, 直到收到新数据包或超时 wait_update函数返回, 继续执行后续用户代码

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (swift):
```swift
while not self._wait_timeout and not self._pending_diffs:     # 这里发现 self._pending_diffs 非空, 中止 while 循环
    self._run_once()
```

Example 2 (swift):
```swift
for d in self._diffs:
    self._merge_diff(self._data, d, self._prototype, False)
```

---

