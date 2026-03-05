# TqSdk 策略示例 - 套利策略 (Part 4)

**包含策略:** Crack Spread Arbitrage, Stock Index Futures Cross-Period Spread Arbitrage, Box Spread Arbitrage, Traditional Model Cross-Period Arbitrage

---

### 10. Crack Spread Arbitrage (裂解价差套利策略)

This strategy exploits price discrepancies between crude oil futures and refined products (gasoline/fuel oil and diesel/rubber). When the spread deviates significantly from historical norms, it establishes offsetting positions and profits when the spread reverts to normal levels.

```python
#!/usr/bin/env python
# coding=utf-8
__author__ = "Chaos"

from datetime import date

from tqsdk import TqApi, TqAuth, TargetPosTask, TqBacktest, BacktestFinished
import numpy as np
import time

# === 用户参数 ===
# 合约参数
CRUDE_OIL = "INE.sc2405"  # 原油期货合约
GASOLINE = "SHFE.fu2405"   # 燃料油期货合约
DIESEL = "INE.nr2405"      # 柴油期货合约
START_DATE = date(2024, 1, 26)  # 回测开始日期
END_DATE = date(2024, 3, 1)  # 回测结束日期

# 套利参数
LOOKBACK_DAYS = 30         # 计算历史价差的回溯天数
STD_THRESHOLD = 1.5        # 标准差阈值，超过此阈值视为套利机会
ORDER_VOLUME = 30           # 下单手数
CLOSE_THRESHOLD = 0.3      # 平仓阈值(标准差)

# 裂解价差比例
CRUDE_RATIO = 2
GASOLINE_RATIO = 1
DIESEL_RATIO = 1

# === 初始化API ===
api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

# 获取合约行情和K线
crude_quote = api.get_quote(CRUDE_OIL)
gasoline_quote = api.get_quote(GASOLINE)
diesel_quote = api.get_quote(DIESEL)

crude_klines = api.get_kline_serial(CRUDE_OIL, 60*60*24, LOOKBACK_DAYS)
gasoline_klines = api.get_kline_serial(GASOLINE, 60*60*24, LOOKBACK_DAYS)
diesel_klines = api.get_kline_serial(DIESEL, 60*60*24, LOOKBACK_DAYS)

# 创建目标持仓任务
crude_pos = TargetPosTask(api, CRUDE_OIL)
gasoline_pos = TargetPosTask(api, GASOLINE)
diesel_pos = TargetPosTask(api, DIESEL)

# 获取合约乘数
crude_volume_multiple = crude_quote.volume_multiple
gasoline_volume_multiple = gasoline_quote.volume_multiple
diesel_volume_multiple = diesel_quote.volume_multiple

# 初始化状态变量
position_time = 0  # 建仓时间
in_position = False  # 是否有持仓
mean_spread = 0  # 历史价差均值
std_spread = 0  # 历史价差标准差

print(f"策略启动，监控合约: {CRUDE_OIL}, {GASOLINE}, {DIESEL}")

# === 主循环 ===
try:
    # 初始计算历史统计值
    spreads = []
    for i in range(len(crude_klines) - 1):
        crude_price = crude_klines.close.iloc[i] * crude_volume_multiple * CRUDE_RATIO
        gasoline_price = gasoline_klines.close.iloc[i] * gasoline_volume_multiple * GASOLINE_RATIO
        diesel_price = diesel_klines.close.iloc[i] * diesel_volume_multiple * DIESEL_RATIO
        
        spread = (gasoline_price + diesel_price) - crude_price
        spreads.append(spread)
    
    mean_spread = np.mean(spreads)
    std_spread = np.std(spreads)
    print(f"历史裂解价差均值: {mean_spread:.2f}, 标准差: {std_spread:.2f}")

    # 主循环
    while True:
        api.wait_update()
        
        # 当K线数据有变化时进行计算
        if api.is_changing(crude_klines) or api.is_changing(gasoline_klines) or api.is_changing(diesel_klines):
            # 重新计算历史价差统计
            spreads = []
            for i in range(len(crude_klines) - 1):
                crude_price = crude_klines.close.iloc[i] * crude_volume_multiple * CRUDE_RATIO
                gasoline_price = gasoline_klines.close.iloc[i] * gasoline_volume_multiple * GASOLINE_RATIO
                diesel_price = diesel_klines.close.iloc[i] * diesel_volume_multiple * DIESEL_RATIO
                
                spread = (gasoline_price + diesel_price) - crude_price
                spreads.append(spread)
            
            mean_spread = np.mean(spreads)
            std_spread = np.std(spreads)
            
            # 计算当前裂解价差
            crude_price = crude_klines.close.iloc[-1] * crude_volume_multiple * CRUDE_RATIO
            gasoline_price = gasoline_klines.close.iloc[-1] * gasoline_volume_multiple * GASOLINE_RATIO
            diesel_price = diesel_klines.close.iloc[-1] * diesel_volume_multiple * DIESEL_RATIO
            
            current_spread = (gasoline_price + diesel_price) - crude_price
            
            # 计算z-score (标准化的价差)
            z_score = (current_spread - mean_spread) / std_spread
            
            print(f"当前裂解价差: {current_spread:.2f}, Z-score: {z_score:.2f}")
            
            # 获取当前持仓
            crude_position = api.get_position(CRUDE_OIL)
            gasoline_position = api.get_position(GASOLINE)
            diesel_position = api.get_position(DIESEL)
            
            current_crude_pos = crude_position.pos_long - crude_position.pos_short
            current_gasoline_pos = gasoline_position.pos_long - gasoline_position.pos_short
            current_diesel_pos = diesel_position.pos_long - diesel_position.pos_short
            
            # === 交易信号判断 ===
            if not in_position:  # 如果没有持仓
                if z_score > STD_THRESHOLD:  # 价差显著高于均值
                    # 卖出裂解价差：买入原油，卖出汽油和柴油
                    print("卖出裂解价差：买入原油，卖出汽油和柴油")
                    crude_pos.set_target_volume(ORDER_VOLUME)
                    gasoline_pos.set_target_volume(-ORDER_VOLUME)
                    diesel_pos.set_target_volume(-ORDER_VOLUME)
                    position_time = time.time()
                    in_position = True
                    
                elif z_score < -STD_THRESHOLD:  # 价差显著低于均值
                    # 买入裂解价差：卖出原油，买入汽油和柴油
                    print("买入裂解价差：卖出原油，买入汽油和柴油")
                    crude_pos.set_target_volume(-ORDER_VOLUME)
                    gasoline_pos.set_target_volume(ORDER_VOLUME)
                    diesel_pos.set_target_volume(ORDER_VOLUME)
                    position_time = time.time()
                    in_position = True
            
            else:  # 如果已有持仓
                # 检查是否应当平仓
                if abs(z_score) < CLOSE_THRESHOLD:  # 价差恢复正常
                    print("价差恢复正常，平仓所有头寸")
                    crude_pos.set_target_volume(0)
                    gasoline_pos.set_target_volume(0)
                    diesel_pos.set_target_volume(0)
                    in_position = False

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 11. Stock Index Futures Cross-Period Spread Arbitrage (股指期货跨期套利策略)

This strategy exploits the mean reversion principle of price spreads between different contract months of the same stock index future. It identifies when spreads deviate from their historical range and establishes arbitrage positions, profiting when spreads normalize.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date

import numpy as np
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished

# 参数设置
NEAR_CONTRACT = "CFFEX.IH2101"  # 近月合约
FAR_CONTRACT = "CFFEX.IH2102"  # 远月合约
K = 2  # 标准差倍数
WINDOW = 80  # 计算窗口
LOTS = 20  # 交易手数
START_DATE = date(2020, 12, 21)  # 回测开始日期
END_DATE = date(2021, 1, 15)  # 回测结束日期

# 创建API实例
api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

# 订阅行情
near_quote = api.get_quote(NEAR_CONTRACT)
far_quote = api.get_quote(FAR_CONTRACT)
near_klines = api.get_kline_serial(NEAR_CONTRACT, 15 * 60, WINDOW * 2)  # 15分钟K线
far_klines = api.get_kline_serial(FAR_CONTRACT, 15 * 60, WINDOW * 2)

# 创建目标持仓任务
near_pos = TargetPosTask(api, NEAR_CONTRACT)
far_pos = TargetPosTask(api, FAR_CONTRACT)

# 持仓状态: 0-无持仓, 1-多价差(买近卖远), -1-空价差(卖近买远)
position_state = 0

print(f"策略启动: {NEAR_CONTRACT}-{FAR_CONTRACT} 跨期套利")
print(f"参数设置: K={K}倍标准差, 窗口={WINDOW}, 交易手数={LOTS}手")

try:
    while True:
        api.wait_update()

        # 检查K线是否更新
        if api.is_changing(near_klines) or api.is_changing(far_klines):
            # 确保有足够的数据
            if len(near_klines) < WINDOW or len(far_klines) < WINDOW:
                continue

            # 计算价差指标
            near_close = near_klines.close.iloc[-WINDOW:]
            far_close = far_klines.close.iloc[-WINDOW:]
            spread = near_close - far_close

            # 计算均值和标准差
            mean = np.mean(spread)
            std = np.std(spread)
            current_spread = near_quote.last_price - far_quote.last_price

            # 计算上下边界
            upper_bound = mean + K * std
            lower_bound = mean - K * std

            print(f"价差: {current_spread:.2f}, 均值: {mean:.2f}, "
                  f"上界: {upper_bound:.2f}, 下界: {lower_bound:.2f}")

            # 交易逻辑
            if position_state == 0:  # 无持仓状态
                if current_spread > upper_bound:  # 做空价差(卖近买远)
                    near_pos.set_target_volume(-LOTS)
                    far_pos.set_target_volume(LOTS)
                    position_state = -1
                    print(f"开仓: 卖出{LOTS}手{NEAR_CONTRACT}, 买入{LOTS}手{FAR_CONTRACT}")

                elif current_spread < lower_bound:  # 做多价差(买近卖远)
                    near_pos.set_target_volume(LOTS)
                    far_pos.set_target_volume(-LOTS)
                    position_state = 1
                    print(f"开仓: 买入{LOTS}手{NEAR_CONTRACT}, 卖出{LOTS}手{FAR_CONTRACT}")

            elif position_state == 1:  # 持有多价差
                if current_spread >= mean:  # 平仓获利
                    near_pos.set_target_volume(0)
                    far_pos.set_target_volume(0)
                    position_state = 0
                    print("平仓: 价差回到均值，平仓获利")

            elif position_state == -1:  # 持有空价差
                if current_spread <= mean:  # 平仓获利
                    near_pos.set_target_volume(0)
                    far_pos.set_target_volume(0)
                    position_state = 0
                    print("平仓: 价差回到均值，平仓获利")

except BacktestFinished as e:
    api.close()
    print("回测结束")
```

---

---

### 12. Box Spread Arbitrage (盒式套利策略)

Box spread arbitrage is a risk-free options strategy that combines a bull call spread and a bear put spread using four option contracts with matching strike prices. Once executed, the profit is unaffected by any market movement. This code finds suitable box spread combinations.

```python
#!/usr/bin/env python # -*- coding: utf-8 -*- __author__ = "Lisiheng"
from tqsdk import TqApi

api = TqApi(auth="信易账户,信易密码")
quote_put = api.query_options("DCE.m2105",option_class="CALL", expired=False)
#  取现有上市的DCE.m2105 期权合约
for i, x in enumerate(quote_put):
    for j, y in enumerate(quote_put[i + 1:]):
        strike_price1 = int(x[12:17])  # 取第一个期权的执行价
        strike_price2 = int(y[12:17])  # 取序列后一个期权的执行价
        strike_high = max(strike_price1, strike_price2)  # 取一组期权执行价中高的一个
        strike_low = min(strike_price1, strike_price2)  # 取一组期权执行价中低的一个
        symbol_lowc = "DCE.m2105-C-" + str(strike_low)  # 组合出低执行价看涨期权
        symbol_highc = "DCE.m2105-C-" + str(strike_high)  # 组合出高执行价看涨期权
        symbol_highp = "DCE.m2105-P-" + str(strike_high)  # 组合出低执行价看跌期权
        symbol_lowp = "DCE.m2105-P-" + str(strike_low)  # 组合出低执行价看跌期权
        buy_call = api.get_quote(symbol_lowc)  # 获取要买入的看涨期权实时行情
        sell_call = api.get_quote(symbol_highc)  # 获取要卖出的看涨期权实时行情
        sell_put = api.get_quote(symbol_lowp)  # 获取要卖出的看涨期权实时行情
        buy_put = api.get_quote(symbol_highp)  # 获取要买入的看涨期权实时行情
        while True:
            api.wait_update()
            if api.is_changing(buy_call, "ask_price1") or api.is_changing(sell_call, "bid_price1") or \
                    api.is_changing(buy_put, "ask_price1") or api.is_changing(sell_put, "bid_price1"):
                print(sell_put.bid_price1)
                premium_receive = (sell_call.bid_price1 + sell_put.bid_price1)*buy_call.volume_multiple  # 计算应收权利金
                premium_paid = (buy_call.ask_price1 + buy_put.ask_price1)*buy_call.volume_multiple  # 计算应付权利金
                commission = 0  # 计算交易成本
                SHIBOR_6m = 0.031810  # 取得持有到期期间的SHIBOR作为无风险利率
                discount_factor = 1/(1+SHIBOR_6m/2)  # 计算折现率
                net_premium_paid = premium_paid + commission - premium_receive  # 计算净支出
                max_profit = int(symbol_highc[12:17]) - int(symbol_lowc[12:17])  # 计算最大收入
                profit = max_profit*discount_factor - net_premium_paid  # 计算总收入
                if profit > 0:  # 判断是否超过无风险套利收益的可能
                    print("收益", profit, "买", symbol_lowc, symbol_highp, "卖", symbol_highc, symbol_lowp)  # 得出交易组合
```

---

---

### 13. Traditional Model Cross-Period Arbitrage (传统模型跨期套利策略/传统商品期货)

This strategy exploits price discrepancies between different delivery months of the same commodity when they deviate from theoretical values. It employs a cost-of-carry model to identify when actual spreads diverge from rational spreads, then executes spread trades anticipating mean reversion.

```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = "Lisiheng"

from tqsdk import TqApi, TqAuth, TargetPosTask, TqSim

# 设置初始资金，信易账号，模拟模式，回测模式和时间，web_gui开启

api = TqApi(TqSim(init_balance=10000000), auth=TqAuth("信易账户", "账号密码"), web_gui=True)

# 参数和合约的相关初始设置

symbol1 = "SHFE.cu2006" # 近期合约

symbol2 = "SHFE.cu2010" # 远期合约

target_pos1 = TargetPosTask(api, symbol1) # 近期合约TargetPosTask设置

target_pos2 = TargetPosTask(api, symbol2) # 远期合约TargetPosTask设置

quote1 = api.get_quote(symbol1) # 取得近期合约最新行情

quote2 = api.get_quote(symbol2) # 取得远期合约最新行情

pos1 = api.get_position(symbol1) # 取得近期合约持仓情况

pos2 = api.get_position(symbol2) # 取得远期合约持仓情况

interest_rate = 1 + 0.0404*3/12 # 远近合约月份之间的利率系数理论值

# 定义价差相关参数

def difference():

    theory_price = quote1.last_price * interest_rate # 根据利率系数计算出的远期合约理论价格

    theory = quote1.last_price - theory_price # 近期实际价格和远期理论价格的价差

    real = quote1.last_price - quote2.last_price # 近期实际价格和远期实际价格的价差

    floor = theory - 200 # -200为自己设置的参数，扩大利润区间下界

    cap = theory + 200 # +200为自己设置的参数，扩大利润区间上界

    close_low = theory - 50 # -50为自己设置的参数，扩大平仓区间上界

    close_high = theory + 50 # +50为自己设置的参数，扩大平仓区间下界

    return theory_price, theory, real, floor, cap, close_low, close_high

while True:

    api.wait_update()

    # 如果近期远期合约最新价变动，相关理论价格和实际价格涉及到的价差更新

    if api.is_changing(quote1, "last_price") or api.is_changing(quote2, "last_price"):

        dif_theory_price, dif_theory, dif_real, dif_floor, dif_cap, dif_closelow, dif_closehigh = difference()

        # 判定目前近期和远期仓位是否为空仓，以及判定价差关系

        if (pos1.pos_long == 0 and pos1.pos_short == 0) and (pos2.pos_long == 0 and pos2.pos_short == 0) and (dif_real < dif_floor):

            print("可以买近卖远交易开仓", dif_real, dif_theory)

            target_pos1.set_target_volume(30) # 实际价差被低估的情况时，近期合约cu2006净持仓调整为30手

            target_pos2.set_target_volume(-30) # 实际价差被低估的情况时，同时远期合约cu2008净持仓调整为-30手

        # 判定目前近期和远期仓位是否为空仓，以及判定价差关系

        elif (pos1.pos_long == 0 and pos1.pos_short == 0) and (pos2.pos_long == 0 and pos2.pos_short == 0) and (dif_real > dif_cap):

            print("可以卖近买远开仓", dif_real, dif_theory)

            target_pos1.set_target_volume(-30) # 实际价差被高估的情况时，近期合约cu2006净持仓调整为-30手

            target_pos2.set_target_volume(30) # 实际价差被高估的情况时，同时远期合约cu2008净持仓调整为30手

        # 判定目前近期和远期仓位是否都有仓位，以及判定价差关系

        elif (pos1.pos_long != 0 or pos1.pos_short != 0 or pos2.pos_long != 0 or pos2.pos_short != 0) and (dif_closelow < dif_real < dif_closehigh):

            target_pos1.set_target_volume(0) # 实际价差接近理论值的情况时，近期合约cu2006的净持仓调整为0手

            target_pos2.set_target_volume(0) # 实际价差接近理论值的情况时，远期合约cu2008的净持仓调整为0手

            print("平仓", dif_real, dif_theory)
```

---