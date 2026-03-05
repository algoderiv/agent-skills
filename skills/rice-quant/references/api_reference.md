# Rq_Dev - Api Reference

**Pages:** 1

---

## 

**URL:** https://www.ricequant.com/doc/rqdata/python/generic-api

**Contents:**
- 行情、交易日及合约信息 ​
  - all_instruments - 获取所有合约基础信息 ​
    - 参数 ​
      - 其中 type 参数传入的合约类型和对应的解释如下： ​
    - 返回 ​
    - 范例 ​
  - instruments - 获取合约详细信息 ​
    - 参数 ​
    - 返回 ​
      - 股票，ETF，指数 Instrument 对象 ​

获取某个国家市场的所有合约信息。使用者可以通过这一方法很快地对合约信息有一个快速了解。 可传入date筛选指定日期可交易的合约，返回的 instrument 数据为合约的最新情况。

pandas DataFrame - 所有合约的基本信息。

详细字段注释请参考 instruments 返回字段说明

获取某个国家市场内一个或多个合约最新的详细信息。目前仅支持中国市场。

目前系统并不支持跨市场的同时调用，传入的 order_book_id list 必须属于同一国家市场，不能混合着中美两个国家市场的 order_book_id。

一个 instrument 对象，或一个 instrument list。

默认返回合约上市距离当前日期的天数。date 支持 str, 如果合约首次上市交易，天数为 0；如果合约尚未上市或已经退市，则天数值为-1

例如，instruments('IF1608').tick_size()获取的就是股指期货的最小价格变动单位，为 0.2，即“一跳”的水平。

将交易所和其他平台的股票代码转换成米筐的标准合约代码，目前仅支持 A 股、期货和期权代码转换。 例如, 支持转换类型包括 000001.SZ, 000001SZ, SZ000001 转换为 000001.XSHE

获取指定合约或合约列表的行情数据（包含起止日期，周线、日线、分钟线和tick）。目前支持中国市场的股票、期货、期权、可转债、ETF、常见指数和上金所现货的行情数据，如黄金、铂金和白银产品。支持历史和实时。

1、 周线数据目前只支持'1w',依据日线数据进行合成，例如股票周线的前复权数据使用前复权日线数据进行合成，股票周线的不复权数据使用不复权的日线数据合成。 2、如需大量获取分钟或 tick 数据，建议以单只合约为单位，并设置长时段获取以提高效率。 3、time_slice参数是先获取 start_date、end_date 区间内所有数据再进行切分，请注意流量使用。

获取科创板、创业板等股票合约盘后固定价格交易信息，可获取历史和实时

获取当日给定合约的 level1 快照行情，无法获取历史，获取历史请使用 get_price

查询时间在交易日 T 日 7.30 pm 之前，返回 T 日的 tick 数据，查询时点在 7.30pm 之后，返回交易日 T+1 日的 tick 数据。

获取当前交易日的股票、期货、期权、ETF、常见指数和上金所现货等合约的 level1 快照行情，无法获取历史。

start_dt 和 end_dt 需同时传入或同时不传入，当不传入 start_dt,end_dt 参数时，查询时间在交易日 T 日 7.30 pm 之前，返回 T 日的 tick 数据，查询时点在 7.30pm 之后，返回交易日 T+1 日的 tick 数据

获取盘前集合竞价结束，交易所撮合后的 level 1 快照。

获取给定合约当日最新的1分钟 k 线，无法获取历史。支持股票。

目前只支持股票、期货、指数、可转债。历史涨跌幅基于后复权价格。

获取某一合约当前的 LEVEL1 行情快照，支持集合竞价数据获取。

Tick 对象 或者一个 Tick list

获取某个国家市场的交易日列表（起止日期加入判断）。

datetime.date list - 交易日期列表

获取最近一个交易日（若当天为交易日，则返回当天；若当天为节假日，则返回上一个交易日）

默认获取当前点国内市场合约字符串形式的连续竞价交易时间段。

该 API 即将退役，可使用 get_trading_periods

默认获取当前点国内市场合约字符串形式的连续竞价交易时间段。

获取某个国家市场在一段时间内收益率曲线水平（包含起止日期）。目前仅支持中国市场。

数据为 2002 年至今的中债国债收益率曲线，来源于中央国债登记结算有限责任公司。

pandas DataFrame - 查询时间段内无风险收益率曲线

获取最近一个期货交易日（从夜盘的集合竞价开始算起，作为新的交易日；若当天为节假日，则返回下一个交易日）

获取成交量加权平均价格，可获取历史和实时。支持品种：股票、期货、期权、ETF、可转债。

获取 000001.XSHE 在 2024-01-01~02-01 的日级别 vwap 数据

获取 IF2403 在 2024-02-01 的 1m 级别 vwap 数据

考虑到实时行情中，用户不太方便通过主动轮询 API 去获取合约最新不间断的实时行情，米筐开发提供了 python sdk 和 websocket 网络接口，用以支持用户获取实时行情推送数据，具体说明如下：

1、 驱动实盘交易或者模拟交易 2、 若客户已有实时行情，米筐可以作为备份

1、 推送会比拉取型 API 返回实时行情更及时，效率更高 2、 提供 python sdk 和 websocket 网络接口，用户可以使用任意语言接入，语言中性 3、 基于 ricequant 的数据能力和 rqdata 的基础设施，数据准确快速，可靠性高

通过简单的一行代码从 RQData 引入 LiveMarketDataClient ，即可实现实时行情数据的推送。分别提供阻塞和不阻塞的调用方式，具体请参考范例。

**Examples:**

Example 1 (rust):
```rust
all_instruments(type=None, date=None, market='cn')
```

Example 2 (json):
```json
[In]all_instruments()
[Out]
    abbrev_symbol order_book_id  sector_code symbol
0 XJDQ 000400.XSHE   Industrials     许继电气
1 HXN     002582.XSHE   ConsumerStaples 好想你
2 NFGF 300004.XSHE   Industrials     南风股份
3 FLYY 002357.XSHE   Industrials     富临运业
...
```

Example 3 (json):
```json
[In]all_instruments(type='LOF')
[Out]
    abbrev_symbol order_book_id product sector_code  symbol
0 CYGA 150303.XSHE null null 华安创业板50A
1 JY500A 150088.XSHE null null 金鹰500A
2 TD500A 150053.XSHE null null 泰达稳健
3 HS500A 150110.XSHE null null 华商500A
4 QSAJ 150235.XSHE null null 鹏华证券A
...
```

Example 4 (json):
```json
[In]all_instruments(type='Future')
[Out]
 abbrev_symbol order_book_id product sector_code symbol
0 MH0610 CF0610 Commodity null 棉花0610
1 LD0209 GN0209 Commodity null 绿豆0209
...
3615 HS1301 IF1301 Index null 沪深1301
...
```

---
