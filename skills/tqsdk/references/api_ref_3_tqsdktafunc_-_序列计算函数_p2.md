pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 v 中对应的值计算理论价

t (float / pandas.Series): 年化到期时间，例如：还有 100 天到期，则年化到期时间为 100/360

float: 对于 series 中每个元素都使用相同的 t 计算理论价

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 t 中对应的值计算理论价

d1 (None | pandas.Series): [可选] 序列对应的 BS 公式中 b1 值

pandas.Series: 该序列的 vega 值

series (pandas.Series): 标的价格序列

v (float / pandas.Series): 波动率

float: 对于 series 中每个元素都使用相同的 v 计算理论价

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 v 中对应的值计算理论价

t (float / pandas.Series): 年化到期时间，例如：还有 100 天到期，则年化到期时间为 100/360

float: 对于 series 中每个元素都使用相同的 t 计算理论价

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 t 中对应的值计算理论价

option_class (str / pandas.Series): 期权方向，必须是两者其一，否则返回的序列值全部为 nan

pandas.Series: 其元素个数应该和 series 元素个数相同，每个元素的值为 "CALL" 或者 "PUT"

d1 (None | pandas.Series): [可选] 序列对应的 BS 公式中 b1 值

pandas.Series: 该序列的 rho 值

series (pandas.Series): 标的价格序列

series_option (pandas.Series): 期权价格序列，与 series 长度应该相同

init_v (float / pandas.Series): 初始波动率，迭代初始值

float: 对于 series 中每个元素都使用相同的 init_v 计算隐含波动率

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 init_v 中对应的值计算隐含波动率

t (float / pandas.Series): 年化到期时间，例如：还有 100 天到期，则年化到期时间为 100/360

float: 对于 series 中每个元素都使用相同的 t 计算理论价

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 t 中对应的值计算理论价

option_class (str / pandas.Series): 期权方向，必须是两者其一，否则返回的序列值全部为 nan

pandas.Series: 其元素个数应该和 series 元素个数相同，每个元素的值为 "CALL" 或者 "PUT"

pandas.Series: 该序列的隐含波动率

df (pandas.DataFrame): Dataframe 格式的 ticks 序列

pandas.Series: 返回序列的开平方向序列

stock_dividend_ratio (list): 除权表（可以由 quote.stock_dividend_ratio 取得）

cash_dividend_ratio (list): 除息表（可以由 quote.cash_dividend_ratio 取得）

pandas.Dataframe: 复权系数矩阵， Dataframe 对象有 ["datetime", "stock_dividend", "cash_dividend"] 三列。

dividend_df (pandas.Dataframe): 除权除息矩阵表

last_item (dict): 前一个 tickItem / klineItem

item (dict): 当前 tickItem / klineItem

series (pandas.Series): 每日收益率序列

trading_days_of_year (int): 年化交易日数量

series (pandas.Series): 每日收益率序列

trading_days_of_year (int): 年化交易日数量

series (pandas.Series): 每日收益率序列

max_drawdown (float): 最大回撤

trading_days_of_year (int): 年化交易日数量

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (swift):
```swift
from tqsdk import TqApi, TqAuth, TqSim, tafunc

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial("CFFEX.IF1908", 24 * 60 * 60)
pre_close = tafunc.ref(klines.close, 1)  # 将收盘价序列右移一位, 得到昨收盘序列
change = klines.close - pre_close        # 收盘价序列 - 昨收盘序列, 得到涨跌序列
print(list(change))
```

Example 2 (swift):
```swift
from tqsdk import TqApi, TqAuth, TqSim, tafunc

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial("CFFEX.IF1908", 24 * 60 * 60)
std = tafunc.std(klines.close, 5)  # 收盘价序列每5项计算一个标准差
print(list(std))
```

Example 3 (swift):
```swift
from tqsdk import TqApi, TqAuth, TqSim, tafunc

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial("CFFEX.IF1908", 24 * 60 * 60)
ma = tafunc.ma(klines.close, 5)
print(list(ma))
```

Example 4 (swift):
```swift
from tqsdk import TqApi, TqAuth, TqSim, tafunc

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial("CFFEX.IF1908", 24 * 60 * 60)
sma = tafunc.sma(klines.close, 5, 2)  # 收盘价序列每5项计算一个扩展指数加权移动平均值
print(list(sma))
```

---

