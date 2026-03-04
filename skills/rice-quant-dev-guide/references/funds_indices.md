# Rq_Dev - Funds Indices

**Pages:** 2

---

## 

**URL:** https://www.ricequant.com/doc/rqdata/python/fund-mod

**Contents:**
- 行情数据 ​
- 基金基础数据，净值数据，以及衍生数据 ​
  - fund.instruments - 获取基金基础信息 ​
    - 参数 ​
    - 返回 ​
    - 范例 ​
  - fund.all_instruments - 获取所有公募基金信息 ​
    - 参数 ​
    - 返回 ​
    - 范例 ​

仅有场内基金提供五档行情，日行情、分钟行情、tick 行情数据，具体调用方式请参考 API-get_price。场外基金无该数据

以下基金 API 中，需先单独安装 rqdatac_fund，导入后进行调用。涉及 order_book_ids 参数无需填后缀，比如 000001、159001。

基金 instrument 对象或 instrument list

从指定日期回溯，获取最近的基金债券投资信用评级信息。

标准版涵盖的衍生指标及频率如下，字段的组成方式为 “支持的频率_字段”, 如 “日度累计收益” 字段名为 'daily_return'，货币基金仅支持展示下面部分衍生指标数据。

标准版涵盖的衍生指标及频率如下，字段的组成方式为 “支持的频率_字段”, 如 “日度累计收益” 字段名为 'daily_return'，货币基金仅支持展示下面部分衍生指标数据。

从指定日期回溯，获取最近的基金债券组合结构信息。

标准版涵盖的衍生指标及频率如下，字段的组成方式为 “支持的频率_字段”, 如 “日度累计收益” 字段名为 'daily_return'

**Examples:**

Example 1 (unknown):
```unknown
fund.instruments(order_book_ids, market='cn')
```

Example 2 (unknown):
```unknown
In [20]: fund.instruments('000014')
Out[20]: Instrument(order_book_id='000014', benchmark='100.0％×一年定期存款收益率(税后)加1.2%', issuer='华夏基金管理有 限公司', establishment_date='2013-03-19', listed_date='2013-03-19', de_listed_date='0000-00-00', stop_date='0000-00-00', symbol='华夏聚利债券', fund_manager='何家琪', fund_type='Bond', accrued_daily=False, latest_size=667046240.1, type='PublicFund', transition_time=1, exchange='', amc_id='41511', amc='华夏基金管理有限公司', abbrev_symbol='华夏聚利',..., min_investment=1.0, object='在控制风险的前提下，追求较高的当期收入和总回报。', trustee=3037, redeem_amount_days=7, confirmation_days=1, round_lot=1.0)
```

Example 3 (unknown):
```unknown
In [10]: fund.instruments('000014_CH0')
Out[7]: Instrument(order_book_id='000014_CH0', benchmark='100.0％×一年定期存款收益率(税后)加1.2%', issuer='华夏基金管理有限公司', establishment_date='2013-03-19', listed_date='2013-03-19', symbol='华夏一年债', accrued_daily=False, fund_type='Bond', transition_time=0, de_listed_date='2014-03-19', stop_date='2014-03-19', latest_size=4016611053.94, type='PublicFund', exchange='', amc_id='41511', amc='华夏基金管理有限公司', round_lot=1.0)
```

Example 4 (rust):
```rust
fund.all_instruments(date=None, market='cn')
```

---

## 

**URL:** https://www.ricequant.com/doc/rqdata/python/indices-mod

**Contents:**
- 指数、场内基金行情数据说明 ​
- 指数相关数据约定 ​
- 指数列表 ​
- index_indicator - 获取指数每日估值指标 ​
    - 参数 ​
    - 返回 ​
    - 范例 ​
- index_components - 获取指数成分股列表 ​
    - 参数 ​
    - 返回 ​

可获取指数、场内基金合约的日行情、分钟行情、tick 行情数据，具体调用方式请参考 API-get_price。

米筐 399001.XSHE、399006.XSHE、399005.XSHE 成交量和成交额的计算范围为各自的成分股，和深交所公布的上述指数的成交量和成交额存在差异。

深交所公布的行情中深证综指（399106.XSHE）和深证成指（399001.XSHE），创业板综（399102.XSHE）和创业板指（399006.XSHE），中小板综（399010.XSHE）和中小板指（399005.XSHE）的成交量和成交额相同。

目前所支持的指数列表可以参考下方表格 个别 API 支持范围有所区别，以 API 标注为准

获取指数每日估值指标。目前仅支持部分市值加权指数的市盈率、市净率、股息率、总市值、流通市值和自由流通市值，支持的市值指数列表可点这里下载

获取某一指数的构成列表，也支持指数的历史构成查询。

构成该指数股票的 order_book_id list

-获取 000300.XSHG 在 2019-07-01 至 2019-07-06 的成分股列表

获取某一指数的历史构成以及权重。注意，该数据为月度更新。

pandas Series，每只股票在指数中的构成权重。

获取某一指数的历史构成以及日度权重。 注意：该数据为基于上月月末成分股自算权重，在指数成分股定期调整实施期间 (为公告日次一交易日起至当月最后一个交易日，以中证系列指数为例，调整于每年 6 月和 12 月第二个星期五后的第一个交易日生效)，结合 ETF 申赎清单进行动态权重调整，确保与指数公司的实际调仓操作保持一致。目前仅支持上证 50、沪深 300、中证 A500、中证 500、中证 800、中证 1000、中证 2000、科创 50、中证红利指数。

pandas Series，每只股票在指数中的构成权重。

**Examples:**

Example 1 (unknown):
```unknown
index_indicator(order_book_ids,start_date,end_date,fields)
```

Example 2 (json):
```json
[In]
index_indicator(['000016.XSHG','000300.XSHG'],start_date=20170402,end_date=20170408)
[Out]
		                pb_lf	pb_lyr	pb_ttm	pe_lyr	pe_ttm  total_market_value  circulation_market_value   dividend_yield_ttm   free_circulation_market_value
order_book_id	trade_date
000016.XSHG	2017-04-05	1.183987	1.196851	1.225406	10.854710	10.778091   15260989825748.05   12191663908025.05   0.026319771992644583    3486627097387.498
                2017-04-06	1.183772	1.195776	1.225508	10.842157	10.779690   15284371041088.639   12215144730131.48   0.02625281789304445    3493549881437.2397
                2017-04-07	1.185690	1.197714	1.227494	10.859727	10.797159   15308846903193.26   12236073966848.059   0.026275557023951103   3490787614421.26
000300.XSHG	2017-04-05	1.503460	1.533702	1.563953	13.899681	13.773018   29550781224331.355   22073998982237.97   0.020803171426540215    8478206547292.009
                2017-04-06	1.505061	1.534583	1.565892	13.904718	13.790629   29612483589821.27   22135688921223.97   0.020741058850743878    8501490339986.75
                2017-04-07	1.508388	1.537986	1.569359	13.935358	13.820774   29678274705464.06   22185711852084.93   0.020697328757145923    8510598400046.74
```

Example 3 (rust):
```rust
index_components(order_book_id, date=None, start_date, end_date, market='cn',return_create_tm=False)
```

Example 4 (json):
```json
[In]
index_components('000001.XSHG')
[Out]
['600000.XSHG',
 '600004.XSHG',
 ...]
```

---
