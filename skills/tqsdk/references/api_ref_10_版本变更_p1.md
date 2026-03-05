# Api Ref - Part 10

**Topics:** 版本变更

---


# Api Ref - Part 10

## 版本变更

**URL:** https://doc.shinnytech.com/tqsdk/latest/version.html

**Contents:**
- 版本变更

新增: query_edb_data() 接口，获取非量价数据

修复: 回测/下载数据在行情服务运维阶段发生断线重连，重连成功后没有发送已订阅请求，导致回测/下载数据无法继续推进的问题

修复: is_changing() 接口无法正确判断 tick 中的字段更新

优化: query_symbol_settlement() 和 query_symbol_settlement() 接口复用 tcp 连接

docs: 提供 trae IDE 配置方式

自该版本起仅支持 Python >=3.8

新增：TqRuleOrderRateLimit 类，设置一个 API 实例每秒最大订单操作次数限制

修复: websockets 10.0 版本引发的连接失败问题

新增: get_trading_status 支持订阅期权交易状态

修复: DataDownloader 计算复权因子错误

修复: 回测模式下，get_kline_serial() 订阅多合约 K 线时可能出现的越界报错

新增: 多策略历史结算查询，详情参考 TqSdk 多策略使用手册

修复: query_his_cont_quotes() 接口获取主力合约不一致的问题

修复: pandas==2.3.0 时，初始化 TqApi 报错

修复：query_all_level_finance_options() 对中金所股指期权标的检查

新增: TqTradingUnit 账户类型，支持多策略交易，详情参考 TqSdk 多策略使用手册

修复: numpy 移除 NaN 常量导致的指标函数执行失败

docs: 增加上交所深交所 ETF 标的合约代码示例

修复: 因郑商所三位合约导致合约信息中部分字段错误的问题

docs: 修正部分文档，补充 wait_update 的 deadline 参数说明

自该版本起不再支持使用 api._data['quotes'] 获取全部合约

修复: 兼容 websockets 14.0 版本

docs: TqCtp、TqJees、TqRohon、TqYida 补充文档 front_url 示例

新增: Quote 增加 position_limit 属性，返回合约持仓限额

增加: TqApi 增加 query_symbol_settlement() 接口，支持查询合约每日结算价

增加: TqAuth 增加 expire_datetime 属性，返回快期账户授权到期时间

自该版本起仅支持 Python >=3.7

新增：TqJees 账户类型，支持杰宜斯资管柜台

新增：TqYida 账户类型，支持易达交易柜台

新增：TqRohon 账户类型，支持融航资管柜台

BREAKING：从安装依赖中移除 tqsdk_zq_otg 模块，用户使用多柜台需要手动安装依赖包：pip install -U tqsdk_zq_otg

修复：在 Windows 系统上使用 TqCtp 账户导致多 TqApi 实例无法运行的问题

修复：TqCtp 文档 demo 代码拼写错误

新增：TqCtp 账户类型，支持直连 CTP 柜台

优化：仅在发送过订阅请求后，才会发送退订请求

增加：支持在 Jupyter 中使用 Tqsdk，详情参考 在 Jupyter Notebook 中使用 TqSdk

修复：tafunc 中部分计算函数对 get_kline_data_series()、get_tick_data_series() 返回的 duration 列单位处理错误的问题

优化：tqsdk 内部 task 的取消机制，避免了部分 task 无法正常取消的问题

docs: 修正文档，支持的 python 版本为 3.7, 3.8, 3.9, 3.10, 3.11, 3.12

修复：某些情况下网络连接发生超时错误时，可能无法重连的问题

优化：get_kline_serial()、get_tick_serial() 在请求没有任何成交数据的合约时能够及时退出，避免等待超时

更新：get_kline_serial()、get_tick_serial() 接口去掉 chart_id 参数，避免用户错误用法导致服务器收到大量重复请求

增加：query_all_level_finance_options() 增加中金所股指期权标的

优化：get_kline_data_series()、get_tick_data_series() 接口在请求没有任何成交数据的合约时能够及时退出，避免等待超时

增加：DataDownloader 增加 write_mode 参数，作为写入模式参数，并且在模式为 'a' 时，不写入标题行

修复：用户在使用 TargetPosScheduler 时可能出现重复创建 TargetPosTask 实例的问题

优化：DataDownloader 在下载没有任何成交数据的合约时能够及时退出，避免等待超时或报错

优化：重构 TargetPosTask 退出时释放资源部分，优化异步代码中 TargetPosTask 的使用方法

优化：回测时，如果订阅没有任何成交数据的合约，构造的空数据给下游，避免程序一直等待

优化：对发送给合约服务器的数据进行检查，避免发送不合法的数据，提前报错通知用户

修复：get_kline_data_series()、get_tick_data_series() 接口在指定时间段没有数据时报错

修复：query_quotes() 函数无法查询广州期货交易所 GFEX 的主连合约和主力合约

修复：TqSim 在调用 set_margin 之后，使用 is_changing 判断某个对象是否更新，可能返回的结果不正确

twap_table() 不需要用户多次指定账户

修复：回测时，订阅多合约 K 线时，成交可能不符合预期的问题

修复：使用 TargetPosScheduler，并且最后一项调仓目标为 0 时，可能出现任务无法结束的问题

修复：回测时如果订阅多个不同周期 K 线时，成交可能不符合预期的问题

open_max_market_order_volume()、open_max_limit_order_volume()、 open_min_market_order_volume()、open_min_limit_order_volume()、 categories()

修复：query_his_cont_quotes() 接口在 3.4.11、3.5.0 版本上报错的问题

新增：行情增加外盘主连合约，通过 api.query_quotes(exchange_id=['KQD']) 查询外盘合约，外盘合约地址参考 外盘行情

新增：TqZq 众期账户类型，支持连接众期服务器交易

优化：query_quotes() 函数 ins_class、exchange_id、product_id 参数支持 list

修复：ticks 回测时，可能出现账户结算信息为 nan 的问题

对于以下接口，用户输入 datetime 类型参数时，如果未指定时区信息，tqsdk 会指定为北京时间；如果指定了时区信息，会转为北京时间，保证 Unix Timestamp 时间戳不变。 get_kline_data_series()、get_tick_data_series()、DataDownloader、TqBacktest。

修复：pandas 2.1.0 版本 fillna 、NumericBlock 会报 deprecated warning 的问题

优化：磁盘空间剩余小于10G，默认不写入日志

修复：回测时 TqSim 可能出现 volume_short_today 为负数的问题

修复：修正多个 TqKq 快期模拟和辅模拟账户 account_key 重复的问题

修复：TargetPosTask 及 twap 添加纯碱期货 2309 合约及 2310 合约暂不支持的提示

docs：完善 TqKq 快期模拟辅模拟账户使用说明

增加：支持同时使用多个快期模拟账户，TqKq、TqKqStock 加入多帐号支持

修复：回测后 trade_log 中持仓信息增加 pos_long、pos_short、pos 字段

优化：TqApi 初始化时账户相关参数的提示信息

修复：TargetPosTask 及 twap 支持红枣期货下单

修复: 升级 numpy 后，tafunc.barlast 报错的问题

api：某些 pandas 版本下，web_gui 不更新绘制序列

docs：修改论坛地址，增加支持杰宜斯的说明

修复: 回测时，部分情况下 expired 字段错误

增加：支持国密连接，可以在 TqAccount() 构造时指定 sm 参数为 True 来启用. 当启用国密时仅支持: win7 或以上, ubuntu 22.04 或以上, debian 12 或以上

增加：支持广州期货交易所 GFEX，如果用户需要交易广期所合约需要升级到此版本以上

优化: query_all_level_finance_options() 增加 ETF 期权标的，文档补充完整 ETF 基金名称

docs：修正文档，添加上交所和深交所中证1000ETF和深交所创业板ETF代码示例

增加：下载数据时 csv_file_name 参数支持 str / asyncio.StreamWriter 两种类型

修复：vwap_table 手数计算错误的问题

增加：增加中证 1000 指数，免费用户可获取该指数行情，参考文档 合约, 行情和历史数据

修复：回测中没有正常更新 quotes 下的 expire_rest_days 字段的问题

修复：回测 web_gui 图表没有显示成交标注、持仓线的问题

增加：下载 tick 数据时增加 average 列

增加：get_tick_data_series() 接口返回值中增加 average 列

优化：tqsdk 内部各个模块使用统一的时间处理函数

修复：TargetPosTask 及 twap 增加添加普麦、早籼稻、粳稻及晚籼稻期货暂不支持的提示

修复：query_symbol_ranking() 接口某些情况可能报错的问题

修复：下载多合约 klines 时数据可能未完全收全的问题

修复：支持多进程下使用 get_kline_data_series()、get_tick_data_series() 接口

优化：TqApi 的 debug 默认值修改为 None，且 debug 为 None 情况下在磁盘剩余空间大于 3G 时才可能开启日志

docs：增加 ETF 期权本地计算卖方保证金示例 o74，完善 targetpostask 的示例文档，完善 Position 下 orders 定义，统一修正文档大小写、变量命名等

修复：修正深交所 ETF 期权的昨结算（pre_settlement）字段未正确显示的问题

修复：修正上交所 ETF 期权的昨结算（pre_settlement）字段未正确显示的问题

修复：TargetPosTask 及 twap 添加强麦期货暂不支持的提示

修复：api.insert_order 没有检查 advanced 参数

优化：某些情况下启用 web_gui 后网页卡顿的问题

修复：修正上交所 ETF 期权的昨结算（pre_settlement）字段

修复：TargetPosTask 及 twap 添加动力煤期货暂不支持的提示

docs：修正文档，增加 tqkq() 的示例，增加 在 TqSdk 中调用 TqSdk2 查询保证金 文档

修复：query_all_level_options 接口查询 ETF 期权可能报错的问题

修复：提升程序在连续订阅 K 线时的运行速度

修复：使用快期模拟账户交易，在断线重连后程序可能报错的问题

优化：规范化测试脚本，能尽早发现由于依赖库版本升级，而导致部分代码写法不兼容的错误

docs：修正文档字体显示格式，增加股票回测文档 对股票合约进行回测

优化：打印通知时，显示期货账户，改善多账户下用户使用体验

优化：免费用户 每日回测 3 次，支持其回测时交易股票；专业版用户 回测次数及交易品种不受限制，专业版购买网址：https://account.shinnytech.com。

修复：linux 下使用多进程时，报单号可能重复的问题

docs：修改交易相关的 get 系列函数文档及示例代码

TqSdk 计划在 20220601 之后放弃支持 Python 3.6 版本，请尽快升级 Python 版本。 建议升级到 3.8 及以上，以保证所有依赖库都可以使用最新版。

新增：TqSimStock 类实现本地股票模拟交易，同时支持在实盘/回测模式下使用。 专业版用户 可用，专业版购买网址：https://account.shinnytech.com。

web_gui：修复回测时不能正常显示结果报告的问题

修复：windows 下调用 get_kline_data_series() 时，可能出现缓存文件不允许重复重命的问题

新增：为各种账户类型增加接口调用，支持 IDE 更好的提供代码提示。TqSdk 目前支持以下账户类型 TqAccount、TqKq、 TqKqStock、TqSim，本次重构为以上账户类型分别添加了 get_account、get_position、get_order、get_trade 几个接口，明确了其返回值的类型。

例如：TqKq 实例调用 get_account() ，返回 Account 类型实例；

TqKqStock 实例调用 get_account() ，返回 SecurityAccount 类型实例。

修复：TargetPosTask 及 twap 增加添加红枣期货暂不支持的提示

修复：从服务器更新节假日表，修复 get_trading_calendar() 接口文档及报错信息

修复：调用 get_kline_serial() 接口获取股票前复权 Kline 时，复权计算结果可能出错的问题

新增：节假日表添加 2022 年节假日信息

新增：支持在 python 3.10 下使用 TqApi

修复：调用 query_symbol_info()，当参数中包含主连/指数合约会报错的问题

修复：在某些情况下，回测时获取期权及标的合约的多合约 Kline 可能报错的问题

修复：回测时取主连合约，如果用 quote.underlying_quote 直接读取标的合约，在标的合约变更时，可能未重新订阅行情的问题

优化：取消网络连接关闭时屏幕输出，改为存入日志文件

docs：完善 get_account()、get_position()、get_order()、 get_trade() 函数返回值类型文档说明，完善专业版 股票模拟交易 文档，完善 tqsdk.TqRohon - 融航资管交易类 融航接入文档

增加：TqKqStock 快期股票模拟 账户类型，支持股票模拟交易。专业版用户 可用，专业版购买网址：https://account.shinnytech.com。

增加：TqRuleAccOpenVolumesLimit 类，日内累计开仓手数限制

优化：使用 sgqlc 库生成合约服务的 graphql 查询

增加：query_symbol_info() 接口返回值中增加 upper_limit, lower_limit 这两个字段

优化: query 系列函数，发送的查询请求中合约列表长度不能大于 8192

增加：TqRuleOpenCountsLimit、TqRuleOpenVolumesLimit 类， 以及 add_risk_rule()、delete_risk_rule() 接口，支持本地风控功能

增加：TqRiskRuleError 错误类型，可以捕获风控触发的错误

修复：实盘账户无法使用 get_trading_status() 接口的问题

增加：get_trading_status() 接口，支持开盘抢单功能

增加：query_symbol_info() 接口返回值中增加 product_id, expire_rest_days, trading_time_day, trading_time_night 几个字段

优化：TqSim 回测报告增加部分字段，和 web_gui 显示回测报告一致

优化：get_kline_data_series()、get_tick_data_series() 接口报错

增加：query_symbol_info() 接口返回值中增加 pre_open_interest, pre_settlement, pre_close 这三个字段

优化：简化回测结束后用户依然需要查看 web_gui 时的代码，详情参考 回测结束在浏览器中查看绘图结果

优化：网络连接失败时，优化对用户的提示信息

优化：实盘账户实盘不支持主连和指数交易，提前抛错提示用户

docs：更新文档，专业版承诺提供A股股票行情

增加：TqApi 增加 query_his_cont_quotes() 接口，可以获取过去 n 个交易日的历史主连信息

增加：通知模块 TqNotify，帮助用户收集通知信息并做定制化处理

docs：完善风控文档，增加专业版权限函数说明

增加：TqApi 增加 query_symbol_ranking() 接口，支持查询合约成交排名/持仓排名。

增加：TqApi 增加 query_option_greeks() 接口，返回指定期权的希腊指标。

修复：pyinstaller 工具由于缺少初始合约文件导致打包失败

优化：get_delta()、get_theta()、get_rho()、 get_bs_price()、get_impv() 接口中 option_class 参数支持类型扩展为 str 或者 pandas.Series，详情见文档

修复：由于缺少初始合约文件，TqApi 初始化可能失败的问题

增加：is_changing 接口增加对于委托单 is_dead()、is_online()、 is_error()、trade_price() 字段支持判断是否更新

优化：将已知下市合约直接打包在代码中，缩短 TqApi 初始化时间

docs：完善主力切换规则说明，将阿里源替换为清华源

增加：is_changing 接口增加对于合约 expire_rest_days()，持仓 pos_long()、 pos_short()、pos() 字段支持判断是否更新

修复：2.8.1 版本重构后，不支持多线程运行的问题

增加：增强在协程中的支持，以下接口 query_quotes()，query_cont_quotes()， query_options()，query_atm_options()， query_symbol_info()，query_all_level_options()， query_all_level_finance_options()，支持协程中 in_options, at_options, out_options = await api.query_all_level_finance_options("SSE.510300", 4.60, "CALL", nearbys = 1) 写法，参考文档：单线程创建多个异步任务

修复：target_pos_task 优化报错提示，已经结束的 TargetPosTask 实例再调用 set_target_volume 设置手数会报错。参考文档：cancel()

修复：下载历史数据时，某些数据未按照最小价格变动单位保留相应小数位数的问题

重构：优化 wait_update、is_changing 接口的实现，增强对协程的支持

增加：支持在回测中使用 query 系列函数，查询结果为回测当天的合约信息

增加：Quote 对象增加 underlying_quote 属性，值是一个 Quote 对象（为 underlying_symbol 属性对应的合约引用）或者是 None

web_gui：修复在 safari 和 firefox 无法正常显示的问题

修复：query 系列查询看跌期权时，未返回指定的实值、平值、虚值序列的问题

docs：完善 position 文档说明

增加：去除 Cython 编译，本地代码全部开源

增加：支持 ARM 架构下 CPU 的安装使用

增加：TqApi 增加 query_all_level_finance_options() 接口，支持查询指定当月、下月、季月等到期月份的金融期权。

增加：支持上期能源下载 ticks 5 档行情

修复：某些参数可能造成 twap 无法执行的问题

修复：客户端发送的 variables 中变量值不支持空字符串、空列表或者列表中包括空字符串

删除：为期权持仓、成交、委托单对象添加部分期权合约信息的功能（2.6.5 增加功能）

doc：添加隔夜开盘抢单示例，不再建议用户自定义次席连接

修复：支持 pandas 1.3.0 版本

修复：回测中某些有夜盘的合约，报夜盘时间不在可交易时间段的问题

web_gui：成交列表中成交价格默认显示4位小数

增加：为期权持仓、成交、委托单对象添加部分期权合约信息，方便用户查看

增加：回测时，Quote 对象支持读取 expired 值

修复：TargetPosScheduler 最后一项等到目标持仓完成退出，最后一项设置的超时时间无效

修复：回测时如果先订阅日线，可能出现无法成交的问题

doc：完善期权文档、增加 TqSdk 企业版 文档说明

增加：Quote 增加 expire_rest_days 属性，表示距离到期日天数

增加：TqApi 增加 query_symbol_info() 接口，支持批量查询合约信息

增加：TqApi 增加 query_all_level_options() 接口，返回标的对应的全部的实值、平值、虚值期权

增加：TqApi 中 query_atm_options() 接口，扩大参数 price_level 支持范围

增加：sim.tqsdk_stat 增加总手续费字段

修复：回测中某些有夜盘的合约，报夜盘时间不在可交易时间段的问题

修复：回测报告中，在有期权交易时，每日收益值有错误

