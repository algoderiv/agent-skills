# Wtpy-Zz - Handbook

**Pages:** 15

---

## 配置文件

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/

**Contents:**
- 配置文件

在WT中，大量的设置通过配置文件的形式实现，配置文件可以分为以下几类：

---

## 工具集

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E5%B7%A5%E5%85%B7%E9%9B%86/

**Contents:**
- 工具集

---

## WTPY

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/

**Contents:**
- WTPY

---

## 工具集

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/工具集/

**Contents:**
- 工具集

---

## 开发手册

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/

**Contents:**
- 开发手册

---

## HFT引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/交易引擎/2.HFT引擎/

**Contents:**
- HFT引擎

HFT引擎，也叫高频策略引擎，主要针对高频或者低延时策略，事件驱动，系统延迟在1-2微秒之间

---

## 数据结构

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/数据结构/

**Contents:**
- 数据结构

---

## 配置文件

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/配置文件/

**Contents:**
- 配置文件

在WT中，大量的设置通过配置文件的形式实现，配置文件可以分为以下几类：

---

## 开发手册

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/

**Contents:**
- 开发手册

---

## CTA引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/交易引擎/1.CTA引擎/

**Contents:**
- CTA引擎

CTA引擎，也叫同步策略引擎，一般适用于标的较少，计算逻辑较快的策略，事件+时间驱动。典型的应用场景包括单标的择时、中频以下的套利等。Demo中提供的DualThrust策略，单次重算平均耗时，Python实现版本约70多微秒，C++实现版本约4.5微秒。

---

## 数据结构

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/

**Contents:**
- 数据结构

---

## UFT引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/交易引擎/4.UFT引擎/

**Contents:**
- UFT引擎

也叫极速策略引擎，主要针对超高频或者超低延时策略，事件驱动，系统延迟在200纳秒之内

---

## 基础配置文件

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C/WTPY/%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6.html

**Contents:**
- 基础配置文件
- commodities.json
- stk_comms.json
- sopt_comms.json
- contracts.json
- fee.json
- fees_stk.json
- holidays.json
- hots.json
- seconds.json

基础配置文件可以在项目中的demo目录下获取common

品种信息表，记录不同交易所中不同品种的相关信息，基本格式如下：(由于json无法使用注释，因此这里采用python的格式写，如果要使用请把注释删除)

合约信息表，记录具体合约的相关信息，基本格式如下：

对于commodities.json 与 contracts.json文件，wt提供了ctp_loader工具帮助维护，其前往工具集目录下查看如何使用。stk_comms.json 与 sopt_comms.json需要自行配置。

可以使用hotpicker来更新hots.json与seconds.json

**Examples:**

Example 1 (unknown):
```unknown
{
    "CFFEX": {                      # 交易所
        "IC": {                     # 品种名
            "covermode": 0,         # 平仓类型 表示该品种支持的开平模式
                                    # 0=开平 1=开平昨平今 
                                    # 2=平未了结的  3=不区分开平
            "pricemode": 0,         # 价格模式 0=市价限价 1=仅限价 2=仅市价
            "category": 1,          # 分类，参考CTP
                                    # 0=股票 1=期货 2=期货期权 3=组合 4=即期
                                    # 5=期转现 6=现货期权(股指期权) 7=个股期权(ETF期权)
                                    # 20=数币现货 21=数币永续 22=数币期货 23=数币杠杆 24=数币期权
            "trademode": 0,         # 交易模式，0=多空都支持 1=只支持做多 2=只支持做多且T+1
            "precision": 1,         # 价格小数点位数
            "pricetick": 0.2,       # 最小价格变动单位
            "volscale": 200,        # 合约倍数
            "name": "中证",         # 名称
            "exchg": "CFFEX",       # 所属交易所
            "session": "SD0930",    # 交易时间，具体参考session配置文件
            "holiday": "CHINA"      # 节假日，具体参考holiday.json
        },
        "IF": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "trademode": 0,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 300,
            "name": "沪深",
            "exchg": "CFFEX",
            "session": "SD0930",
            "holiday": "CHINA"
        },
    }
    "CZCE": {
        "AP": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "trademode": 0,
            "precision": 0,
            "pricetick": 1.0,
            "volscale": 10,
            "name": "苹果",
            "exchg": "CZCE",
            "session": "FD0900",
            "holiday": "CHINA"
        },
    }
}
```

Example 2 (unknown):
```unknown
{
    "CFFEX": {                      # 交易所
        "IC": {                     # 品种名
            "covermode": 0,         # 平仓类型 表示该品种支持的开平模式
                                    # 0=开平 1=开平昨平今 
                                    # 2=平未了结的  3=不区分开平
            "pricemode": 0,         # 价格模式 0=市价限价 1=仅限价 2=仅市价
            "category": 1,          # 分类，参考CTP
                                    # 0=股票 1=期货 2=期货期权 3=组合 4=即期
                                    # 5=期转现 6=现货期权(股指期权) 7=个股期权(ETF期权)
                                    # 20=数币现货 21=数币永续 22=数币期货 23=数币杠杆 24=数币期权
            "trademode": 0,         # 交易模式，0=多空都支持 1=只支持做多 2=只支持做多且T+1
            "precision": 1,         # 价格小数点位数
            "pricetick": 0.2,       # 最小价格变动单位
            "volscale": 200,        # 合约倍数
            "name": "中证",         # 名称
            "exchg": "CFFEX",       # 所属交易所
            "session": "SD0930",    # 交易时间，具体参考session配置文件
            "holiday": "CHINA"      # 节假日，具体参考holiday.json
        },
        "IF": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "trademode": 0,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 300,
            "name": "沪深",
            "exchg": "CFFEX",
            "session": "SD0930",
            "holiday": "CHINA"
        },
    }
    "CZCE": {
        "AP": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "trademode": 0,
            "precision": 0,
            "pricetick": 1.0,
            "volscale": 10,
            "name": "苹果",
            "exchg": "CZCE",
            "session": "FD0900",
            "holiday": "CHINA"
        },
    }
}
```

Example 3 (unknown):
```unknown
{
    "SSE" : 
    {
        "STK" : 
        {
            "category" : 0,
            "covermode" : 0,
            "exchg" : "SSE",
            "holiday" : "CHINA",
            "name" : "上证股票",
            "precision" : 2,
            "pricemode" : 1,
            "pricetick" : 0.01,
            "session" : "SD0930",
            "volscale" : 1,
            "trademode": 2
        },
        "IDX" : 
        {
            "category" : 0,
            "covermode" : 0,
            "exchg" : "SSE",
            "holiday" : "CHINA",
            "name" : "上证指数",
            "precision" : 2,
            "pricemode" : 1,
            "pricetick" : 0.01,
            "session" : "SD0930",
            "volscale" : 1
        }
    },
}
```

Example 4 (unknown):
```unknown
{
    "SSE" : 
    {
        "STK" : 
        {
            "category" : 0,
            "covermode" : 0,
            "exchg" : "SSE",
            "holiday" : "CHINA",
            "name" : "上证股票",
            "precision" : 2,
            "pricemode" : 1,
            "pricetick" : 0.01,
            "session" : "SD0930",
            "volscale" : 1,
            "trademode": 2
        },
        "IDX" : 
        {
            "category" : 0,
            "covermode" : 0,
            "exchg" : "SSE",
            "holiday" : "CHINA",
            "name" : "上证指数",
            "precision" : 2,
            "pricemode" : 1,
            "pricetick" : 0.01,
            "session" : "SD0930",
            "volscale" : 1
        }
    },
}
```

---

## SEL引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/交易引擎/3.SEL引擎/

**Contents:**
- SEL引擎

也叫异步策略引擎，一般适用于标的较多，计算逻辑耗时较长的策略，时间驱动。典型应用场景包括多因子选股策略、截面多空策略等。

---

## 交易引擎

**URL:** https://zzzzhej.github.io/WonderTrader-Learning-Notes/开发手册/WTPY/交易引擎/

**Contents:**
- 交易引擎

---
