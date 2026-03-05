# Rq_Dev - Fixed Income

**Pages:** 2

---

## 

**URL:** https://www.ricequant.com/doc/rqdata/python/convertible-mod

**Contents:**
- 可转债行情数据说明 ​
- 可转债基础信息，历史日行情、分钟线 ​
  - convertible.all_instruments - 获取所有可转债合约 ​
    - 参数 ​
    - 返回 ​
    - 范例 ​
  - convertible.instruments - 获取可转债合约基础信息 ​
    - 参数 ​
    - 返回 ​
      - 可转债 Instrument 对象 ​

可获取可转债合约的日行情、分钟行情、tick 行情数据，具体调用方式请参考 API-get_price.

获取所有可转债基础信息,传入日期可筛选该日上市状态合约列表

一个 instrument 对象，或一个 instrument list

其中参数 optiontype 可以支持输入 _int 类型 1~7 ，代表含义参考下表。默认返回全部类型

option() 方法返回结构为 pandas DataFrame，字段说明如下：

获取可转债合约一段时期的转股价变动。信息来源为交易所的可转债转股统计公告。

判断某只可转债或列表在一段时间内是否全天停牌。若在查询期间内转债尚未上市，或已退市，函数则报错提示；若开始日期早于转债上市日期，则以转债上市日期作为开始日期

获取某个日期转债所属的行业分类，转债行业分类即为对应正股上市公司行业分类

通过传入行业名称、行业指数代码或者行业代号，拿到指定行业的转债列表

获取可转债应计利息数据，应计利息从转债起息日起算

查询可转债赎回提示性公告数据，包含赎回和不赎回的信息

**Examples:**

Example 1 (rust):
```rust
convertible.all_instruments(date=None, market='cn')
```

Example 2 (json):
```json
[In]convertible.all_instruments()
[Out]
  order_book_id  symbol  full_name  exchange  bond_type  trade_type  value_date  maturity_date  par_value  coupon_rate  ...  coupon_method    total_issue_size
0  100001.XSHG  南化转债  南宁化工股份有限公司可转换公司债券  XSHG  convertible  clean_price  1998-08-03  2003-08-03  100.0  1.00  ...  stepup_rate    1.500000e+08
1  100009.XSHG  机场转债  上海国际机场股份有限公司可转换公司债券  XSHG  convertible  clean_price  2000-02-25  2005-02-25  100.0  0.80  ...  fixed_rate    1.350000e+09
2  100016.XSHG  民生转债  中国民生银行股份有限公司可转换公司债券  XSHG  convertible  clean_price  2003-02-27  2008-02-27  100.0  1.50  ...  fixed_rate    4.000000e+09
...
```

Example 3 (unknown):
```unknown
convertible.instruments(order_book_ids, market='cn')
```

Example 4 (unknown):
```unknown
convertible.instruments(order_book_ids).coupon_rate_table()
```

---

## 

**URL:** https://www.ricequant.com/doc/rqdata/python/repo

**Contents:**
- 国债回购基础信息及行情数据说明 ​
- 银行间同业拆放利率 ​
  - get_interbank_offered_rate - 获取上海银行间同业拆放利率数据 ​
    - 参数 ​
    - 返回 ​
    - 范例 ​

可获取深交所、上交所上市的各个期限的国债回购的日行情、分钟行情、tick 行情数据，具体调用方式请参考：API-get_price.

**Examples:**

Example 1 (rust):
```rust
rqdatac.get_interbank_offered_rate(start_date=None, end_date=None, fields=None, source='Shibor')
```

Example 2 (json):
```json
[In]
rqdatac.get_interbank_offered_rate(20230501,20230530)
[Out]
               ON     1W     2W     1M     3M     6M     9M     1Y
date
2023-05-04  1.658  1.963  1.979  2.294  2.414  2.501  2.582  2.636
2023-05-05  1.234  1.815  1.920  2.277  2.401  2.491  2.564  2.618
2023-05-06  1.095  1.745  1.764  2.259  2.393  2.481  2.548  2.603
2023-05-08  1.408  1.795  1.793  2.248  2.386  2.470  2.540  2.596
2023-05-09  1.239  1.825  1.813  2.235  2.377  2.462  2.529  2.583
2023-05-10  1.110  1.902  1.861  2.228  2.363  2.452  2.521  2.569
2023-05-11  1.102  1.803  1.818  2.212  2.349  2.439  2.502  2.548
2023-05-12  1.320  1.829  1.846  2.199  2.325  2.427  2.487  2.524
2023-05-15  1.523  1.874  1.909  2.189  2.311  2.419  2.474  2.508
2023-05-16  1.476  1.766  1.822  2.180  2.294  2.410  2.462  2.496
2023-05-17  1.540  1.844  1.870  2.160  2.288  2.403  2.454  2.490
2023-05-18  1.463  1.760  1.948  2.136  2.278  2.392  2.447  2.490
2023-05-19  1.408  1.910  2.062  2.131  2.269  2.390  2.445  2.489
2023-05-22  1.276  1.897  2.100  2.124  2.262  2.379  2.442  2.489
2023-05-23  1.224  1.824  2.065  2.117  2.255  2.372  2.438  2.484
2023-05-24  1.271  1.762  2.050  2.113  2.252  2.366  2.438  2.481
2023-05-25  1.578  1.908  2.050  2.111  2.243  2.364  2.436  2.478
2023-05-26  1.438  1.992  2.096  2.108  2.238  2.358  2.434  2.478
2023-05-29  1.370  1.937  2.051  2.104  2.233  2.355  2.425  2.474
```

---
