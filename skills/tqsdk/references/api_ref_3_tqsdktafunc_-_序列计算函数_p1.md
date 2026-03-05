# Api Ref - Part 3

**Topics:** tqsdk.tafunc - 序列计算函数

---


# Api Ref - Part 3

## tqsdk.tafunc - 序列计算函数

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.tafunc.html

**Contents:**
- tqsdk.tafunc - 序列计算函数

简单移动: 求series序列位移n个周期的结果

标准差: 求series序列每n个周期的标准差

简单移动平均线: 求series序列n周期的简单移动平均

扩展指数加权移动平均: 求series序列n周期的扩展指数加权移动平均

指数加权移动平均线: 求series序列n周期的指数加权移动平均

线性加权移动平均: 求series值的n周期线性加权移动平均 (也称WMA)

向上穿越: 表当a从下方向上穿过b, 成立返回1, 否则返回0

向下穿越: 表示当a从上方向下穿b，成立返回1, 否则返回0

三角移动平均: 求series的n周期三角移动平均值

调和平均值: 求series在n个周期内的调和平均值

min(series1, series2)

获取series1和series2中的最小值

max(series1, series2)

获取series1和series2中的最大值

中位数: 求series在n个周期内居于中间的数值

判断n个周期内, 是否有满足cond的条件, 若满足则值为1, 不满足为0

判断n个周期内, 是否一直满足cond条件, 若满足则值为1, 不满足为0

平均绝对偏差: 求series在n周期内的平均绝对偏差

time_to_ns_timestamp(input_time)

将传入的时间转换为int类型的纳秒级时间戳

time_to_s_timestamp(input_time)

time_to_str(input_time)

将传入的时间转换为 %Y-%m-%d %H:%M:%S.%f 格式的 str 类型

time_to_datetime(input_time)

将传入的时间转换为 datetime.datetime 类型

返回一个序列，其中每个值表示从上一次条件成立到当前的周期数

get_t(df, expire_datetime)

计算 K 线序列对应的年化到期时间，主要用于计算期权相关希腊指标时，需要得到计算出序列对应的年化到期时间

get_his_volatility(df, quote)

get_bs_price(series, k, r, v, t, option_class)

get_delta(series, k, r, v, t, option_class)

get_gamma(series, k, r, v, t[, d1])

get_theta(series, k, r, v, t, option_class)

get_vega(series, k, r, v, t[, d1])

get_rho(series, k, r, v, t, option_class[, d1])

get_impv(series, series_option, k, r, ...)

get_dividend_df(stock_dividend_ratio, ...)

get_dividend_factor(dividend_df, last_item, item)

get_sharp(series[, trading_days_of_year, r])

get_sortino(series[, trading_days_of_year, r])

get_calmar(series, max_drawdown[, ...])

简单移动: 求series序列位移n个周期的结果

注意: 当n为0, 函数返回原序列; 当n为有效值但当前的series序列元素个数不足 n + 1 个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

pandas.Series: 位移后的序列

标准差: 求series序列每n个周期的标准差

注意: n为0的情况下, 或当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

简单移动平均线: 求series序列n周期的简单移动平均

计算公式: ma(x, 5) = (x(1) + x(2) + x(3) + x(4) + x(5)) / 5

注意: 1. 简单移动平均线将设定周期内的值取平均值, 其中各元素的权重都相等 2. n为0的情况下, 或当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

pandas.Series: 简单移动平均值序列

扩展指数加权移动平均: 求series序列n周期的扩展指数加权移动平均

计算公式: sma(x, n, m) = sma(x, n, m).shift(1) * (n - m) / n + x(n) * m / n

series (pandas.Series): 数据序列

pandas.Series: 扩展指数加权移动平均序列

指数加权移动平均线: 求series序列n周期的指数加权移动平均

ema(x, n) = 2 * x / (n + 1) + (n - 1) * ema(x, n).shift(1) / (n + 1)

series (pandas.Series): 数据序列

pandas.Series: 指数加权移动平均线序列

线性加权移动平均: 求series值的n周期线性加权移动平均 (也称WMA)

ema2(x, n) = [n * x(0) + (n - 1) * x(1) + (x - 2) * x(2) + ... + 1 * x(n - 1)] / [n + (n - 1) + (n - 2) + ... + 1]

注意: 当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

pandas.Series: 线性加权移动平均线序列

向上穿越: 表当a从下方向上穿过b, 成立返回1, 否则返回0

a (pandas.Series): 数据序列1

b (pandas.Series): 数据序列2

pandas.Series: 上穿标志序列

向下穿越: 表示当a从上方向下穿b，成立返回1, 否则返回0

a (pandas.Series): 数据序列1

b (pandas.Series): 数据序列2

pandas.Series: 下穿标志序列

注意: 如果n为0, 则从第一个有效值开始统计

cond (array_like): 条件

三角移动平均: 求series的n周期三角移动平均值

三角移动平均线公式, 是采用算数移动平均, 并且对第一个移动平均线再一次应用算数移动平均

注意: n为0的情况下, 或当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

pandas.Series: 三角移动平均值序列

调和平均值: 求series在n个周期内的调和平均值

harmean(x, 5) = 1 / [(1 / x(1) + 1 / x(2) + 1 / x(3) + 1 / x(4) + 1 / x(5)) / 5]

注意: 1. 调和平均值与倒数的简单平均值互为倒数 2. 当n为0, 或当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

pandas.Series: 调和平均值序列

numpow(x, n, m) = n ^ m * x + (n - 1) ^ m * x.shift(1) + (n - 2) ^ m * x.shift(2) + ... + 2 ^ m * x.shift(n - 2) + 1 ^ m * x.shift(n - 1)

注意: 当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

注意: 正数的绝对值是它本身, 负数的绝对值是它的相反数, 0的绝对值还是0

series (pandas.Series): 数据序列

获取series1和series2中的最小值

series1 (pandas.Series): 数据序列1

series2 (pandas.Series): 数据序列2

获取series1和series2中的最大值

series1 (pandas.Series): 数据序列1

series2 (pandas.Series): 数据序列2

中位数: 求series在n个周期内居于中间的数值

当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

对n个周期内所有series排序后, 若n为奇数, 则选择第(n + 1) / 2个为中位数, 若n为偶数, 则中位数是(n / 2)以及(n / 2 + 1)的平均数

series (pandas.Series): 数据序列

判断n个周期内, 是否有满足cond的条件, 若满足则值为1, 不满足为0

cond (array_like): 条件

pandas.Series: 判断结果序列

判断n个周期内, 是否一直满足cond条件, 若满足则值为1, 不满足为0

cond (array_like): 条件

pandas.Series: 判断结果序列

注意: n为0的情况下, 或当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

注意: n为0的情况下, 或当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

平均绝对偏差: 求series在n周期内的平均绝对偏差

计算avedev(df["close"],3)在最近一根K线上的值: (abs(df["close"] - (df["close"] + df["close"].shift(1) + df["close"].shift(2)) / 3) + abs( df["close"].shift(1) - (df["close"] + df["close"].shift(1) + df["close"].shift(2)) / 3) + abs( df["close"].shift(2) - (df["close"] + df["close"].shift(1) + df["close"].shift(2)) / 3)) / 3

注意: n为0的情况下, 或当n为有效值但当前的series序列元素个数不足n个, 函数返回 NaN 序列

series (pandas.Series): 数据序列

pandas.Series: 平均绝对偏差序列

将传入的时间转换为int类型的纳秒级时间戳

str: str 类型的时间，如Quote行情时间的datetime字段 (eg. 2019-10-14 14:26:01.000000)

int: int 类型的纳秒级或秒级时间戳

float: float 类型的纳秒级或秒级时间戳，如K线或tick的datetime字段 (eg. 1.57103449e+18)

datetime.datetime: datetime 模块中的 datetime 类型时间

str: str 类型的时间，如Quote行情时间的datetime字段 (eg. 2019-10-14 14:26:01.000000)

int: int 类型的纳秒级或秒级时间戳

float: float 类型的纳秒级或秒级时间戳，如K线或tick的datetime字段 (eg. 1.57103449e+18)

datetime.datetime: datetime 模块中的 datetime 类型时间

将传入的时间转换为 %Y-%m-%d %H:%M:%S.%f 格式的 str 类型

input_time (int/ float/ datetime.datetime): 需要转换的时间:

int: int 类型的纳秒级或秒级时间戳

float: float 类型的纳秒级或秒级时间戳，如K线或tick的datetime字段 (eg. 1.57103449e+18)

datetime.datetime: datetime 模块中的 datetime 类型时间

str : %Y-%m-%d %H:%M:%S.%f 格式的 str 类型时间

将传入的时间转换为 datetime.datetime 类型

input_time (int/ float/ str): 需要转换的时间:

int: int 类型的纳秒级或秒级时间戳

float: float 类型的纳秒级或秒级时间戳，如K线或tick的datetime字段 (eg. 1.57103449e+18)

str: str 类型的时间，如Quote行情时间的 datetime 字段 (eg. 2019-10-14 14:26:01.000000)

datetime.datetime : datetime 模块中的 datetime 类型时间

返回一个序列，其中每个值表示从上一次条件成立到当前的周期数

(注： 如果从cond序列第一个值到某个位置之间没有True，则此位置的返回值为 -1； 条件成立的位置上的返回值为0)

cond (pandas.Series): 条件序列(序列中的值需为 True 或 False)

pandas.Series : 周期数序列（其长度和 cond 相同；最后一个值即为最后一次条件成立到最新一个数据的周期数）

计算 K 线序列对应的年化到期时间，主要用于计算期权相关希腊指标时，需要得到计算出序列对应的年化到期时间

df (pandas.DataFrame): Dataframe 格式的 K 线序列

expire_datetime (int): 到期日, 秒级时间戳

pandas.Series : 返回的 df 对应的年化时间序列

df (pandas.DataFrame): Dataframe 格式的 K 线序列

quote (tqsdk.objs.Quote): df 序列对应合约对象

float : 返回的 df 对应的历史波动率

series (pandas.Series): 标的价格序列

v (float / pandas.Series): 波动率

float: 对于 series 中每个元素都使用相同的 v 计算理论价

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 v 中对应的值计算理论价

t (float / pandas.Series): 年化到期时间，例如：还有 100 天到期，则年化到期时间为 100/360

float: 对于 series 中每个元素都使用相同的 t 计算理论价

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 t 中对应的值计算理论价

option_class (str / pandas.Series): 期权方向，必须是两者其一，否则返回的序列值全部为 nan

pandas.Series: 其元素个数应该和 series 元素个数相同，每个元素的值为 "CALL" 或者 "PUT"

pandas.Series: 返回该序列理论价

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

pandas.Series: 该序列的 delta 值

series (pandas.Series): 标的价格序列

v (float / pandas.Series): 波动率

float: 对于 series 中每个元素都使用相同的 v 计算理论价

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 v 中对应的值计算理论价

t (float / pandas.Series): 年化到期时间，例如：还有 100 天到期，则年化到期时间为 100/360

float: 对于 series 中每个元素都使用相同的 t 计算理论价

pandas.Series: 其元素个数应该和 series 元素个数相同，对于 series 中每个元素都使用 t 中对应的值计算理论价

d1 (None | pandas.Series): [可选] 序列对应的 BS 公式中 b1 值

pandas.Series: 该序列的 gamma 值

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

pandas.Series: 该序列的 theta 值

series (pandas.Series): 标的价格序列

v (float / pandas.Series): 波动率

float: 对于 series 中每个元素都使用相同的 v 计算理论价

