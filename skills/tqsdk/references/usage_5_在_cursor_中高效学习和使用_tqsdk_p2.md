Example 2 (python):
```python
from tqsdk import TqApi, TqAuth
api = TqApi(auth=TqAuth("快期账户", "账户密码"))

ls = api.query_options("SHFE.au2012")
print(ls)  # 标的为 "SHFE.au2012" 的所有期权

ls = api.query_options("SHFE.au2012", option_class="PUT")
print(ls)  # 标的为 "SHFE.au2012" 的看跌期权

ls = api.query_options("SHFE.au2012", option_class="PUT", expired=False)
print(ls)  # 标的为 "SHFE.au2012" 的看跌期权, 未下市的

ls = api.query_options("SHFE.au2012", strike_price=340)
print(ls)  # 标的为 "SHFE.au2012" 、行权价为 340 的期权

ls = api.query_options("SSE.510300")
print(ls)  # 中金所沪深300股指期权

ls = api.query_options("SSE.510300")
print(ls)  # 上交所沪深300etf期权

ls = api.query_options("SSE.510300", exercise_year=2020, exercise_month=12)
print(ls)  # 上交所沪深300etf期权, 限制条件 2020 年 12 月份行权
```

---


## 在 Jupyter Notebook 中使用 TqSdk

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/jupyter.html

**Contents:**
- 在 Jupyter Notebook 中使用 TqSdk
- 安装 Jupyter Notebook
- 安装 TqSdk
- 在 Jupyter Notebook 中使用 TqSdk
- 注意事项

本文档将介绍如何在 Jupyter Notebook 中使用 TqSdk。

当您的主要是希望使用 TqSdk 来进行行情分析和研究，并不涉及到交易时使用 Jupyter Notebook 可以带来一些潜在优势

1. 交互式编程 Jupyter Notebook 提供了一个交互式的环境，可以逐步执行代码块并立即查看输出结果。这种特性特别适合数据分析和探索性编程，使得用户可以实时查看数据处理和可视化的效果。

2. 内嵌可视化 Jupyter Notebook 支持在单元格中嵌入图表和图像，使得数据可视化变得非常方便。用户可以在一个文档中同时编写代码、生成图表和撰写分析报告，无需切换到其他工具或窗口。

Jupyter Notebook 是一个开源项目，能够在交互式编程环境中提供丰富的可视化表达，可以创建和共享代码和文档。

可以使用以下命令安装 Jupyter Notebook：

`bash pip install jupyter `

更多 Jupyter Notebook 安装文档请参考

Jupyter Notebook 安装文档

不建议用户在 jupyter 里使用 tqsdk 的交易功能，因为 TqSdk 行情只有在调用 wait_update() 之后才会更新，但是在 jupyter 交互运行的环境中，无法及时调用 wait_update()，可能会导致行情延时

不能在 jupyter 中异步的使用 tqsdk，jupyter 只能用 TqSdk 的同步代码的写法

在 jupyter 中使用 wait_update() 时，建议增加 deadline 参数在函数里，避免长时间的阻塞

© 版权所有 2018-2026, TianQin。

---


## 外盘行情

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/kqd_symbol.html

**Contents:**
- 外盘行情
- 板块介绍
- 合约展示

为了满足投资者对全球市场信息的需求，提供更全面、准确的投资决策支持，快期专业版 / 天勤量化 上线了外盘行情（延时15分钟）

在快期专业版中的添加方式：【添加板块】 - 【系统报价表】 - 【外盘行情(延时)】

在 TqSdk 中，您可以使用 query_quotes 函数来获取对应的外盘合约列表，然后再安装自己的需求获取对应合约

目前提供了纽约NYMEX、纽约COMEX、芝加哥CBOT、芝加哥CME、芝加哥CFE、美国NYBOT、欧洲EUREX、伦敦LME、香港HKFE、新加坡SGX 等多家交易所的主连合约行情

© 版权所有 2018-2026, TianQin。

---

