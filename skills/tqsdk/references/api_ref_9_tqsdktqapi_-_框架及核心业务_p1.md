# Api Ref - Part 9

**Topics:** tqsdk.TqApi - 框架及核心业务

---


# Api Ref - Part 9

## tqsdk.TqApi - 框架及核心业务

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.api.html

**Contents:**
- tqsdk.TqApi - 框架及核心业务

通常情况下, 一个线程中 应该只有一个 TqApi的实例, 它负责维护网络连接, 接收行情及账户数据, 并在内存中维护业务数据截面

None: 账号将根据环境变量决定, 默认为 TqSim

TqAccount : 使用实盘账号, 直连行情和交易服务器, 需提供期货公司/帐号/密码

TqKq : 使用快期账号登录，直连行情和快期模拟交易服务器

TqKqStock : 使用快期账号登录，直连行情和快期股票模拟交易服务器

TqSim : 使用 TqApi 自带的内部模拟账号

TqSimStock : 使用 TqApi 自带的内部股票模拟账号

TqTradingUnit : 使用交易单元账号

TqMultiAccount : 多账户列表，列表中支持 TqAccount、TqKq、TqKqStock、 TqSim、TqSimStock、TqZq、TqRohon、TqJees、 TqYida 、TqTradingUnit 和 TqCtp 中的 0 至 N 个或者组合

TqAuth : 添加快期账户类，例如：TqAuth("tianqin@qq.com", "123456")

str: 用户权限认证对象为天勤用户论坛的邮箱和密码，中间以英文逗号分隔，例如： "tianqin@qq.com,123456" 快期账户注册链接 https://www.shinnytech.com/register-intro/

当 account 为 TqAccount、TqKq、TqKqStock 类型时, 可以通过该参数指定交易服务器地址, 默认使用对应账户的交易服务地址，行情地址使用快期账户对应的行情服务地址

当 account 为 TqSim、TqSimStock 类型时, 可以通过该参数指定行情服务器地址, 默认使用快期账户对应的行情服务地址

TqBacktest : 传入 TqBacktest 对象，进入回测模式 在回测模式下, TqBacktest 连接 wss://backtest.shinnytech.com/t/md/front/mobile 接收行情数据, 由 TqBacktest 内部完成回测时间段内的行情推进和 K 线、Tick 更新.

None [默认]: 根据账户情况不同，默认值的行为不同。

仅有本地模拟账户 TqSim、TqSimStock 时，调试信息不输出。

当有其他类型账户时，即 TqAccount、TqKq、TqKqStock， 调试信息输出到指定文件夹 `~/.tqsdk/logs`（如果磁盘剩余空间不足 10G 则不会输出调试信息）。

True: 调试信息会输出到指定文件夹 ~/.tqsdk/logs。

str: 指定一个日志文件名, 调试信息输出到指定文件。

提示信息会输出到标准输出流，即 sys.stdout，标准输出流默认指向控制台。

loop(asyncio.AbstractEventLoop): [可选] 使用指定的 IOLoop, 默认创建一个新的.

启用图形化界面传入参数 web_gui=True 会每次以随机端口生成网页，也可以直接设置本机IP和端口 web_gui=[ip]:port 为网页地址， ip 可选，默认为 0.0.0.0，参考example 6

为了图形化界面能够接收到程序传输的数据并且刷新，在程序中，需要循环调用 api.wait_update的形式去更新和获取数据

推荐打开图形化界面的浏览器为Google Chrome 或 Firefox

创建当前TqApi的一个副本. 这个副本可以在另一个线程中使用

TqApi: 返回当前TqApi的一个副本. 这个副本可以在另一个线程中使用

symbol (str): 指定合约代码。

Quote: 返回一个盘口行情引用. 其内容将在 wait_update() 时更新.

注意: 1. 在 tqsdk 还没有收到行情数据包时, 此对象中各项内容为 NaN 或 0 2. 天勤接口从0.8版本开始，合约代码格式变更为 交易所代码.合约代码 的格式. 可用的交易所代码如下：

symbols (list of str): 合约代码列表

list of Quote: 返回一个列表，每个元素为指定合约盘口行情引用。

注意: 1. 在 tqsdk 还没有收到行情数据包时, 此对象中各项内容为 NaN 或 0 2. 天勤接口从0.8版本开始，合约代码格式变更为 交易所代码.合约代码 的格式. 可用的交易所代码如下：

获取指定合约的交易状态，便于实现开盘抢单功能。

TradingStatus: 返回指定合约交易状态引用。

请求指定合约及周期的K线数据. 序列数据会随着时间推进自动更新

list of str: 合约代码列表 （一次提取多个合约的K线并根据相同的时间向第一个合约（主合约）对齐)

duration_seconds (int): K线数据周期, 以秒为单位。例如: 1分钟线为60,1小时线为3600,日线为86400。 注意: 周期在日线以内时此参数可以任意填写, 在日线以上时只能是日线(86400)的整数倍, 最大为28天 (86400*28)。

data_length (int): 需要获取的序列长度。默认200根, 返回的K线序列数据是从当前最新一根K线开始往回取data_length根。 每个序列最大支持请求 10000 个数据

adj_type (str/None): [可选]指定复权类型，默认为 None。adj_type 参数只对股票和基金类型合约有效。 "F" 表示前复权；"B" 表示后复权；None 表示不做处理。

chart_id (str): [Deprecated] 由 api 自动生成，此参数不再使用

**获取后的 kline 对象请不要修改/重命名原始的 kline 序列，可能会导致后续的 kline 数据不更新 **

注：关于传入合约代码列表 获取多合约K线的说明：

主合约的字段名为原始K线数据字段，从第一个副合约开始，字段名在原始字段后加数字，如第一个副合约的开盘价为 "open1" , 第二个副合约的收盘价为 "close2"。

每条K线都包含了订阅的所有合约数据，即：如果任意一个合约（无论主、副）在某个时刻没有数据（即使其他合约在此时有数据）, 则不能对齐，此多合约K线在该时刻那条数据被跳过，现象表现为K线不连续（如主合约有夜盘，而副合约无夜盘，则生成的多合约K线无夜盘时间的数据）。

若设置了较大的序列长度参数，而所有可对齐的数据并没有这么多，则序列前面部分数据为NaN（这与获取单合约K线且数据不足序列长度时情况相似）。

若主合约与副合约的交易时间在所有合约数据中最晚一根K线时间开始往回的 10000*周期 时间段内完全不重合，则无法生成多合约K线，程序会报出获取数据超时异常。

datetime、duration是所有合约公用的字段，则未单独为每个副合约增加一份副本，这两个字段使用原始字段名（即没有数字后缀）。

暂不支持复权 获取多合约K线，若填入 adj_type，程序会报参数错误。

pandas.DataFrame: 本函数总是返回一个 pandas.DataFrame 实例. 行数=data_length, 包含以下列:

datetime: 1501080715000000000 (K线起点时间(按北京时间)，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数)

open: 51450.0 (K线起始时刻的最新价)

high: 51450.0 (K线时间范围内的最高价)

low: 51450.0 (K线时间范围内的最低价)

close: 51450.0 (K线结束时刻的最新价)

volume: 11 (K线时间范围内的成交量)

open_oi: 27354 (K线起始时刻的持仓量)

close_oi: 27355 (K线结束时刻的持仓量)

请求指定合约的Tick序列数据. 序列数据会随着时间推进自动更新

symbol (str): 指定合约代码.

data_length (int): 需要获取的序列长度。每个序列最大支持请求 10000 个数据

adj_type (str/None): [可选]指定复权类型，默认为 None。adj_type 参数只对股票和基金类型合约有效。 "F" 表示前复权；"B" 表示后复权；None 表示不做处理。

chart_id (str): [Deprecated] 由 api 自动生成，此参数不再使用

pandas.DataFrame: 本函数总是返回一个 pandas.DataFrame 实例. 行数=data_length, 包含以下列:

datetime: 1501074872000000000 (tick从交易所发出的时间(按北京时间)，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数)

last_price: 3887.0 (最新价)

average: 3820.0 (当日均价)

highest: 3897.0 (当日最高价)

lowest: 3806.0 (当日最低价)

ask_price1: 3886.0 (卖一价)

bid_price1: 3881.0 (买一价)

bid_volume1: 18 (买一量)

amount: 19237841.0 (成交额)

open_interest: 1941 (持仓量)

获取指定时间段内的 K 线序列，TqSdk 会缓存已经下载过的合约，提升代码执行效率、节约请求流量。

本接口仅限专业版用户使用，如需购买专业版或者申请试用，请访问 https://www.shinnytech.com/tqsdk-buy/。

该函数返回的对象不会更新，不建议在循环内调用该方法。

symbol (str): 指定合约代码。当前只支持单个合约。

duration_seconds (int): K 线数据周期, 以秒为单位。例如: 1 分钟线为 60，1 小时线为 3600，日线为 86400。 注意: 周期在日线以内时此参数可以任意填写, 在日线以上时只能是日线(86400)的整数倍

datetime: 指的是具体时间点，如果没有指定时区信息，则默认为北京时间

datetime: 指的是具体时间点，如果没有指定时区信息，则默认为北京时间

adj_type (str/None): [可选]指定复权类型，默认为 None。adj_type 参数只对股票和基金类型合约有效。 "F" 表示前复权；"B" 表示后复权；None 表示不做处理。

pandas.DataFrame: 本函数总是返回一个 pandas.DataFrame 实例。包含以下列:

datetime: 1501080715000000000 (K线起点时间(按北京时间)，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数)

open: 51450.0 (K线起始时刻的最新价)

high: 51450.0 (K线时间范围内的最高价)

low: 51450.0 (K线时间范围内的最低价)

close: 51450.0 (K线结束时刻的最新价)

volume: 11 (K线时间范围内的成交量)

open_oi: 27354 (K线起始时刻的持仓量)

close_oi: 27355 (K线结束时刻的持仓量)

获取指定时间段内的 tick 序列，TqSdk 会缓存已经下载过的合约，提升代码执行效率、节约请求流量。

本接口仅限专业版用户使用，如需购买专业版或者申请试用，请访问 https://www.shinnytech.com/tqsdk-buy/。

该函数返回的对象不会更新，不建议在循环内调用该方法。

symbol (str): 指定合约代码。当前只支持单个合约。

datetime: 指的是具体时间点，如果没有指定时区信息，则默认为北京时间

datetime: 指的是具体时间点，如果没有指定时区信息，则默认为北京时间

adj_type (str/None): [可选]指定复权类型，默认为 None。adj_type 参数只对股票和基金类型合约有效。 "F" 表示前复权；"B" 表示后复权；None 表示不做处理。

pandas.DataFrame: 本函数总是返回一个 pandas.DataFrame 实例。包含以下列:

datetime: 1501074872000000000 (tick从交易所发出的时间(按北京时间)，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数)

last_price: 3887.0 (最新价)

average: 3820.0 (当日均价)

highest: 3897.0 (当日最高价)

lowest: 3806.0 (当日最低价)

ask_price1: 3886.0 (卖一价)

bid_price1: 3881.0 (买一价)

bid_volume1: 18 (买一量)

amount: 19237841.0 (成交额)

open_interest: 1941 (持仓量)

获取一段时间内的交易日历信息，每年的交易日历将在交易所新一年的节假日安排后更新

datetime: 指的指的是该时间点所在年月日日期

datetime: 指的指的是该时间点所在年月日日期

pandas.DataFrame: 包含以下列:

date: (datetime64[ns]) 日期，为北京时间的日期

trading: (bool) 是否是交易日

list of str: 合约代码列表 （一次提取多个合约的K线并根据相同的时间向第一个合约（主合约）对齐)

n：返回 n 个交易日对应品种的主力, 默认值为 200

pandas.DataFrame: 包含 n 行数据，列数为指定主连合约代码个数加 1，有以下列:

date: (datetime64[ns]) 日期

如果返回的时间段中，还未上市的主连合约，其对应的标的合约值为空字符串。

rule (TqRiskRule): 风控规则实例，必须是 TqRiskRule 的子类型

rule (TqRiskRule): 风控规则实例，必须是 TqRiskRule 的子类型

发送下单指令. 注意: 指令将在下次调用 wait_update() 时发出

symbol (str): 拟下单的合约symbol, 格式为 交易所代码.合约代码, 例如 "SHFE.cu1801"

direction (str): "BUY" 或 "SELL"

offset (str): "OPEN", "CLOSE" 或 "CLOSETODAY" (上期所和上期能源分平今/平昨, 平今用"CLOSETODAY", 平昨用"CLOSE"; 其他交易所直接用"CLOSE" 按照交易所的规则平仓), 股票交易中该参数无需填写

volume (int): 下单交易数量, 期货为下单手数, A 股股票为股数

数字类型: 限价单，按照限定价格或者更优价格成交

None: 市价单，默认值就是市价单 (郑商所期货/期权、大商所期货支持)

"BEST": 最优一档，以对手方实时最优一档价格为成交价格成交（仅中金所支持）

"FIVELEVEL": 最优五档，在对手方实时最优五个价位内以对手方价格为成交价格依次成交（仅中金所支持）

None: 对于限价单，任意手数成交，委托单当日有效；对于市价单、最优一档、最优五档(与 FAK 指令一致)，任意手数成交，剩余撤单。

"FAK": 剩余即撤销，指在指定价位成交，剩余委托自动被系统撤销。(限价单、市价单、最优一档、最优五档有效)

"FOK": 全成或全撤，指在指定价位要么全部成交，否则全部自动被系统撤销。(限价单、市价单有效，郑商所期货品种不支持 FOK)

order_id (str): [可选]指定下单单号, 默认由 api 自动生成

account (TqAccount/TqKq/TqKqStock/TqSim): [可选]指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

Order: 返回一个委托单对象引用. 其内容将在 wait_update() 时更新.

发送撤单指令. 注意: 指令将在下次调用 wait_update() 时发出

order_or_order_id (str/ Order ): 拟撤委托单或单号

account (TqAccount/TqKq/TqKqStock/TqSim/TqSimStock): [可选]指定发送撤单指令的账户实例, 多账户模式下, 该参数必须指定

account (TqAccount/TqKq/TqKqStock/TqSim/TqSimStock): [可选]指定获取账户资金信息的账户实例, 多账户模式下, 该参数必须指定

Account / SecurityAccount: 返回一个账户对象引用. 其内容将在 wait_update() 时更新.

期货账户资金返回对象类型为 Account，股票账户资金返回对象类型为 SecurityAccount。

symbol (str): [可选]合约代码, 不填则返回所有持仓

account (TqAccount/TqKq/TqKqStock/TqSim/TqSimStock): [可选]指定获取持仓信息的账户实例, 多账户模式下, 必须指定

Position / SecurityPosition: 当指定了 symbol 时, 返回一个持仓对象引用. 其内容将在 wait_update() 时更新.

期货账户持仓返回对象类型为 Position，股票账户持仓返回对象类型为 SecurityPosition。

不填 symbol 参数调用本函数, 将返回包含用户所有持仓的一个tqsdk.objs.Entity对象引用, 使用方法与dict一致, 其中每个元素的key为合约代码, value为 Position。

注意: 为保留一些可供用户查询的历史信息, 如 volume_long_yd(本交易日开盘前的多头持仓手数) 等字段, 因此服务器会返回当天已平仓合约( pos_long 和 pos_short 等字段为0)的持仓信息

order_id (str): [可选]单号, 不填单号则返回所有委托单

account (TqAccount/TqKq/TqKqStock/TqSim/TqSimStock): [可选]指定获取委托单号的账户实例, 多账户模式下, 该参数必须指定

Order / SecurityOrder : 当指定了order_id时, 返回一个委托单对象引用. 其内容将在 wait_update() 时更新.

期货账户委托单返回对象类型为 Order，股票账户委托单返回对象类型为 SecurityOrder。

不填order_id参数调用本函数, 将返回包含用户所有委托单的一个tqsdk.objs.Entity对象引用, 使用方法与dict一致, 其中每个元素的key为委托单号, value为 Order

注意: 在刚下单后, tqsdk 还没有收到回单信息时, 此对象中各项内容为空

trade_id (str): [可选]成交号, 不填成交号则返回所有委托单

account (TqAccount/TqKq/TqKqStock/TqSim/TqSimStock): [可选]指定获取用户成交信息的账户实例, 多账户模式下, 该参数必须指定

Trade / SecurityTrade: 当指定了trade_id时, 返回一个成交对象引用. 其内容将在 wait_update() 时更新.

期货账户成交返回对象类型为 Trade，股票账户成交返回对象类型为 SecurityTrade。

不填trade_id参数调用本函数, 将返回包含用户当前交易日所有成交记录的一个tqsdk.objs.Entity对象引用, 使用方法与dict一致, 其中每个元素的key为成交号, value为 Trade

推荐优先使用 trade_records() 获取某个委托单的相应成交记录, 仅当确有需要时才使用本函数.

exchange_id (str): [可选] 交易所代码, 不填交易所代码则返回所有交易所风控规则 目前支持设置风控规则的交易所 SSE（上海证券交易所）、SZSE（深圳证券交易所）

RiskManagementRule: 当指定了 exchange_id 时, 返回该交易所的风控统计规则对象的引用.

不填 exchange_id 参数调用本函数, 将返回包含所有交易所风控规则的一个 tqsdk.objs.Entity 对象引用, 使用方法与dict一致, 其中每个元素的 key 为交易所代码, value为 RiskManagementRule。

设置交易所风控规则. 注意: 指令将在下次调用 wait_update() 时发出 调用本函数时，没有填写的可选参数会被服务器设置为默认值。

exchange_id (str): 交易所代码, "SSE" 或者 "SZSE"

enable (bool): 是否启用该规则

count_limit (int): [可选]最大自成交次数限制，如果未填写，服务器将根据交易所不同赋不同的默认值。

insert_order_count_limit (int): [可选]频繁报撤单起算报单次数，如果未填写，服务器将根据交易所不同赋不同的默认值。

cancel_order_count_limit (int): [可选]频繁报撤单起算撤单次数，如果未填写，服务器将根据交易所不同赋不同的默认值。

cancel_order_percent_limit (float): [可选]频繁报撤单撤单比例限额，为百分比，如果未填写，服务器将根据交易所不同赋不同的默认值。

trade_units_limit (int): [可选]成交持仓比起算成交手数，如果未填写，服务器将根据交易所不同赋不同的默认值。

trade_position_ratio_limit (float): [可选]成交持仓比例限额，为百分比，如果未填写，服务器将根据交易所不同赋不同的默认值，持仓数为该合约的净持仓。

RiskManagementRule: 返回一个风控规则对象引用. 其内容将在 wait_update() 时更新.

symbol (str): [可选]合约代码, 不填合约代码则返回账户下所有持仓合约的风控统计数据

RiskManagementData: 当指定了 symbol 时, 返回该合约下的风控统计数据对象引用. 其内容将在 wait_update() 时更新.

不填 symbol 参数调用本函数, 将返回包含用户所有持仓合约的一个 tqsdk.objs.Entity 对象引用, 使用方法与 dict 一致, 其中每个元素的 key 为合约代码, value为 RiskManagementData。

调用此函数将阻塞当前线程, 等待天勤主进程发送业务数据更新并返回

实际发出网络数据包(如行情订阅指令或交易指令等).

尝试从服务器接收一个数据包, 并用收到的数据包更新内存中的业务数据截面.

让正在运行中的后台任务获得动作机会(如策略程序创建的后台调仓任务只会在wait_update()时发出交易指令).

deadline (float): [可选]指定截止时间，自unix epoch(1970-01-01 00:00:00 GMT)以来的秒数(time.time())。默认没有超时(无限等待), 当 wait_update 有数据更新或者任务执行时会返回为 True，否则会陷入阻塞，使用 deadline 参数会让 wait_update 函数在当前时间大于 deadline 参数后返回 False ，避免长时间阻塞。非简单用法，如果设置的 deadline 过小，而计算任务过多，可能会导致处理任务堆积，引发潜在问题。

bool: 如果收到业务数据更新则返回 True, 如果到截止时间依然没有收到业务数据更新则返回 False

由于存在网络延迟, 因此有数据更新不代表之前发出的所有请求都被处理了, 例如:

可能输出 ""(空字符串), 表示还没有收到该合约的行情

当业务数据更新导致 wait_update 返回后可以使用该函数判断 本次业务数据更新是否包含特定obj或其中某个字段 。 如果 wait_update 设置了 deadline 参数，那么判断业务数据是否更新需要根据 wait_update 函数的返回值来确定。

关于判断K线更新的说明： 当生成新K线时，其所有字段都算作有更新，若此时执行 api.is_changing(klines.iloc[-1]) 则一定返回True。

obj (any): 任意业务对象, 包括 get_quote 返回的 quote, get_kline_serial 返回的 k_serial, get_account 返回的 account 等

不指定: 当该obj下的任意字段有更新时返回True, 否则返回 False.

str: 当该obj下的指定字段有更新时返回True, 否则返回 False.

list of str: 当该obj下的指定字段中的任何一个字段有更新时返回True, 否则返回 False.

bool: 如果本次业务数据更新包含了待判定的数据则返回 True, 否则返回 False.

obj (pandas.Dataframe): K线数据

