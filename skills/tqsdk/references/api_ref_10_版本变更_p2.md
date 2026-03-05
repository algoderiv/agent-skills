修复：回测中限制 get_quote_list() 参数列表长度，最多支持 100 合约

web_gui：修复部分成交记录箭头标注位置不对的问题

web_gui：修复报告页面日期没有显示的问题

web_gui：支持代码运行中可以修改指标颜色

web_gui：成交列表中，部分成交价格没有按照最小变动价格保留小数位数的问题

修复：twap 策略某些参数组合无法执行的问题，修改后生成随机手数可能最后一笔的下单手数小于设置的最小手数

修复：TqSim 模拟交易期权时，某些情况下标的行情不更新的问题

完善文档：增加指数、主连行情、期权使用文档说明

web_gui：增加回测报告图表页面（增加每日资金、每日盈亏、滚动夏普比率、滚动索提诺比率图表）

修复：在回测某些时间段时，指数无法交易的问题

重构：TqSim 回测统计函数重构，增加 sortino_ratio 索提诺比率指标

优化：target_pos_task 报错提示文字

优化：单线程创建多个异步任务文档完善，参考文档：单线程创建多个异步任务

web_gui：修复成交量图在高分屏下高度错误的问题

web_gui：图表不显示 BoardId

增加：增强在协程中的支持，以下接口 get_quote()，get_quote_list()， get_kline_serial()，get_tick_serial() 支持协程中 quote = await api.get_quote('SHFE.cu2106') 写法，参考文档：单线程创建多个异步任务

增加：vwap_table() 的示例代码，参考链接 vwap_table - 交易量平均加权算法

优化：twap_table() 的示例代码，参考链接 twap_table - 时间平均加权算法

优化：在网络链接开始尝试重连时，增加通知和日志

修复：多次创建同合约 TargetPosTask 实例，可能抛错的问题

增加：tqsdk.algorithm 模块提供 vwap_table() 帮助用户完成 vwap 算法下单。

增加：TqTimeoutError 错误类型，方便用于捕获此错误

增加：TargetPosTask 实例提供 cancel()、is_finished() 方法

修复：在异步代码中调用 get_quote 函数时，可能遇到 Task 未被引用而引发的错误

修复：Windows 中下载数据时，文件已经被占用而无法继续下载时，TqSdk 没有正常退出的错误

优化：针对初始化时的可能出现超时退出的问题，增加错误收集和提示

增加：负责策略执行工具 TargetPosScheduler，帮助用户完成复杂的下单策略，同时提供给用户极大的调整空间。文档参考 基于时间维度目标持仓策略

修复：协程中调用 get_quote 可能超时的问题

增加：get_quote_list() 接口，支持批量订阅合约。注意其参数和返回值都是 list 类型。

增加：版本通知功能，后续版本升级将在 TqSdk 版本大于等于 2.5.0 以上版本做通知

优化：TqApi 初始化逻辑，减少了一大半 TqApi 初始化时间

增加：TqSim 支持 BEST / FIVELEVEL 市价单

修复：回测情况下可能遇到单个合约行情回退的问题

修复：get_position 获取持仓添加默认的 exchange_id, instrument_id

修复：回测时用到多合约 Kline 且其中某个合约在回测区间内下市，可能导致程序崩溃

重构：合约服务模块独立为一个模块，增加了查询合约服务等待时间，减少了api初始化创建失败的概率

增加：twap 增加 trades，average_trade_price 属性，用于获取成交记录和成交均价

增加：query_cont_quotes 接口增加 has_night 参数，详情参考 query_cont_quotes()

增加：支持用户回测中设置 TqSim 的保证金和手续费，详情参考 set_margin()、set_commission()、get_margin()、get_commission()

增加：支持用户回测中使用 quote.underlying_symbol 获取主连对应的主力合约，详情参考 回测时获取主连合约标的

修复：回测时大于日线周期的 K 线的收盘时间错误

重构： TqSim 模拟交易模块，修复了 TqSim 模拟交易期权时部分字段计算错误的问题，增加测试用例覆盖，提高 TqSim 模块准确性

修复：TargetPosTask 能支持多账户下使用

修复：之前版本下载无任何成交的合约会显示在 0% 卡住或退出程序，修改为超时（30s）之后跳过该无成交合约下载后续合约

完善文档：增加 TargetPosTask 大单拆分模式用法示例，修改完善期权文档等

依赖库升级：pandas 版本要求为 >= 1.1.0

增加：TargetPosTask 增加 min_volume, max_volume 参数，支持大单拆分模式，详情参考 TargetPosTask

重构：TqSim 模拟交易模块，修复了 TqSim 模拟交易时账户、持仓部分资金字段计算错误的 bug

修复：query_options(), query_atm_options() 接口中 has_A 参数不生效的 bug

修复：在使用 TargetPosTask 时，主动调用 api.close() 程序不能正常退出的错误的 bug

修复：回测时使用多合约 Kline 可能引起的 bug

修复：在节假日时回测，由于节假日当日无夜盘而导致部分夜盘品种的交易时间段错误

修复：web_gui 在切换合约/周期时未更新用户绘图数据的 bug

修复：web_gui 幅图数据默认保留两位小数显示

修复获取交易日历接口在低版本 pandas 下结果可能出错的问题

增加 get_trading_calendar() 接口，支持用户获取交易日历

增加 query_atm_options() 接口，支持用户获取指定档位期权

修复在回测时订阅当天上市的合约可能出现报错的情况

修复 web_gui 回测时某些情况下定位不准确的问题

优化 query_quotes() , 支持用户查询交易所的全部主连或指数

增加 t96.py macd 绘图示例，详情参考 t96 - 附图中画MACD

修复获取大量合约的多合约Kline，有可能等待超时的问题

web 优化图表，回测时图表跳转到回测时间段

回测增加支持获取多合约 Kline，现在可以在回测中使用期权相关函数

TqSim 增加属性 tqsdk_stat，提供给用户查看回测交易统计信息，详情参考 策略程序回测

修复 twap 可能少下单的问题，增加针对 twap 的测试用例

增加接口 get_kline_data_series()、get_tick_data_series()，支持 专业版用户 获取一段时间 K 线或 Tick 的用法

修复 web 需要拖拽才能更新 K 线的问题，支持自动更新 K 线

修复下载多合约 K 线，列名顺序错误的问题

修复 web 盘口总手数可能显示错误的问题

修复 draw_text 设置颜色无效的问题

复权统一命名规范 "F" 表示前复权，"B" 表示后复权，请检查您的代码是否符合规范

修复下载复权数据时，由于下载时间段无复权信息，可能导致失败的问题

修复在 get_kline_serial / get_tick_serial 在 pandas=1.2.0 版本下用法不兼容的问题

修复新用户第一次安装 TqSdk 可能遇到依赖库 pyJWT 版本不兼容的错误

修复 web_gui 拖拽不能缩小图表的问题

修复 twap 在退出时由于未等待撤单完成，可能造成重复下单的问题

修复 twap 未按时间随机，成交后立即退出的问题

修复在复盘模式下 TqSim 设置初始资金无效

修复回测模式下连接老版行情服务器无法运行问题

支持获取复权后 klines/ticks，详情请参考文档 get_kline_serial()、get_tick_serial()

支持下载复权后 klines/ticks，详情请参考文档 DataDownloader

Quote 对象增加除权表(stock_dividend_ratio)，除息表(cash_dividend_ratio) 两个字段，详情请参考文档 Quote

修复 twap 算法在手数已经成交时状态没有变为已结束的 bug

修复文档中 reference/tqsdk.ta 页面内不能跳转连接

修复用户使用 pyinstaller 打包文件，不会自动添加穿管认证文件和 web 资源文件的问题

更换 web_gui 绘图引擎，极大改善 web_gui 交互性能

由于后续行情服务器升级等原因，建议用户 2020/12/31 号前将 tqsdk 升级至 2.0 以上版本

增加计算波动率曲面函数，详情参考 VOLATILITY_CURVE()

TargetPosTask 支持 price 参数为函数类型，详情参考 TargetPosTask

优化下载数据体验，已下市无数据合约提前退出

修复在复盘情况下会持续重复发送订阅合约请求的问题，可以改善复盘连接成功率

修复 twap 在某些边界条件下无法下单的 bug

修复 linux 平台下 web_gui 可能因为端口占用无法启动网页

DataDownloader.get_data_series() 函数使用可能导致内存泄漏，暂时下线修复

下载数据工具支持默认下载 ticks 五档行情

下载数据工具增加 get_data_series 接口，可以获取 dataframe 格式数据，详情请参考 get_data_series()

修复 twap 算法可能无法持续下单的 bug

增加多账户功能，详情请参考 multiaccount

优化日志模块，明确区分屏幕输出、日志文件中的日志格式，并在 TqApi 中提供参数 disable_print，可以禁止 TqApi 在屏幕输出内容，详情请参考 TqApi

Python >=3.6.4, 3.7, 3.8, 3.9 才能支持 TqSdk 2.1.0 及以上版本

优化：Quote 对象增加若干字段：instrument_name、 exercise_year、exercise_month、last_exercise_datetime、exercise_type、public_float_share_quantity，详情请参考文档 Quote

修改：query_options 接口参数名调整，兼容之前的用法

修复：CFFEX.IO 指数回测可能报错的bug

修复：快期模拟在 web_gui 中优化用户名显示

修复：未设置过 ETF 期权风控规则的账户首次设置风控规则时可能报错

优化文档：增加 query 系列函数返回数据类型的注释

增加 Python 支持版本说明(3.6/3.7/3.8)

修复 2020/08/03-2020/09/15 时间内下市合约查询失败的问题

增加下载委托单和成交记录的示例 downloader_orders - 下载委托单和成交记录

增加 algorithm 算法模块，增加 twap 算法以及对应的 demo 示例 twap_table - 时间平均加权算法

2020/10/01 以后，免费版用户不再支持回测，下载数据等功能，点击了解专业版和免费版区别

修改中证 500 的合约名称为 SSE.000905

修改 TqAccount 检查参数类型并提示用户

股票行情正式上线，点击查看详情 合约, 行情和历史数据

发布 TqSdk 专业版，点击查看详情 TqSdk 专业版

支持 ETF 期权交易，支持的期货公司名单参见 点击查看详细说明

提供新版合约接口服务 query_quotes()、query_cont_quotes()、query_options()，替代原有 _data 用法，建议尽早换用

增加设置、读取 ETF 期权风控规则的接口，set_risk_management_rule()、get_risk_management_rule()

增加 TqAuth 用户认证类，使用 TqApi 时 auth 为必填参数，TqAuth，兼容原有 auth 用法。

修复：pandas 的 consolidate 函数调用可能会造成 K 线数据不更新

修复：api.insert_order 没有检查大商所期权不支持市价单

优化用户 import pandas 遇到 ImportError 时问题提示

更新优化文档，增加股票相关示例，更新示例中的期货合约，标注文档中 objs 对象类型说明

增加提供高级委托指令 FAK、FOK，并增加相关文档说明 高级委托指令、示例代码

本地模拟交易 sim 支持 FAK、FOK 交易指令，快期模拟暂不支持

优化测试用例代码，增加关于交易指令的测试用例

增加 TqKq 账户类型，可以使用统一的快期模拟账户登录，详情点击 模拟交易和论坛

支持 with TqApi() as api 写法

quote 对象增加 exchange_id 字段，表示交易所代码

重构 sim 模块代码，便于接入新版行情服务器

修复 settargetpos 回测时，在一个交易时段内最后一根 K 线下单无法成交的 bug

修复 ticksinfo 函数在 pandas 版本低于 1.0.0 无法正常使用的 bug

更新 logo，整理优化文档，增加股票行情、主连获取主力等文档说明，优化示例代码目录结构

股票行情测试版发布，_stock 参数设置为 True 可以连接测试行情服务器，提供股票数据 详细说明请点击查看

增加计算 ticks 开平方向函数(详见: get_ticks_info() )

增加期权指标的计算公式 (希腊值、隐含波动率、理论价等)

增加TqSim模拟交易成交时间判断 (非交易时间段下的委托单将被判定为错单，以减小模拟帐号与实盘的差距)

增加账户、持仓中的市值字段 (如果交易了期权，则模拟帐号的账户、持仓字段的定义有一些改变(详见: tqsdk.objs.Account ))

优化文档 (增加 期权交易 & 交易所官方组合 文档内容、增加在 在无人监控环境下执行策略 教程内容、优化文档其他细节）

修复vscode 插件中不能登录交易的bug

修改 web_gui 默认显示的 ip 地址为 127.0.0.1

修复 web_gui 不显示成交记录箭头的问题

策略结束后 api 将关闭所有 web 链接

增加 Quote 的 option_class (期权方向)和 product_id (品种代码)字段

修复 web_gui 不显示成交记录的问题

修复 python3.8 下设置 web_gui 参数无效的问题

交易网关升级, 所有用户需升级至 1.6.0 版本以上

修复参数搜索时由于 TargetPosTask 单实例造成的内存泄漏

web_gui 参数格式改成 [ip]:port, 允许公网访问

改进 web 界面，增加分时图，优化盘口显示内容，修复相关问题

修改 barlast() 的返回值为 pandas.Series 类型序列

优化 TqApi 参数 web_gui, 允许指定网页地址和端口(详见: 策略程序图形化界面 )

更新优化 vscode 插件以及web 页面

增加 barlast 功能函数(详见: barlast() )

修复 TargetPosTask 进行参数搜索时无法正确执行的bug

TqSim 的 trade_log 改为公开变量

从当前版本开始，不再支持天勤终端合约代码图形显示

修复 web_gui 功能中的部分已知问题

修复使用 slave/master 多线程模式时的报错问题

从 logger 中分离从服务器返回的通知信息(以便单独处理或屏蔽)

修复使用 TargetPoseTask 实例时可能引发的报错

修复在填写了画图的 color 参数时引起的报错

修复在 vscode 插件和天勤终端中不能运行策略的bug

支持通过 tqsdk.TqApi 内 设置 web_gui=True 参数以实现实盘/回测的图像化支持 , (详见: 策略程序图形化界面 )

完善 TqSdk 各公开函数的参数类型标注及函数返回值类型标注

将 api 中除业务数据以外的所有变量私有化

完善 insert_order() 函数返回的 order 的初始化字段：增加 limit_price、price_type、volume_condition、time_condition 字段

增加 quote 行情数据中的 trading_time、expire_datetime、delivery_month、delivery_year、ins_class 字段

删除 quote 行情数据中的 change、change_percent 字段

支持同时获取对齐的多合约 K 线 (详见 get_kline_serial() )

修复回测时未将非 TqSim 账号转换为 TqSim 的 bug

修复 wait_update() 填写 deadline 参数并等待超时后向服务器发送大量消息

增加时间类型转换的功能函数 (详见 tafunc() )

修改回测log内容,去除回测时log中的当前本地时间

修复: 命令行运行策略文件时,复盘模式下的参数返回值

修复: register_update_notify 以 klines 作为参数输入时报错的bug

修复: 因不能删除业务数据导致的内存泄漏bug

部分修复: diff中的数据不是dict类型导致的bug

修复: TqApi.copy()创建slave实例时工作不正常的bug

增强TargetPosTask及指标函数等内容的说明文档

去除get_order()及get_position()等函数的返回值中与业务无关的"_path", "_listener" 数据, 使其只返回业务数据

持仓对象 Position 增加了实时持仓手数属性 pos_long_his, pos_long_today, pos_short_his, pos_short_today ，这些属性在成交时与成交记录同步更新

修正 TargetPosTask 因为持仓手数更新不同步导致下单手数错误的bug

TqApi 增加 copy 函数，支持在一个进程中用master/slave模式创建多个TqApi实例

Quote, Account, Position, Order, Trade 的成员变量名在IDE中支持自动补全(Pycharm测试可用)

Order 增加了 is_dead() 属性 - 用于判定委托单是否确定已死亡（以后一定不会再产生成交）

Order 增加了 is_online() 属性 - 用于判定这个委托单是否确定已报入交易所（即下单成功，无论是否成交）

Order 增加了 is_error() 属性 - 用于判定这个委托单是否确定是错单（即下单失败，一定不会有成交）

Order 增加了 trade_price() 属性 - 委托单的平均成交价

Order 增加了 trade_records() 属性 - 委托单的成交记录

加入期货公司次席支持, 创建 TqAccount 时可以通过 front_broker 和 front_url 参数指定次席服务器

(BREAKING) 模拟交易默认资金调整为一千万

加入穿透式监管支持. 用户只需升级 TqSdk 到此版本, 无需向期货公司申请AppId, 即可满足穿透式监管信息采集规范要求.

(BREAKING) TqApi.get_quote, get_kline_serial, get_account 等函数, 现在调用时会等待初始数据到位后才返回

(BREAKING) k线序列和tick序列格式改用pandas.DataFrame

增加 数十个技术指标 和 序列计算函数, 使用纯python实现. 加入ta和ta_func库

加入策略单元支持. 在一个账户下运行多个策略时, 可以实现仓位, 报单的相互隔离

加强与天勤终端的协作，支持策略程序在天勤中画图, 支持回测结果图形化显示与分析, 支持策略运行监控和手工下单干预

示例程序增加随机森林(random_forest)策略

