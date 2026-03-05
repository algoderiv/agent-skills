# Api Ref - Part 7

**Topics:** tqsdk.ta - 技术指标计算函数

---

## tqsdk.ta - 技术指标计算函数

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.ta.html

**Contents:**
- tqsdk.ta - 技术指标计算函数

MACD(df, short, long, m)

SAR(df, n, step, max)

DMA(df, short, long, m)

BBI(df, n1, n2, n3, n4)

DDI(df, n, n1, m, m1)

PRICEOSC(df, long, short)

价格震荡指数 Price Oscillator

SLOWKD(df, n, m1, m2, m3)

累积/派发指标 Accumulation/Distribution

VOSC(df, short, long)

移动平均成交量指标 Volume Oscillator

BS_VALUE(df, quote[, r, v])

OPTION_GREEKS(df, quote[, r, v])

OPTION_VALUE(df, quote)

OPTION_IMPV(df, quote[, r])

VOLATILITY_CURVE(df, quotes, underlying[, r])

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 分别是"tr"和"atr", 分别代表真实波幅和平均真实波幅

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"bias", 代表计算出来的乖离率值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的dataframe包含3列, 分别是"mid", "top"和"bottom", 分别代表布林线的中、上、下轨

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含5列, 是"atr", "pdi", "mdi", "adx"和"adxr", 分别代表平均真实波幅, 上升方向线, 下降方向线, 趋向平均值以及评估数值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含3列, 是"k", "d"和"j", 分别代表计算出来的K值, D值和J值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含3列, 是"diff", "dea"和"bar", 分别代表离差值, DIFF的指数加权移动平均线, MACD的柱状线

(注: 因 DataFrame 有diff()函数，因此获取到此指标后："diff"字段使用 macd["diff"] 方式来取值，而非 macd.diff )

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"sar", 代表计算出来的SAR值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"wr", 代表计算出来的威廉指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"rsi", 代表计算出来的相对强弱指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"asi", 代表计算出来的振动升降指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"vr", 代表计算出来的VR

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"ar"和"br" , 分别代表人气指标和意愿指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"ddd"和"ama", 分别代表长短周期均值的差和ddd的简单移动平均值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"ma1"和"ma2", 分别代表指数加权移动平均线1和指数加权移动平均线2

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"cr"和"crma", 分别代表CR值和CR值的简单移动平均值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"cci", 代表计算出来的CCI值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"obv", 代表计算出来的OBV值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含4列, 是"ah", "al", "nh", "nl", 分别代表最高值, 最低值, 近高值, 近低值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含3列, 是"mah", "mal", "mac", 分别代表最高价的移动平均线, 最低价的移动平均线以及收盘价的移动平均线

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"upper", "lower", 分别代表上线和下线

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含6列, 是"wr", "mr", "sr", "ws", "ms", "ss", 分别代表初级压力价,中级压力,强力压力,初级支撑,中级支撑和强力支撑

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"pb", 代表计算出的瀑布线

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"bbi", 代表计算出的多空指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"b", "d", 分别代表计算出来的DKX指标及DKX的m日简单移动平均值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含3列, 是"bbiboll", "upr", "dwn", 分别代表多空布林线, 压力线和支撑线

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"adtm", "adtmma", 分别代表计算出来的ADTM指标及其M日的简单移动平均

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"b36", "b612", 分别代表收盘价的3日移动平均线与6日移动平均线的乖离值及收盘价的6日移动平均线与12日移动平均线的乖离值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"dbcd", "mm", 分别代表离差值及其简单移动平均值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含3列, 是"ddi", "addi", "ad", 分别代表DIZ与DIF的差值, DDI的加权平均, ADDI的简单移动平均

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"k", "d", 分别代表计算出来的K值与D值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"lwr", 代表计算出来的威廉指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"mass", 代表计算出来的梅斯线指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"mfi", 代表计算出来的MFI指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"a", "mi", 分别代表当日收盘价与N日前收盘价的差值以及MI值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"dif", "micd", 代表离差值和MICD指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"mtm", "mtmma", 分别代表MTM值和MTM的简单移动平均值

价格震荡指数 Price Oscillator

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"priceosc", 代表计算出来的价格震荡指数

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"psy", "psyma", 分别代表心理线和心理线的简单移动平均

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"qhl5", "qhl10", 分别代表计算出来的QHL5值和QHL10值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"arc", 代表计算出来的变化率指数

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"dif", "rccd", 分别代表离差值和异同离差变化率指数

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"roc", "rocma", 分别代表ROC值和ROC的简单移动平均值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"k", "d", 分别代表K值和D值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"srdm", "asrdm", 分别代表计算出来的SRDM值和SRDM值的加权移动平均值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"a", "mi", 分别代表A值和MI值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"b", "d", 分别代表A值的n2周期简单移动平均和A值的n3周期简单移动平均

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"dpo", 代表计算出来的DPO指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"lon", "ma1", 分别代表长线指标和长线指标的10周期简单移动平均值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"short", "ma1", 分别代表短线指标和短线指标的10周期简单移动平均值

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"mv1", "mv2", 分别代表均量线1和均量线2

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含3列, 是"a", "b", "e", 分别代表A/D值,A/D值n周期的以1为权重的移动平均, A/D值m周期的以1为权重的移动平均

累积/派发指标 Accumulation/Distribution

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"ad", 代表计算出来的累积/派发指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"ccl", 代表计算出来的持仓异动指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含2列, 是"vol", "opid", 分别代表成交量和持仓量

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"opi", 代表持仓量

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"pvt", 代表计算出来的价量趋势指数

移动平均成交量指标 Volume Oscillator

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"vosc", 代表计算出来的移动平均成交量指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"vroc", 代表计算出来的量变动速率

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"vrsi", 代表计算出来的量相对强弱指标

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"wvad", 代表计算出来的威廉变异离散量

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"ma", 代表计算出来的简单移动平均线

df (pandas.DataFrame): Dataframe格式的K线序列

n (int): 扩展指数加权移动平均的周期

m (int): 扩展指数加权移动平均的权重

pandas.DataFrame: 返回的DataFrame包含1列, 是"sma", 代表计算出来的扩展指数加权移动平均线

df (pandas.DataFrame): Dataframe格式的K线序列

n (int): 指数加权移动平均线的周期

pandas.DataFrame: 返回的DataFrame包含1列, 是"ema", 代表计算出来的指数加权移动平均线

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"ema2", 代表计算出来的线性加权移动平均线

df (pandas.DataFrame): Dataframe格式的K线序列

pandas.DataFrame: 返回的DataFrame包含1列, 是"trma", 代表计算出来的三角移动平均线

df (pandas.DataFrame): 需要计算理论价的期权对应标的合约的 K 线序列，Dataframe 格式

quote (tqsdk.objs.Quote): 需要计算理论价的期权对象，其标的合约应该是 df 序列对应的合约，否则返回序列值全为 nan

v (None | float | pandas.Series): [可选]波动率

None [默认]: 使用 df 中的 close 序列计算的波动率来计算期权理论价格

float: 对于 df 中每一行都使用相同的 v 计算期权理论价格

pandas.Series: 其行数应该和 df 行数相同，对于 df 中每一行都使用 v 中对应行的值计算期权理论价格

pandas.DataFrame: 返回的 DataFrame 包含 1 列, 是 "bs_price", 代表计算出来的期权理论价格, 与参数 df 行数相同

df (pandas.DataFrame): 期权合约及对应标的合约组成的多 K 线序列, Dataframe 格式

对于参数 df，需要用 api.get_kline_serial() 获取多 K 线序列，第一个参数为 list 类型，顺序为期权合约在前，对应标的合约在后，否则返回序列值全为 nan。

例如：api.get_kline_serial(["SHFE.cu2006C44000", "SHFE.cu2006"], 60, 100)

quote (tqsdk.objs.Quote): 期权对象，应该是 df 中多 K 线序列中的期权合约对象，否则返回序列值全为 nan。

例如：api.get_quote("SHFE.cu2006C44000")

v (None | float | pandas.Series): [可选]波动率

None [默认]: 使用 df 序列计算出的隐含波动率计算希腊指标值

float: 对于 df 中每一行都使用相同的 v 计算希腊指标值

pandas.Series: 其行数应该和 df 行数相同，对于 df 中每一行都使用 v 中对应行的值计算希腊指标值

pandas.DataFrame: 返回的 DataFrame 包含 5 列, 分别是 "delta", "theta", "gamma", "vega", "rho", 与参数 df 行数相同

df (pandas.DataFrame): 期权合约及对应标的合约组成的多 K 线序列, Dataframe 格式

对于参数 df，需要用 api.get_kline_serial() 获取多 K 线序列，第一个参数为 list 类型，顺序为期权合约在前，对应标的合约在后，否则返回序列值全为 nan。

例如：api.get_kline_serial(["SHFE.cu2006C44000", "SHFE.cu2006"], 60, 100)

quote (tqsdk.objs.Quote): 期权对象，应该是 df 中多 K 线序列中的期权合约对象，否则返回序列值全为 nan。

例如：api.get_quote("SHFE.cu2006C44000")

pandas.DataFrame: 返回的 DataFrame 包含 2 列, 是 "intrins" 和 "time", 代表内在价值和时间价值, 与参数 df 行数相同

df (pandas.DataFrame): 期权合约及对应标的合约组成的多 K 线序列, Dataframe 格式

对于参数 df，需要用 api.get_kline_serial() 获取多 K 线序列，第一个参数为 list 类型，顺序为期权合约在前，对应标的合约在后，否则返回序列值全为 nan。

例如：api.get_kline_serial(["SHFE.cu2006C44000", "SHFE.cu2006"], 60, 100)

quote (tqsdk.objs.Quote): 期权对象，应该是 df 中多 K 线序列中的期权合约对象，否则返回序列值全为 nan。

例如：api.get_quote("SHFE.cu2006C44000")

pandas.DataFrame: 返回的 DataFrame 包含 1 列, 是 "impv", 与参数 df 行数相同

df (pandas.DataFrame): 期权合约及基础标合约组成的多 K 线序列, Dataframe 格式

quote (dict): 批量获取合约的行情信息, 存储结构必须为 dict, key 为合约, value 为行情数据

例如: {'SHFE.cu2101':{ ... }, ‘SHFE.cu2101C34000’:{ ... }}

underlying (str): 基础标的的合约名称, 如 SHFE.cu2101

pandas.DataFrame: 返回的 DataFrame

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
# 获取 CFFEX.IF1903 合约的平均真实波幅
from tqsdk import TqApi, TqAuth, TqSim
from tqsdk.ta import ATR

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial("CFFEX.IF1903", 24 * 60 * 60)
atr = ATR(klines, 14)
print(atr.tr)  # 真实波幅
print(atr.atr)  # 平均真实波幅

