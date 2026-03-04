# Rq_Dev - Stocks

**Pages:** 2

---

## 

**URL:** https://www.ricequant.com/doc/rqdata/python/stock-hk

**Contents:**
- 港交所股票合约基础信息 ​
  - all_instruments - 获取所有合约基础信息 ​
    - 参数 ​
      - 其中 type 参数传入的合约类型和对应的解释如下： ​
    - 返回 ​
    - 范例 ​
  - instruments - 获取合约详细信息 ​
    - 参数 ​
    - 返回 ​
      - 股票 Instrument 对象 ​

API 传参 market='hk' 即可获取港交所合约数据

获取港交所的所有股票合约信息。使用者可以通过这一方法很快地对合约信息有一个快速了解. 可传入date筛选指定日期可交易的合约，返回的 instrument 数据为合约的最新情况。

pandas DataFrame - 所有合约的基本信息。 详细字段注释请参考 instruments 返回字段说明

获取港交所某一个或多个股票最新的详细信息。

一个 instrument 对象，或一个 instrument list。

获取某只股票或股票列表在一段时间内的复权因子（包含起止日期，以除权除息日为查询基准）。如未指定日期，则默认所有。

pandas DataFrame - 包含了复权因子的日期和对应的各项数值

获取股票或股票列表在一段时间内的流通情况（包含起止日期）。

通过传入行业名称、行业指数代码或者行业代号，拿到指定行业的股票列表

通过传入行业名称、行业指数代码或者行业代号，拿到指定行业的股票纳入剔除日期

通过 order_book_id 传入，拿到某个日期的该股票指定的行业分类

通过传入分类依据，获得对应的一二三级行业代码和名称。

获取某只股票或股票列表在一段时间内的现金分红情况（包含起止日期，以分红宣布日为查询基准）。如未指定日期，则默认所有。

1、rqdatac 3.4.3 起，expect_df 默认参数值由 False 更改为True 2、market='hk' 不支持 expect_df = False

单只股票 pandas single-index DataFrame - 查询时间段内的某个股票的现金分红数据

一组股票 pandas multi-index DataFrame - 查询时间段内的一组股票的现金分红数据

请先单独安装 rqdatac_hk，导入后使用

某一天港股通成分股的 order_book_id list

获取指定港股合约或合约列表的历史数据（包含起止日期，周线、日线、分钟线或 tick）。

周线数据目前只支持'1w',依据日线数据进行合成，例如股票周线的前复权数据使用前复权日线数据进行合成，股票周线的不复权数据使用不复权的日线数据合成。

以给定一个报告期回溯的方式获取季度基础财务数据（三大表），即利润表，资产负债表，现金流量表。

该 API 返回的因子值均为做了港币汇率转换后的值，除了货币单位为港币的合约，其余并非财报披露的原始值。若需获取原始值，请参考hk.get_detailed_financial_items-params

请先单独安装 rqdatac_hk，导入后使用

默认返回指定因子上一个交易日的值，目前港股因子仅支持获取市值和流通市值

请先单独安装 rqdatac_hk，导入后使用

**Examples:**

Example 1 (rust):
```rust
rqdatac.all_instruments(type=None, market='hk', date=None)
```

Example 2 (json):
```json
[In]rqdatac.all_instruments('CS',market='hk')
[Out]
     order_book_id      eng_symbol abbrev_symbol board_type    symbol listed_date de_listed_date  status  round_lot a_share_id exchange type trading_code      unique_id stock_connect
0       00001.XHKG    CKH HOLDINGS            CH  MainBoard        长和  1972-11-01     0000-00-00  Active      500.0       None     XHKG   CS        00001  00001_01.XHKG     sh_and_sz
1       00002.XHKG    CLP HOLDINGS          ZDKG  MainBoard      中电控股  1980-01-02     0000-00-00  Active      500.0       None     XHKG   CS        00002  00002_01.XHKG     sh_and_sz
2       00003.XHKG  HK & CHINA GAS        XGZHMQ  MainBoard    香港中华煤气  1960-04-11     0000-00-00  Active     1000.0       None     XHKG   CS        00003  00003_01.XHKG     sh_and_sz
3       00004.XHKG  WHARF HOLDINGS         JLCJT  MainBoard     九龙仓集团  1921-01-01     0000-00-00  Active     1000.0       None     XHKG   CS        00004  00004_01.XHKG     sh_and_sz
4       00005.XHKG   HSBC HOLDINGS          HFKG  MainBoard      汇丰控股  1980-01-02     0000-00-00  Active      400.0       None     XHKG   CS        00005  00005_01.XHKG     sh_and_sz
...            ...             ...           ...        ...       ...         ...            ...     ...        ...        ...      ...  ...          ...            ...           ...
3309    83690.XHKG      MEITUAN-WR          MTWR  MainBoard     美团-WR  2023-06-19     0000-00-00  Active      100.0       None     XHKG   CS        83690  83690_01.XHKG
3310    86618.XHKG     JD HEALTH-R         JDJKR  MainBoard    京东健康-R  2023-06-19     0000-00-00  Active       50.0       None     XHKG   CS        86618  86618_01.XHKG
3311    89618.XHKG          JD-SWR       JDJTSWR  MainBoard  京东集团-SWR  2023-06-19     0000-00-00  Active       50.0       None     XHKG   CS        89618  89618_01.XHKG
3312    89888.XHKG        BIDU-SWR       BDJTSWR  MainBoard  百度集团-SWR  2023-06-19     0000-00-00  Active       50.0       None     XHKG   CS        89888  89888_01.XHKG
3313    89988.XHKG         BABA-WR        ALBBWR  MainBoard   阿里巴巴-WR  2023-06-19     0000-00-00  Active      100.0       None     XHKG   CS        89988  89988_01.XHKG

[3314 rows x 15 columns]
```

Example 3 (unknown):
```unknown
rqdatac.instruments(order_book_ids, market='hk')
```

Example 4 (unknown):
```unknown
In [5]: rqdatac.instruments('00013.XHKG',market='hk')
Out[5]:
Instrument(order_book_id='00013.XHKG', eng_symbol='HUTCHMED', abbrev_symbol='HHYY', board_type='MainBoard', symbol='和黄医药', listed_date='2021-06-30', de_listed_date='0000-00-00', status='Active', round_lot=500.0, exchange='XHKG', type='CS', trading_code='00013', unique_id='00013_02.XHKG', stock_connect='sh_and_sz')
```

---

## 

**URL:** https://www.ricequant.com/doc/rqdata/python/stock-mod

**Contents:**
- 股票行情数据说明 ​
- A 股财务数据 ​
  - get_pit_financials_ex - 查询季度财务信息(point-in-time 形式) ​
    - 参数 ​
    - 返回 ​
      - 利润表 ​
        - 资产负债表 ​
        - 现金流量表 ​
    - 范例 ​
  - current_performance - 查询财务快报数据 ​

可获取股票合约的日行情、分钟行情、tick 行情数据，具体调用方式请参考 API-get_price.

米筐科技（RiceQuant）基于量化交易的实际投资研究需求，对财务数据建立了一套完整的上市公司财务数据处理流程。米筐科技的财务数据有如下优点：

以下表格的字段全部基于新会计准则并直接采集于三大财报（资产负债表、利润表和现金流量表）。财报本身的来源常见有交易所的定期公告中的常规季报和年报、临时公告中的业绩快报和比较式财务报告以及招股说明书等。该部分数据一定来源于完整的财报，所以一般意义上的业绩快报，业绩预增报告中的数据并不会出现。

以给定一个报告期回溯的方式获取季度基础财务数据（三大表），即利润表，资产负债表，现金流量表。

默认返回给定的 order_book_id 当前最近一期的快报数据。

默认返回给定的 order_book_ids 当前最近一期的业绩预告数据。 业绩预告主要用来调取公司对即将到来的财务季度的业绩预期的信息。有时同一个财务季度会有多条记录，分别是季度预期和累计预期（即本年至今）。

除提供三大表基础财务数据外，我们还对其进行了单季度处理：

在发布财务报告以后，上市公司可能会对数据进行修正。因此，在进行指标计算的时候，需要考虑当前时间点所能取到的最新数据，以避免未来数据的问题。该处理称为 Point-in-Time（PIT）处理。例如，考虑以下例子：

TTM 是 Trailing Twelve Months 的简称，会使用过去 4 个季度的滚动财务数据进行计算，可避免某一期财报数据的偶然性。（对于来自利润表和现金流量表的数据 TTM 为滚动加和，来自资产负债表的数据 TTM 为滚动求平均）。

对于利润表中的基本每股收益，目前米筐的单季度处理以及 TTM 处理都直接采用的每股收益指标相减的方式。

LYR 是 Last Year Ratio 的简称，会使用最近一期年报的数据。

估值、经营、现金流、财务和成长衍生指标，经 LF、LYR、TTM 处理后的命名规则和三大基础会计科目经相同方式处理后的命名规则存在区别

默认返回指定因子上一个交易日的值。包括财务衍生指标因子、技术指标因子、alpha101 因子 等。

为方便阅读，可点这里下载 Excel 版本的指标列表

为方便阅读，可点这里下载 Excel 版本的指标列表

为方便阅读，可点这里下载 Excel 版本的指标列表

为方便阅读，可点这里下载 Excel 版本的指标列表

为方便阅读，可点这里下载 Excel 版本的指标列表

默认返回全部因子，可选择返回不同类型的所有因子字段名称列表。

查询股票因代码变更或并购等情况更换了股票代码的信息

目前支持的 code 板块分类如下，其取值参考自 MSCI 发布的全球行业标准分类:

属于该板块的order_book_id list.

我们目前使用的行业分类来自于中国国家统计局的国民经济行业分类，可以使用这里的任何一个行业代码来调用行业的股票列表：

属于该行业的order_book_id list.

通过传入分类依据，获得对应的一二三级行业代码和名称。

通过传入行业名称、行业指数代码或者行业代号，拿到指定行业的股票列表

通过传入行业名称、行业指数代码或者行业代号，拿到指定行业的股票纳入剔除日期

通过 order_book_id 传入，拿到某个日期的该股票指定的行业分类

获取某只股票或股票列表在一段时间内的分红情况（包含起止日期）。如未指定日期，则默认所有。目前仅支持中国市场。

单只股票 pandas single-index DataFrame - 查询时间段内的某个股票的分红数据

一组股票 pandas multi-index DataFrame - 查询时间段内的一组股票的分红数据

获取某只股票或股票列表在一段时间内的现金分红情况（包含起止日期，以分红宣布日为查询基准）。如未指定日期，则默认所有。

1、rqdatac 3.4.3 起，expect_df 默认参数值由 False 更改为True 2、market='hk' 不支持 expect_df = False

单只股票 pandas single-index DataFrame - 查询时间段内的某个股票的现金分红数据

一组股票 pandas multi-index DataFrame - 查询时间段内的一组股票的现金分红数据

获取股票历年分红总额数据。目前仅支持中国市场。

单只股票 pandas single-index DataFrame - 查询时间段内的某个股票的分红总额数据

一组股票 pandas multi-index DataFrame - 查询时间段内的一组股票的分红总额数据

获取某只股票或股票列表在一段时间内的拆分情况（包含起止日期，以股权登记日为查询基准），如未指定日期，则默认所有。目前仅支持中国市场。

单只股票 pandas single-index DataFrame - 查询时间段内的某个股票的拆分数据

一组股票 pandas multi-index DataFrame - 查询时间段内的一组股票的拆分数据

例如：每 10 股转增 2 股，则 split_coefficient_from = 10, split_coefficient_to = 12.

获取某只股票或股票列表在一段时间内的复权因子（包含起止日期，以除权除息日为查询基准）。如未指定日期，则默认所有。

pandas dataframe - 包含了复权因子的日期和对应的各项数值

判断某只股票或股票列表在一段时间（包含起止日期）是否全天停牌。

pandas DataFrame 如果在查询期间内股票尚未上市，或已经退市，则函数返回 None；如果开始日期早于股票上市日期，则以股票上市日期作为开始日期。

判断一只或多只股票在一段时间（包含起止日期）内是否为 ST 股。

pandas DataFrame - 查询时间段内是否为 ST 股的查询结果

获取股票或者股票列表在一段时间内的股本数据（包含起止日期）。

获取 A 股主要股东构成及持流通 A 股数量比例、持股性质等信息，通常为前十位。

获取股票在一段时间内的定向增发信息（包含起止日期，以公告发布日为查询基准）。如未指定日期，则默认所有。

获取股票在一段时间内的配股信息（包含起止日期，以首次信息发布日为查询基准）。如未指定日期，则默认所有。

获取某只股票或股票列表的资金流入流出信息。目前仅支持中国市场。

pandas multi-index DataFrame

另，连续竞价撮合成当天第一笔交易的，成交价>=上一笔卖询价，为主动买，否则为主动卖。

该 API 基于 level 1 数据合成，所以暂且不对资金量（大中小）作主观分类。

获取当日某只股票或股票列表的最近一分钟资金流入流出信息，无法获取历史。该 API 基于 level 1 数据合成。

获取融资融券信息。包括深证融资融券数据以及上证融资融券数据情况。既包括个股数据，也包括市场整体数据。需要注意，融资融券的开始日期为 2010 年 3 月 31 日;根据交易所的原始数据，上交所个股跟整个市场的输出信息列表不一致，个股没有融券余量金额跟融资融券余额两项, 而深交所个股跟整个市场的输出信息列表一致。

list 证券列表 - 如果所查询日期没有融资融券股票列表，则返回空 list

获取融资融券可充抵保证金证券信息。 （不含地方债、公司债）

list 证券列表 - 如果所查询日期没有融资融券可充抵保证金证券信息，则返回空 list

获取股票在一段时间内的在香港上市交易的持股情况。

**Examples:**

Example 1 (rust):
```rust
get_pit_financials_ex(order_book_ids, fields, start_quarter, end_quarter, date=None, statements='latest', market='cn')
```

Example 2 (json):
```json
[In]
get_pit_financials_ex(fields=['revenue','net_profit'], start_quarter='2018q2', end_quarter='2018q3',order_book_ids=['000001.XSHE','000048.XSHE'])
[Out]
                  info_date revenue if_adjusted net_profit
order_book_id quarter
000001.XSHE 2018q2 2019-08-08 5.724100e+10 1 1.337200e+10
            2018q3 2019-10-22 8.666400e+10 1 2.045600e+10
000048.XSHE 2018q2 2019-08-31 7.362684e+08 1 -3.527276e+07
            2018q3 2019-10-31 1.216331e+09 1 -4.566952e+07
```

Example 3 (json):
```json
[In]
get_pit_financials_ex(fields=['revenue','net_profit'], start_quarter='2018q2', end_quarter='2018q2',order_book_ids=['000001.XSHE','000048.XSHE'],statements='all')
[Out]
                  info_date revenue if_adjusted net_profit
order_book_id quarter
000001.XSHE 2018q2 2018-08-16 5.724100e+10 0 1.337200e+10
            2018q2 2019-08-08 5.724100e+10 1 1.337200e+10
000048.XSHE 2018q2 2018-08-31 1.063670e+09 0 7.790205e+07
            2018q2 2018-10-31 1.060487e+09 0 7.880372e+07
            2018q2 2019-06-15 7.362684e+08 0 -3.527276e+07
            2018q2 2019-08-31 7.362684e+08 1 -3.527276e+07
```

Example 4 (json):
```json
[In]
get_pit_financials_ex(fields=['revenue','net_profit'], start_quarter='2018q2', end_quarter='2018q2',order_book_ids=['000001.XSHE','000048.XSHE'],statements='all',date='20190807')
[Out]
                  info_date revenue if_adjusted net_profit
order_book_id quarter
000001.XSHE 2018q2 2018-08-16 5.724100e+10 0 1.337200e+10
000048.XSHE 2018q2 2018-08-31 1.063670e+09 0 7.790205e+07
            2018q2 2018-10-31 1.060487e+09 0 7.880372e+07
            2018q2 2019-06-15 7.362684e+08 0 -3.527276e+07
```

---
