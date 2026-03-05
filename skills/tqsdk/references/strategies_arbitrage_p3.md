# TqSdk 策略示例 - 套利策略 (Part 3)

**包含策略:** Methanol-to-Olefins MTO Arbitrage, Copper-Aluminum Ratio Arbitrage, Crush Spread Arbitrage

---

### 7. Methanol-to-Olefins MTO Arbitrage (甲醇制烯烃/MTO产业链套利策略)

This strategy exploits price spread fluctuations between polyethylene (L) and polypropylene (PP) futures versus methanol (MA) futures. It monitors the "MTO profit spread" using Z-score standardization, executing mean-reversion trades when spreads deviate significantly from historical averages.

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
MA = "CZCE.MA409"      # 甲醇期货合约
L = "DCE.l2409"        # 聚乙烯期货合约
PP = "DCE.pp2409"      # 聚丙烯期货合约
START_DATE = date(2023, 11, 1)   # 回测开始日期
END_DATE = date(2024, 4, 9)     # 回测结束日期

# 套利参数
LOOKBACK_DAYS = 30        # 计算历史价差的回溯天数
STD_THRESHOLD = 2.0       # 标准差阈值，超过此阈值视为套利机会
ORDER_VOLUME = 100         # 聚乙烯的下单手数
CLOSE_THRESHOLD = 0.5     # 平仓阈值(标准差)

# 生产比例（可根据实际工艺调整）
MA_RATIO = 3      # 生产1吨烯烃消耗3吨甲醇
L_RATIO = 1       # 产出1吨聚乙烯
PP_RATIO = 1      # 产出1吨聚丙烯

# === 初始化API ===
api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

# 获取合约行情和K线
ma_quote = api.get_quote(MA)
l_quote = api.get_quote(L)
pp_quote = api.get_quote(PP)

ma_klines = api.get_kline_serial(MA, 60*60*24, LOOKBACK_DAYS)
l_klines = api.get_kline_serial(L, 60*60*24, LOOKBACK_DAYS)
pp_klines = api.get_kline_serial(PP, 60*60*24, LOOKBACK_DAYS)

# 创建目标持仓任务
ma_pos = TargetPosTask(api, MA)
l_pos = TargetPosTask(api, L)
pp_pos = TargetPosTask(api, PP)

# 获取合约乘数
ma_volume_multiple = ma_quote.volume_multiple
l_volume_multiple = l_quote.volume_multiple
pp_volume_multiple = pp_quote.volume_multiple

# 初始化状态变量
position_time = 0        # 建仓时间
in_position = False      # 是否有持仓
mean_spread = 0          # 历史价差均值
std_spread = 0           # 历史价差标准差

print(f"策略启动，监控合约: {MA}, {L}, {PP}")

# === 主循环 ===
try:
    # 初始计算历史统计值
    spreads = []
    for i in range(len(ma_klines) - 1):
        ma_price = ma_klines.close.iloc[i] * ma_volume_multiple * MA_RATIO
        l_price = l_klines.close.iloc[i] * l_volume_multiple * L_RATIO
        pp_price = pp_klines.close.iloc[i] * pp_volume_multiple * PP_RATIO
        
        # MTO利润 = (L价值 + PP价值) - MA成本
        spread = (l_price + pp_price) - ma_price
        spreads.append(spread)
    
    mean_spread = np.mean(spreads)
    std_spread = np.std(spreads)
    print(f"历史MTO利润均值: {mean_spread:.2f}, 标准差: {std_spread:.2f}")

    # 主循环
    while True:
        api.wait_update()
        
        # 当K线数据有变化时进行计算
        if api.is_changing(ma_klines) or api.is_changing(l_klines) or api.is_changing(pp_klines):
            # 重新计算历史价差统计
            spreads = []
            for i in range(len(ma_klines) - 1):
                ma_price = ma_klines.close.iloc[i] * ma_volume_multiple * MA_RATIO
                l_price = l_klines.close.iloc[i] * l_volume_multiple * L_RATIO
                pp_price = pp_klines.close.iloc[i] * pp_volume_multiple * PP_RATIO
                
                spread = (l_price + pp_price) - ma_price
                spreads.append(spread)
            
            mean_spread = np.mean(spreads)
            std_spread = np.std(spreads)
            
            # 计算当前利润价差
            ma_price = ma_klines.close.iloc[-1] * ma_volume_multiple * MA_RATIO
            l_price = l_klines.close.iloc[-1] * l_volume_multiple * L_RATIO
            pp_price = pp_klines.close.iloc[-1] * pp_volume_multiple * PP_RATIO
            
            current_spread = (l_price + pp_price) - ma_price
            
            # 计算z-score (标准化的价差)
            z_score = (current_spread - mean_spread) / std_spread
            
            print(f"当前MTO利润: {current_spread:.2f}, Z-score: {z_score:.2f}")
            
            # 获取当前持仓
            ma_position = api.get_position(MA)
            l_position = api.get_position(L)
            pp_position = api.get_position(PP)
            
            current_ma_pos = ma_position.pos_long - ma_position.pos_short
            current_l_pos = l_position.pos_long - l_position.pos_short
            current_pp_pos = pp_position.pos_long - pp_position.pos_short
            
            # 计算实际下单手数（依据比例）
            ma_volume = int(ORDER_VOLUME * MA_RATIO / L_RATIO)
            # L和PP按同等手数下单
            
            # === 交易信号判断 ===
            if not in_position:  # 如果没有持仓
                if z_score > STD_THRESHOLD:  # 利润显著高于均值
                    # 做空利润：卖出L和PP，买入MA
                    print(f"做空利润：卖出L{ORDER_VOLUME}手和PP{ORDER_VOLUME}手，买入MA{ma_volume}手")
                    l_pos.set_target_volume(-ORDER_VOLUME)
                    pp_pos.set_target_volume(-ORDER_VOLUME)
                    ma_pos.set_target_volume(ma_volume)
                    position_time = time.time()
                    in_position = True
                    
                elif z_score < -STD_THRESHOLD:  # 利润显著低于均值
                    # 做多利润：买入L和PP，卖出MA
                    print(f"做多利润：买入L{ORDER_VOLUME}手和PP{ORDER_VOLUME}手，卖出MA{ma_volume}手")
                    l_pos.set_target_volume(ORDER_VOLUME)
                    pp_pos.set_target_volume(ORDER_VOLUME)
                    ma_pos.set_target_volume(-ma_volume)
                    position_time = time.time()
                    in_position = True
            
            else:  # 如果已有持仓
                # 检查是否应当平仓
                if abs(z_score) < CLOSE_THRESHOLD:  # 利润回归正常
                    print("利润回归正常，平仓所有头寸")
                    l_pos.set_target_volume(0)
                    pp_pos.set_target_volume(0)
                    ma_pos.set_target_volume(0)
                    in_position = False
                
                # 止损逻辑
                if (z_score > STD_THRESHOLD * 1.5 and current_l_pos < 0) or \
                   (z_score < -STD_THRESHOLD * 1.5 and current_l_pos > 0):
                    print("止损：利润向不利方向进一步偏离")
                    l_pos.set_target_volume(0)
                    pp_pos.set_target_volume(0)
                    ma_pos.set_target_volume(0)
                    in_position = False

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 8. Copper-Aluminum Ratio Arbitrage (铜铝比价套利策略)

This strategy monitors the price ratio between copper and aluminum futures. It exploits the mean-reverting behavior of the "copper-aluminum value ratio" by establishing positions when the ratio significantly deviates from its historical average.

```python
from tqsdk import TqApi, TqAuth, TargetPosTask, TqBacktest, BacktestFinished
import numpy as np
from datetime import date

CU = "SHFE.cu2407"
AL = "SHFE.al2407"
START_DATE = date(2023, 11, 1)
END_DATE = date(2024, 4, 30)
LOOKBACK_DAYS = 30
STD_THRESHOLD = 2.0
ORDER_VOLUME = 30
CLOSE_THRESHOLD = 0.5

api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE), auth=TqAuth("快期账号", "快期密码"))

cu_quote = api.get_quote(CU)
al_quote = api.get_quote(AL)
cu_klines = api.get_kline_serial(CU, 60*60*24, LOOKBACK_DAYS)
al_klines = api.get_kline_serial(AL, 60*60*24, LOOKBACK_DAYS)
cu_pos = TargetPosTask(api, CU)
al_pos = TargetPosTask(api, AL)

try:
    # 计算历史铜铝比
    ratios = []
    for i in range(len(cu_klines) - 1):
        cu_price = cu_klines.close.iloc[i] * cu_quote.volume_multiple
        al_price = al_klines.close.iloc[i] * al_quote.volume_multiple
        ratios.append(cu_price / al_price)
    mean_ratio = np.mean(ratios)
    std_ratio = np.std(ratios)
    print(f"历史铜铝比均值: {mean_ratio:.4f}, 标准差: {std_ratio:.4f}")

    in_position = False
    while True:
        api.wait_update()
        if api.is_changing(cu_klines) or api.is_changing(al_klines):
            # 重新计算
            ratios = []
            for i in range(len(cu_klines) - 1):
                cu_price = cu_klines.close.iloc[i] * cu_quote.volume_multiple
                al_price = al_klines.close.iloc[i] * al_quote.volume_multiple
                ratios.append(cu_price / al_price)
            mean_ratio = np.mean(ratios)
            std_ratio = np.std(ratios)
            cu_price = cu_klines.close.iloc[-1] * cu_quote.volume_multiple
            al_price = al_klines.close.iloc[-1] * al_quote.volume_multiple
            current_ratio = cu_price / al_price
            z_score = (current_ratio - mean_ratio) / std_ratio
            print(f"当前铜铝比: {current_ratio:.4f}, Z-score: {z_score:.2f}")

            cu_position = api.get_position(CU)
            al_position = api.get_position(AL)
            current_cu_pos = cu_position.pos_long - cu_position.pos_short
            current_al_pos = al_position.pos_long - al_position.pos_short

            if not in_position:
                if z_score > STD_THRESHOLD:
                    # 做多铜铝比：买入铜，卖出铝
                    print(f"做多铜铝比：买入铜{ORDER_VOLUME}手，卖出铝{ORDER_VOLUME}手")
                    cu_pos.set_target_volume(ORDER_VOLUME)
                    al_pos.set_target_volume(-ORDER_VOLUME)
                    in_position = True
                elif z_score < -STD_THRESHOLD:
                    # 做空铜铝比：卖出铜，买入铝
                    print(f"做空铜铝比：卖出铜{ORDER_VOLUME}手，买入铝{ORDER_VOLUME}手")
                    cu_pos.set_target_volume(-ORDER_VOLUME)
                    al_pos.set_target_volume(ORDER_VOLUME)
                    in_position = True
            else:
                if abs(z_score) < CLOSE_THRESHOLD:
                    print("比率回归正常，平仓所有头寸")
                    cu_pos.set_target_volume(0)
                    al_pos.set_target_volume(0)
                    in_position = False
                # 止损逻辑
                if (z_score > STD_THRESHOLD * 1.5 and current_cu_pos < 0) or \
                   (z_score < -STD_THRESHOLD * 1.5 and current_cu_pos > 0):
                    print("止损：比率向不利方向进一步偏离")
                    cu_pos.set_target_volume(0)
                    al_pos.set_target_volume(0)
                    in_position = False

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 9. Crush Spread Arbitrage (压榨价差套利策略)

This strategy exploits price differences between soybean futures and their processed products (soybean meal and soybean oil). It uses Z-score to identify when the crush spread deviates significantly from historical averages, then initiates mean-reversion trades.

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
SOYBEAN = "DCE.a2409"    # 大豆期货合约
SOYMEAL = "DCE.m2409"    # 豆粕期货合约
SOYOIL = "DCE.y2409"     # 豆油期货合约
START_DATE = date(2023, 11, 1)   # 回测开始日期
END_DATE = date(2024, 4, 30)     # 回测结束日期

# 套利参数
LOOKBACK_DAYS = 30       # 计算历史价差的回溯天数
STD_THRESHOLD = 2.0      # 标准差阈值，超过此阈值视为套利机会
ORDER_VOLUME = 500        # 大豆的下单手数
CLOSE_THRESHOLD = 0.5    # 平仓阈值(标准差)

# 压榨价差比例 - 1吨大豆压榨可得约0.785吨豆粕和0.18吨豆油
# 为了简化，使用10:8:2的整数比例
BEAN_RATIO = 10
MEAL_RATIO = 8
OIL_RATIO = 2

# === 初始化API ===
api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

# 获取合约行情和K线
bean_quote = api.get_quote(SOYBEAN)
meal_quote = api.get_quote(SOYMEAL)
oil_quote = api.get_quote(SOYOIL)

bean_klines = api.get_kline_serial(SOYBEAN, 60*60*24, LOOKBACK_DAYS)
meal_klines = api.get_kline_serial(SOYMEAL, 60*60*24, LOOKBACK_DAYS)
oil_klines = api.get_kline_serial(SOYOIL, 60*60*24, LOOKBACK_DAYS)

# 创建目标持仓任务
bean_pos = TargetPosTask(api, SOYBEAN)
meal_pos = TargetPosTask(api, SOYMEAL)
oil_pos = TargetPosTask(api, SOYOIL)

# 获取合约乘数
bean_volume_multiple = bean_quote.volume_multiple
meal_volume_multiple = meal_quote.volume_multiple
oil_volume_multiple = oil_quote.volume_multiple

# 初始化状态变量
position_time = 0        # 建仓时间
in_position = False      # 是否有持仓
mean_spread = 0          # 历史价差均值
std_spread = 0           # 历史价差标准差

print(f"策略启动，监控合约: {SOYBEAN}, {SOYMEAL}, {SOYOIL}")

# === 主循环 ===
try:
    # 初始计算历史统计值
    spreads = []
    for i in range(len(bean_klines) - 1):
        bean_price = bean_klines.close.iloc[i] * bean_volume_multiple * BEAN_RATIO
        meal_price = meal_klines.close.iloc[i] * meal_volume_multiple * MEAL_RATIO
        oil_price = oil_klines.close.iloc[i] * oil_volume_multiple * OIL_RATIO
        
        # 压榨价差 = (豆粕价值 + 豆油价值) - 大豆价值
        spread = (meal_price + oil_price) - bean_price
        spreads.append(spread)
    
    mean_spread = np.mean(spreads)
    std_spread = np.std(spreads)
    print(f"历史压榨价差均值: {mean_spread:.2f}, 标准差: {std_spread:.2f}")

    # 主循环
    while True:
        api.wait_update()
        
        # 当K线数据有变化时进行计算
        if api.is_changing(bean_klines) or api.is_changing(meal_klines) or api.is_changing(oil_klines):
            # 重新计算历史价差统计
            spreads = []
            for i in range(len(bean_klines) - 1):
                bean_price = bean_klines.close.iloc[i] * bean_volume_multiple * BEAN_RATIO
                meal_price = meal_klines.close.iloc[i] * meal_volume_multiple * MEAL_RATIO
                oil_price = oil_klines.close.iloc[i] * oil_volume_multiple * OIL_RATIO
                
                spread = (meal_price + oil_price) - bean_price
                spreads.append(spread)
            
            mean_spread = np.mean(spreads)
            std_spread = np.std(spreads)
            
            # 计算当前压榨价差
            bean_price = bean_klines.close.iloc[-1] * bean_volume_multiple * BEAN_RATIO
            meal_price = meal_klines.close.iloc[-1] * meal_volume_multiple * MEAL_RATIO
            oil_price = oil_klines.close.iloc[-1] * oil_volume_multiple * OIL_RATIO
            
            current_spread = (meal_price + oil_price) - bean_price
            
            # 计算z-score (标准化的价差)
            z_score = (current_spread - mean_spread) / std_spread
            
            print(f"当前压榨价差: {current_spread:.2f}, Z-score: {z_score:.2f}")
            
            # 获取当前持仓
            bean_position = api.get_position(SOYBEAN)
            meal_position = api.get_position(SOYMEAL)
            oil_position = api.get_position(SOYOIL)
            
            current_bean_pos = bean_position.pos_long - bean_position.pos_short
            current_meal_pos = meal_position.pos_long - meal_position.pos_short
            current_oil_pos = oil_position.pos_long - oil_position.pos_short
            
            # 计算实际下单手数（依据比例）
            meal_volume = int(ORDER_VOLUME * MEAL_RATIO / BEAN_RATIO)
            oil_volume = int(ORDER_VOLUME * OIL_RATIO / BEAN_RATIO)
            
            # === 交易信号判断 ===
            if not in_position:  # 如果没有持仓
                if z_score > STD_THRESHOLD:  # 价差显著高于均值，压榨利润偏高
                    # 卖出压榨价差：买入大豆，卖出豆粕和豆油
                    print(f"卖出压榨价差：买入大豆{ORDER_VOLUME}手，卖出豆粕{meal_volume}手和豆油{oil_volume}手")
                    bean_pos.set_target_volume(ORDER_VOLUME)
                    meal_pos.set_target_volume(-meal_volume)
                    oil_pos.set_target_volume(-oil_volume)
                    position_time = time.time()
                    in_position = True
                    
                elif z_score < -STD_THRESHOLD:  # 价差显著低于均值，压榨利润偏低
                    # 买入压榨价差：卖出大豆，买入豆粕和豆油
                    print(f"买入压榨价差：卖出大豆{ORDER_VOLUME}手，买入豆粕{meal_volume}手和豆油{oil_volume}手")
                    bean_pos.set_target_volume(-ORDER_VOLUME)
                    meal_pos.set_target_volume(meal_volume)
                    oil_pos.set_target_volume(oil_volume)
                    position_time = time.time()
                    in_position = True
            
            else:  # 如果已有持仓
                # 检查是否应当平仓
                if abs(z_score) < CLOSE_THRESHOLD:  # 价差恢复正常
                    print("价差恢复正常，平仓所有头寸")
                    bean_pos.set_target_volume(0)
                    meal_pos.set_target_volume(0)
                    oil_pos.set_target_volume(0)
                    in_position = False
                
                # 也可以添加止损逻辑
                if (z_score > STD_THRESHOLD * 1.5 and current_bean_pos > 0) or \
                   (z_score < -STD_THRESHOLD * 1.5 and current_bean_pos < 0):
                    print("止损：价差向不利方向进一步偏离")
                    bean_pos.set_target_volume(0)
                    meal_pos.set_target_volume(0)
                    oil_pos.set_target_volume(0)
                    in_position = False

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---