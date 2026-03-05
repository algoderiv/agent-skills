bool: 返回 True 表示已经从服务器收到了所有订阅的数据

一个task就是一个协程，task的调度是在 wait_update 函数中完成的，如果代码从来没有调用 wait_update，则task也得不到执行

coro (coroutine): 需要创建的协程

注册一个channel以便接受业务数据更新通知

调用此函数将返回一个channel, 当obj更新时会通知该channel

推荐使用 async with api.register_update_notify() as update_chan 来注册更新通知

如果直接调用 update_chan = api.register_update_notify() 则使用完成后需要调用 await update_chan.close() 避免资源泄漏

obj (any/list of any): [可选]任意业务对象, 包括 get_quote 返回的 quote, get_kline_serial 返回的 k_serial, get_account 返回的 account 等。默认不指定，监控所有业务对象

chan (TqChan): [可选]指定需要注册的channel。默认不指定，由本函数创建

发送基于 GraphQL 的合约服务请求查询，在同步代码中返回查询结果；异步代码中返回查询结果的引用地址。

query (str): [必填] 查询语句

variables (dict): [必填] 查询语句对应的参数取值

query_id (str): [可选] 查询请求 id

其的结构为 {query: "", variables: {}, result: {}} query 和 variables 为发送请求时传入的参数，result 为查询结果

该函数返回的对象不会更新，不建议在循环内调用该方法。

symbol (str / list of str): [必填] 合约代码

days (int): [可选] 交易日数，默认为 1

start_dt (date): [可选] 开始日期，默认为 None

pandas.DataFrame: 本函数返回 pandas.DataFrame 实例。行数为 days * 合约数，每行是一条合约结算价信息。包含以下列:

注意: 1. 每日结算价产生时间: 每天的 18:30 ~ 19:00：

如果想查询交易日 20241011 的结算价，需要在 20241011 的 19:00 之后查询，否则无法获取 20241011 的数据

此处的 days 指的是交易日数，会自动跳过节假日、周末等非交易日

在 19:00 前查询，返回 20240925 20240926 20240927 20240930 20241008 20241009 20241010 的结算价信息

在 19:00 后查询，返回 20240926 20240927 20240930 20241008 20241009 20241010 20241011 的结算价信息

假设 days=3，返回 20241008 20241009 20241010 三天的结算价信息

假设 days=4，如果在 19:00 之前查询，返回 20241008 20241009 20241010 三天的结算价信息

假设 days=4，如果在 19:00 之后查询，返回 20241008 20241009 20241010 20241011 四天的结算价信息

中金所、上期所、上能所、大商所、郑商所：20200102 起

中金所、上期所、大商所、郑商所：20200102 起

这是一个实验性接口（unstable）：EDB 后端的 id/表结构/数据口径可能变更， 本接口的参数与返回结构未来可能调整，届时可能不兼容旧代码。

EDB 指标数据网站入口：https://edb.shinnytech.com

指标 id 列表、指标含义、以及数据可视化展示，可在上述网站中在线查询与查看

ids (list[int]): [必填] 指标 id 列表，长度 1~100（会去重并保持请求顺序） n (int): [可选] 时间窗口长度（单位：自然日，闭区间），默认 1。

end 使用当前 api 时间（回测模式下为回测推进的当前时间对应日期）

start = end - (n - 1) 天

None [默认]：仅返回实际有值的日期（稀疏），不补齐缺失日期

"day"：在 [start, end] 内按自然日补齐日期

None [默认]：不填充，缺失为 NaN

pandas.DataFrame: 本函数返回 pandas.DataFrame 实例。返回值不会再更新。包含以下结构说明:

align=None 时：仅包含实际有值的日期（稀疏），不补齐缺失日期

align="day" 时：包含 [start, end] 内的所有自然日（补齐缺失日期）

columns: 指标 id（会对 ids 去重并保持请求顺序；最终以服务端返回为准）

align="day" 且 fill="ffill"/"bfill" 时，会对缺失值进行前向/后向填充

该函数返回的对象不会更新，不建议在循环内调用该方法。

symbol (str): [必填] 合约代码

ranking_type (str)：[必填] 表示返回结果以哪一项为排名基准，VOLUME 成交量排名，LONG 多头持仓排名, SHORT 空头持仓排名

days (int): [可选] 返回结果中包含的天数，默认为 1

如果开始日期为 date 类型，则返回从开始日期之后 days 个交易日的有效数据

如果开始日期为 None，则返回最近 days 个交易日的持仓排名情况

broker (str): [可选] 指定期货公司，以各家交易所列出的期货公司名称为准来进行查询，各家交易所可能期货公司名称不一致，如果对应这一天这家交易所没数据则返回对应数值为nan

pandas.DataFrame: 本函数返回 pandas.DataFrame 实例。行数为 days * 20，每行为一条成交量/多头持仓量/空头持仓量的排名信息。返回值不会再更新。包含以下列:

symbol (合约代码，以交易所列出的期货公司名称为准)

instrument_id (交易所内合约代码)

volume_change (成交量变化)

volume_ranking (成交量排名)

long_change (多头持仓增减量)

long_ranking (多头持仓量排名)

short_change (空头持仓增减量)

short_ranking (空头持仓量排名)

注意: 1. 返回值中 datetime、symbol、exchange_id、instrument_id、broker 这几列一定为有效值。其他列会根据不同的 ranking_type 参数值，可能返回 nan：

如果该期货公司的 long_ranking 在前 20 名内，long_oi、long_change、long_ranking 这三列为有效值，否则为 nan。 如果该期货公司的 short_ranking 在前 20 名内，short_oi、short_change、short_ranking 这三列为有效值，否则为 nan。

数据更新时间: 18:30~19:00。 用户在交易日 19:00 之前可以查询当前交易日之前的所有数据，19:00 之后可以查询包括当前交易日的数据。

数据支持范围：从 20200720 开始的期货数据。

根据相应的参数发送合约服务请求查询，并返回查询结果

product_id (str / list of str): [可选] 品种（股票、期权不能通过 product_id 筛选查询）

expired (bool): [可选] 是否已下市

None 表示筛选结果既包括有夜盘品种也包括无夜盘品种

list: 符合筛选条件的合约代码的列表，例如: ["SHFE.cu2012", "SHFE.au2012", "SHFE.wr2012"]

根据填写的参数筛选，返回主力连续合约对应的标的合约列表

product_id (str): [可选] 品种

None 表示筛选结果既包括有夜盘品种也包括无夜盘品种

list: 符合筛选条件的合约代码的列表，例如: ["SHFE.cu2012", "SHFE.au2012", "SHFE.wr2012"]

发送合约服务请求查询，查询符合条件的期权列表，并返回查询结果

underlying_symbol (str): 标的合约

exercise_year (int): [可选] 最后行权日年份

exercise_month (int): [可选] 最后行权日月份

strike_price (float): [可选] 行权价格

expired (bool): [可选] 是否下市

has_A (bool): [可选] 是否含有A，输入True代表只含A的期权，输入False代表不含A的期权，默认为None不做区分

list: 符合筛选条件的合约代码的列表，例如: ["SHFE.cu2012C24000", "SHFE.cu2012P24000"]

underlying_symbol (str): [必填] 标的合约 （目前每个标的只对应一个交易所的期权）

underlying_price (float): [必填] 标的价格，该价格用户输入可以是任意值，例如合约最新价，最高价，开盘价等然后以该值去对比实值/虚值/平值期权

如果没有用户指定档位的期权（比如 -100 档），则返回的列表该位置上为 None。例如当 price_level = [-100, 0]，则会返回 [None, "平值期权合约代码"]

exercise_year (str): [ETF 期权、股指期权必填] 期权最后行权日年份

exercise_month (str): [ETF 期权、股指期权必填] 期权最后行权日月份

has_A (bool): [可选] 是否含有 A，输入True代表只含A的期权，输入False代表不含A的期权，默认为None不做区分

注：当选择平值期权时，会按以下逻辑进行选择：

根据用户传入参数来生成一个期权列表，在这个期权列表中来选择是否有和传入价格相比的平值期权并返回

如果没有符合的平值期权，则取行权价距离传入价格绝对值相差最小的期权作为平值期权

如果存在最近的两个期权的行权价到传入价格的绝对值最小且相等，则取虚值的那个期权作为平值期权，其他档位期权依次展开

pandas.DataFrame: 本函数返回 pandas.DataFrame 实例。每行为一个合约的合约信息。返回值不会再更新。包含以下列:

instrument_id: 合约代码，参考 合约, 行情和历史数据

instrument_name: 合约中文名

exchange_id: 交易所代码，参考 合约, 行情和历史数据

volume_multiple: 合约乘数

max_limit_order_volume: 最大限价单手数

max_market_order_volume: 最大市价单手数

underlying_symbol: 标的合约 （CONT OPTION 类型的合约此字段有效）

expire_datetime: 到期具体日，以秒为单位的 timestamp 值

expire_rest_days: 距离到期日的剩余天数（自然日天数）

delivery_year: 期货交割日年份，只对期货品种有效。期权推荐使用最后行权日年份

delivery_month: 期货交割日月份，只对期货品种有效。期权推荐使用最后行权日月份

last_exercise_datetime: 期权最后行权日，以秒为单位的 timestamp 值

exercise_year: 期权最后行权日年份，只对期权品种有效。

exercise_month: 期权最后行权日月份，只对期权品种有效。

pre_open_interest: 昨持仓

trading_time_day: 白盘交易时间段，pandas.Series 类型

trading_time_night: 夜盘交易时间段，pandas.Series 类型

回测时，以下字段值为 nan: "upper_limit", "lower_limit", "pre_settlement", "pre_open_interest", "pre_close"

发送合约服务请求查询，查询符合条件的期权列表，返回全部的实值、平值、虚值期权

underlying_symbol (str): [必填] 标的合约 （目前每个标的只对应一个交易所的期权）

underlying_price (float): [必填] 标的价格，该价格用户输入可以是任意值，例如合约最新价，最高价，开盘价等然后以该值去对比实值/虚值/平值期权

exercise_year (str): [ETF 期权、股指期权必填] 期权最后行权日年份

exercise_month (str): [ETF 期权、股指期权必填] 期权最后行权日月份

has_A (bool): [可选] 是否含有 A，输入True代表只含A的期权，输入False代表不含A的期权，默认为None不做区分

返回三个列表，分别为实值期权列表、平值期权列表、虚值期权列表。其中，平值期权列表只包含一个元素。

对于看涨期权，返回的实值期权列表、平值期权列表、虚值期权列表其期权行权价依此递增；

对于看跌期权，返回的实值期权列表、平值期权列表、虚值期权列表其期权行权价依此递减。

注：当选择平值期权时，会按以下逻辑进行选择：

根据用户传入参数来生成一个期权列表，在这个期权列表中来选择是否有和传入价格相比的平值期权并返回

如果没有符合的平值期权，则取行权价距离传入价格绝对值相差最小的期权作为平值期权

如果存在最近的两个期权的行权价到传入价格的绝对值最小且相等，则取虚值的那个期权作为平值期权，其他档位期权依次展开

发送合约服务请求查询，针对 ETF 期权和股指期权，只查询未下市合约，可以按照距离最后行权日的距离的远近，查询符合条件的期权列表，返回全部的实值、平值、虚值期权

"SSE.000300" 为中金所沪深 300 股指期权(IO)标的

"SSE.000852" 为中金所中证 1000 股指期权(MO)标的

"SSE.000016" 为中金所上证 50 股指期权(HO)标的

"SSE.510050" 为上交所华夏上证 50 ETF 期权标的

"SSE.510300" 为上交所华泰柏瑞沪深 300 ETF 期权标的

"SZSE.159919" 为深交所嘉实沪深 300 ETF 期权标的

"SZSE.159915" 为深交所易方达创业板 ETF 期权标的

"SZSE.159922" 为深交所嘉实中证 500 ETF 期权标的

"SSE.510500" 为上交所南方中证 500 ETF 期权标的

underlying_price (float): [必填] 标的价格，该价格用户输入可以是任意值，例如合约最新价，最高价，开盘价等然后以该值去对比实值/虚值/平值期权

对于 ETF 期权来说 1 代表在参数 0 后的下月，2 代表随后的第一个一个季月，3 代表随后的第二个季月

对于股指期权来说 1 代表在参数 0 后的下月，2 代表下下月，3 代表随后第一个季月，4 代表随后第二个季月，5 代表随后第三个季月

has_A (bool): [可选] 是否含有 A，输入 True 代表只含 A 的期权，输入 False 代表不含 A 的期权，默认为 None 不做区分

返回三个列表，分别为实值期权列表、平值期权列表、虚值期权列表。其中，平值期权列表只包含一个元素。

对于看涨期权，返回的实值期权列表、平值期权列表、虚值期权列表其期权行权价依此递增；

对于看跌期权，返回的实值期权列表、平值期权列表、虚值期权列表其期权行权价依此递减。

注：当选择平值期权时，会按以下逻辑进行选择：

根据用户传入参数来生成一个期权列表，按照到期时间和行权价排序（看涨期权升序排列，看跌期权降序排列，使得实值期权在前、虚值期权在后）。

将所有的行权价去重排序（看涨期权升序排列，看跌期权降序排列，实值期权在前、虚值期权在后）

找到与 underlying_price 差值最小的价格为平值期权行权价格，如果有两个行权价与 underlying_price 的差相等，则选取下标大价格的为平值期权行权价格（即两个期权与标的价格距离相等，选取虚值的那个期权作为平值期权）。

如果有多个行权月份的期权，选取距离行权日期最近的那个作为平值期权（保证平值期权列表只包含一个元素）。

下标比平值期权小的为实值期权，下标比平值期权大的为虚值期权。

float: 一个波动率值，symbol 为 str 类型时，可以只传入一个波动率值

list of float: 波动率序列，symbol 为 list 类型时，必须传入与 symbol 数量相同，顺序一一对应的波动率序列

r (float): [可选] 无风险利率

pandas.DataFrame: 行数与参数 symbol 的合约数量相同，包含以下列：

instrument_name: 合约中文名

expire_rest_days: 距离到期日的剩余天数

expire_datetime: 到期具体日，以秒为单位的 timestamp 值

underlying_symbol: 标的合约

delta: 期权希腊指标 detla 值

gamma: 期权希腊指标 gamma 值

theta: 期权希腊指标 theta 值

配合天勤使用时, 在天勤的行情图上绘制一个字符串

base_k_dataframe (pandas.DataFrame): 基础K线数据序列, 要绘制的K线将出现在这个K线图上. 需要画图的数据以附加列的形式存在

x (int): X 坐标, 以K线的序列号表示. 可选, 缺省为对齐最后一根K线,

y (float): Y 坐标. 可选, 缺省为最后一根K线收盘价

id (str): 字符串ID, 可选. 以相同ID多次调用本函数, 后一次调用将覆盖前一次调用的效果

board (str): 选择图板, 可选, 缺省为 "MAIN" 表示绘制在主图

str : 符合 CSS Color 命名规则的字符串, 例如: "red", "#FF0000", "#FF0000FF", "rgb(255, 0, 0)", "rgba(255, 0, 0, .5)"

int : 十六进制整数表示颜色, ARGB, 例如: 0xffff0000

配合天勤使用时, 在天勤的行情图上绘制一个直线/线段/射线

base_k_dataframe (pandas.DataFrame): 基础K线数据序列, 要绘制的K线将出现在这个K线图上. 需要画图的数据以附加列的形式存在

x1 (int): 第一个点的 X 坐标, 以K线的序列号表示

y1 (float): 第一个点的 Y 坐标

x2 (int): 第二个点的 X 坐标, 以K线的序列号表示

y2 (float): 第二个点的 Y 坐标

id (str): 字符串ID, 可选. 以相同ID多次调用本函数, 后一次调用将覆盖前一次调用的效果

board (str): 选择图板, 可选, 缺省为 "MAIN" 表示绘制在主图

line_type (str): 画线类型, 目前只支持 "SEG" 线段

str : 符合 CSS Color 命名规则的字符串, 例如: "red", "#FF0000", "#FF0000FF", "rgb(255, 0, 0)", "rgba(255, 0, 0, .5)"

int : 十六进制整数表示颜色, ARGB, 例如: 0xffff0000

width (int): 线宽度, 可选, 缺省为 1

配合天勤使用时, 在天勤的行情图上绘制一个矩形

base_k_dataframe (pandas.DataFrame): 基础K线数据序列, 要绘制的K线将出现在这个K线图上. 需要画图的数据以附加列的形式存在

x1 (int): 矩形左上角的 X 坐标, 以K线的序列号表示

y1 (float): 矩形左上角的 Y 坐标

x2 (int): 矩形右下角的 X 坐标, 以K线的序列号表示

y2 (float): 矩形右下角的 Y 坐标

id (str): ID, 可选. 以相同ID多次调用本函数, 后一次调用将覆盖前一次调用的效果

board (str): 选择图板, 可选, 缺省为 "MAIN" 表示绘制在主图

str : 符合 CSS Color 命名规则的字符串, 例如: "red", "#FF0000", "#FF0000FF", "rgb(255, 0, 0)", "rgba(255, 0, 0, .5)"

int : 十六进制整数表示颜色, ARGB, 例如: 0xffff0000

str : 符合 CSS Color 命名规则的字符串, 例如: "red", "#FF0000", "#FF0000FF", "rgb(255, 0, 0)", "rgba(255, 0, 0, .5)"

int : 十六进制整数表示颜色, ARGB, 例如: 0xffff0000

width (int): 边框宽度, 可选, 缺省为 1

配合 web_gui 使用时, 在天勤的回测报告中绘制成交统计的图表

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
# 使用实盘帐号直连行情和交易服务器
from tqsdk import TqApi, TqAuth, TqAccount
api = TqApi(TqAccount("H海通期货", "022631", "123456"), auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
# 使用simnow帐号连接行情和交易服务器
from tqsdk import TqApi, TqAuth, TqAccount
api = TqApi(TqAccount("simnow", "simnow用户名", "simnow密码"), auth=TqAuth("快期账户", "账户密码"))
```

Example 3 (python):
```python
# 使用快期模拟帐号连接行情服务器
from tqsdk import TqApi, TqAuth, TqKq
api = TqApi(TqKq(), auth=TqAuth("快期账户", "账户密码"))  # 根据填写的快期账户参数连接指定的快期模拟账户
```

