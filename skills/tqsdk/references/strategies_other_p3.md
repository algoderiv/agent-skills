# TqSdk 策略示例 - 均值回归与其他策略 (Part 3)

**包含策略:** Mean-Reversion Strategy, Iceberg Order Algorithm, Order Book Placement Algorithm, Percentage of Volume / POV

---

### 7. Mean-Reversion Strategy (反转策略)

Capitalizes on price oscillations by entering positions when prices reach extreme levels, utilizing RSI and Bollinger Bands combined with moving average filters on hourly gold futures data.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"
from tqsdk import TqApi, TqAuth, TargetPosTask, TqSim, TqBacktest, BacktestFinished
from tqsdk.ta import RSI, BOLL, MA
from datetime import datetime
from tqsdk.tafunc import time_to_str

# Strategy Parameters
SYMBOL = "SHFE.au2106"
RSI_PERIOD = 9
OVERBOUGHT = 78
OVERSOLD = 22
BOLL_PERIOD = 15
BOLL_DEV = 2.5
STOP_LOSS_PCT = 0.015
TAKE_PROFIT_PCT = 0.045
TRADE_VOL = 10

account = TqSim()
api = TqApi(web_gui=True,backtest=TqBacktest
        (start_dt=datetime(2020, 11, 1),
        end_dt=datetime(2021, 5, 1)),
        account=account, auth=TqAuth("快期账号", "快期密码"))

klines = api.get_kline_serial(SYMBOL, 60 * 60)
quote = api.get_quote(SYMBOL)

target_pos = TargetPosTask(api, SYMBOL)

try:
    position = 0
    entry_price = 0
    stop_loss = 0
    take_profit = 0
    
    while True:
        api.wait_update()
        print(time_to_str(klines.datetime.iloc[-2]))
        
        if api.is_changing(klines.iloc[-1], "datetime"):
            rsi = RSI(klines, RSI_PERIOD)
            boll = BOLL(klines, BOLL_PERIOD, BOLL_DEV)
            current_rsi = rsi.rsi.iloc[-2]
            current_close = klines.close.iloc[-2]
            up_track = boll["top"].iloc[-2]
            mid_track = boll["mid"].iloc[-2]
            low_track = boll["bottom"].iloc[-2]
            
            ma_short = MA(klines, 20)
            ma_long = MA(klines, 60)
            trend_up = ma_short.ma.iloc[-2] > ma_long.ma.iloc[-2]
            trend_down = ma_short.ma.iloc[-2] < ma_long.ma.iloc[-2]
            
            print(
                f"当前价格: {current_close}, RSI: {current_rsi}, 上轨: {up_track}, 中轨: {mid_track}, 下轨: {low_track}")
            
            if position == 0:
                if current_rsi < OVERSOLD and current_close < low_track and trend_down:
                    print(f"反转做多信号! RSI: {current_rsi}, 价格: {current_close}, 下轨: {low_track}")
                    target_pos.set_target_volume(TRADE_VOL)
                    position = 1
                    entry_price = current_close
                    stop_loss = entry_price * (1 - STOP_LOSS_PCT)
                    take_profit = entry_price * (1 + TAKE_PROFIT_PCT)
                    print(f"做多开仓! 入场价: {entry_price}, 止损: {stop_loss}, 止盈: {take_profit}")
                
                elif current_rsi > OVERBOUGHT and current_close > up_track and trend_up:
                    print(f"反转做空信号! RSI: {current_rsi}, 价格: {current_close}, 上轨: {up_track}")
                    target_pos.set_target_volume(-TRADE_VOL)
                    position = -1
                    entry_price = current_close
                    stop_loss = entry_price * (1 + STOP_LOSS_PCT)
                    take_profit = entry_price * (1 - TAKE_PROFIT_PCT)
                    print(f"做空开仓! 入场价: {entry_price}, 止损: {stop_loss}, 止盈: {take_profit}")
            
            else:
                if position == 1:
                    if current_close >= entry_price * 1.02:
                        stop_loss = entry_price * 1.002
                    
                    if current_close <= stop_loss:
                        print(f"触发止损! 价格: {current_close}, 止损线: {stop_loss}")
                        target_pos.set_target_volume(0)
                        position = 0
                    elif current_close >= take_profit:
                        print(f"触发止盈! 价格: {current_close}, 止盈线: {take_profit}")
                        target_pos.set_target_volume(0)
                        position = 0
                    elif current_close >= mid_track and current_rsi >= 50 and (current_close - entry_price)/entry_price >= 0.02:
                        print(f"均值回归平仓信号! 价格: {current_close}, 中轨: {mid_track}, RSI: {current_rsi}")
                        target_pos.set_target_volume(0)
                        position = 0
                
                elif position == -1:
                    if current_close <= entry_price * 0.98:
                        stop_loss = entry_price * 0.998
                    
                    if current_close >= stop_loss:
                        print(f"触发止损! 价格: {current_close}, 止损线: {stop_loss}")
                        target_pos.set_target_volume(0)
                        position = 0
                    elif current_close <= take_profit:
                        print(f"触发止盈! 价格: {current_close}, 止盈线: {take_profit}")
                        target_pos.set_target_volume(0)
                        position = 0
                    elif current_close <= mid_track and current_rsi <= 50 and (current_close - entry_price)/entry_price >= 0.02:
                        print(f"均值回归平仓信号! 价格: {current_close}, 中轨: {mid_track}, RSI: {current_rsi}")
                        target_pos.set_target_volume(0)
                        position = 0

except BacktestFinished:
    api.close()
```

---

## OTHER STRATEGIES (其他策略)

---

---

### 8. Iceberg Order Algorithm (冰山订单算法)

Divides large orders into multiple smaller visible orders, executing them sequentially while keeping the true trading volume hidden from the market.

```python
#!/usr/bin/env python
# coding=utf-8
__author__ = "Chaos"

from tqsdk import TqApi, TqAuth, TqKq, TargetPosTask

# === 用户参数 ===
SYMBOL = "SHFE.ag2506"      # 交易合约
TOTAL_VOLUME = 100          # 目标总手数
MIN_VOLUME = 1              # 每笔最小委托手数
MAX_VOLUME = 10             # 每笔最大委托手数
DIRECTION = "BUY"           # "BUY"为买入，"SELL"为卖出
ORDER_TYPE = "ACTIVE"       # 对价 "ACTIVE" / 挂价 "PASSIVE" / 指定价 lambda direction: 价格

# === 初始化 ===
api = TqApi(account=TqKq(), auth=TqAuth("快期账号", "快期密码"))
quote = api.get_quote(SYMBOL)

# 创建目标持仓任务
target_pos = TargetPosTask(
    api, SYMBOL,
    price=ORDER_TYPE,
    min_volume=MIN_VOLUME,
    max_volume=MAX_VOLUME
)

# 获取下单方式描述
order_type_str = (f"指定价 {ORDER_TYPE(DIRECTION)}" if callable(ORDER_TYPE) 
                 else str(ORDER_TYPE))

print(f"冰山算法启动，合约: {SYMBOL}，目标: {TOTAL_VOLUME}手，"
      f"每批: {MIN_VOLUME}-{MAX_VOLUME}手，方向: {DIRECTION}，下单方式: {order_type_str}")

try:
    # 获取初始持仓并设置目标
    pos = api.get_position(SYMBOL)
    start_net_pos = pos.pos_long - pos.pos_short
    target_volume = start_net_pos + (TOTAL_VOLUME if DIRECTION == "BUY" else -TOTAL_VOLUME)
    target_pos.set_target_volume(target_volume)
    
    last_progress = 0  # 记录上次进度

    while True:
        api.wait_update()
        pos = api.get_position(SYMBOL)
        net_pos = pos.pos_long - pos.pos_short
        progress = abs(net_pos - start_net_pos)
        
        # 当进度发生变化时打印
        if progress != last_progress:
            print(f"当前进度: {progress}/{TOTAL_VOLUME}")
            last_progress = progress
        
        # 检查是否完成
        if (DIRECTION == "BUY" and net_pos >= target_volume) or \
           (DIRECTION == "SELL" and net_pos <= target_volume):
            print(f"冰山算法完成")
            break

except Exception as e:
    print(f"算法执行异常: {e}")
finally:
    api.close()
```

---

---

### 9. Order Book Placement Algorithm (盘口下单算法)

Dynamically calculates order sizes based on preset reference order book depths and quantity ratios, combined with specified pricing methods to execute orders efficiently.

```python
#!/usr/bin/env python
# coding=utf-8
__author__ = "Chaos"

from tqsdk import TqApi, TqAuth, TqKq
import math

# === 用户参数 ===
SYMBOL = "SHFE.ag2506"      # 交易合约
DIRECTION = "BUY"          # "BUY"为买入，"SELL"为卖出
OFFSET = "OPEN"       # "OPEN"为开仓，"CLOSE"为平仓,"CLOSETODAY"为平今仓
TOTAL_VOLUME = 20          # 目标总手数
ORDERBOOK_RATIO = 0.2       # 盘口量比比例（如0.2表示20%）
ORDER_TYPE = "对价"         # "对价"为对价报单，"挂价"为挂价报单
ORDERBOOK_TYPE = "对手盘口"  # "对手盘口"或"挂单盘口"

# === 初始化API ===
acc = TqKq()
api = TqApi(account=acc, auth=TqAuth("快期账号", "快期密码"))
quote = api.get_quote(SYMBOL)

# === 初始化变量 ===
traded_volume = 0           # 已成交手数
last_printed_volume = 0     # 上次打印的成交手数
current_order = None        # 当前订单

print(f"盘口下单算法启动，合约: {SYMBOL}，目标: {TOTAL_VOLUME}手，方向: {DIRECTION}，量比比例: {ORDERBOOK_RATIO*100}%")

def get_orderbook_volume():
    """获取盘口数量"""
    if ORDERBOOK_TYPE == "对手盘口":
        # 对手盘口：买入时看卖一量，卖出时看买一量
        return quote.ask_volume1 if DIRECTION == "BUY" else quote.bid_volume1
    else:
        # 挂单盘口：买入时看买一量，卖出时看卖一量
        return quote.bid_volume1 if DIRECTION == "BUY" else quote.ask_volume1

def get_order_price():
    """获取下单价格"""
    if ORDER_TYPE == "对价":
        # 对价报单
        return quote.ask_price1 if DIRECTION == "BUY" else quote.bid_price1
    else:
        # 挂价报单
        return quote.bid_price1 if DIRECTION == "BUY" else quote.ask_price1

try:
    while traded_volume < TOTAL_VOLUME:
        api.wait_update()
        
        # 获取当前盘口数量
        orderbook_volume = get_orderbook_volume()
        # 计算本轮应下单手数
        order_volume = int(math.floor(orderbook_volume * ORDERBOOK_RATIO))
        # 不能超过剩余目标
        order_volume = min(order_volume, TOTAL_VOLUME - traded_volume)
        
        if order_volume > 0:
            print(f"\n当前盘口数量: {orderbook_volume}手")
            print(f"计算下单手数: {orderbook_volume} * {ORDERBOOK_RATIO} = {order_volume}手")
            
            # 下新单
            price = get_order_price()
            current_order = api.insert_order(
                symbol=SYMBOL,
                direction=DIRECTION,
                offset=OFFSET,
                volume=order_volume,
                limit_price=price
            )
            print(f"下单: {order_volume}手，价格: {price}，报单类型: {ORDER_TYPE}")
            
            # 记录上一次的状态和剩余量
            last_status = current_order.status
            last_volume_left = current_order.volume_left
            
            # 等待订单状态更新
            while current_order.status == "ALIVE":
                api.wait_update()
                # 只在状态或剩余量发生变化时打印
                if current_order.status != last_status or current_order.volume_left != last_volume_left:
                    print(f"订单状态更新: {current_order.status}, 剩余量: {current_order.volume_left}")
                    last_status = current_order.status
                    last_volume_left = current_order.volume_left
                
                # 检查价格是否变化
                new_price = get_order_price()
                if new_price != price:
                    print(f"价格发生变化: {price} -> {new_price}")
                    # 如果还有未成交部分，先撤单
                    if current_order.volume_left > 0:
                        print(f"撤单: {current_order.volume_left}手")
                        api.cancel_order(current_order.order_id)
                        # 等待撤单完成
                        while current_order.status == "ALIVE":
                            api.wait_update()
                        # 重新下单
                        current_order = api.insert_order(
                            symbol=SYMBOL,
                            direction=DIRECTION,
                            offset=OFFSET,
                            volume=current_order.volume_left,
                            limit_price=new_price
                        )
                        print(f"重新下单: {current_order.volume_left}手，价格: {new_price}")
                        price = new_price
                        last_status = current_order.status
                        last_volume_left = current_order.volume_left
                    else:
                        # 如果订单已经完成，跳出循环
                        break
            
            # 检查订单是否出错
            if current_order.is_error:
                print(f"下单失败: {current_order.last_msg}")
                break
            
            # 计算实际成交量
            actual_trade = current_order.volume_orign - current_order.volume_left
            if actual_trade > 0:
                print(f"本轮成交: {actual_trade}手")
                traded_volume += actual_trade
                
                # 只在成交手数变化时打印进度
                if traded_volume > last_printed_volume:
                    print(f"当前进度: {traded_volume} / {TOTAL_VOLUME}")
                    last_printed_volume = traded_volume
            else:
                print(f"订单未成交: {current_order.last_msg}")
                
    print("盘口下单算法执行完毕")
except Exception as e:
    print(f"算法执行异常: {e}")
finally:
    api.close()
```

---

---

### 10. Percentage of Volume / POV (跟量下单算法)

Monitors real-time market trading volume changes to dynamically adjust order placement timing and volume. Executes orders according to a specific percentage of market trading volume.

```python
#!/usr/bin/env python
# coding=utf-8
__author__ = "Chaos"

from tqsdk import TqApi, TqAuth, TqKq
import math

# === 用户参数 ===
SYMBOL = "SHFE.ag2506"      # 交易合约
DIRECTION = "BUY"           # "BUY"为买入，"SELL"为卖出
OFFSET = "OPEN"             # "OPEN"为开仓，"CLOSE"为平仓,"CLOSETODAY"为平今仓
TOTAL_VOLUME = 30          # 目标总手数
POV_RATIO = 0.1             # 跟量比例（如0.1表示10%）
ORDER_TYPE = "对价"         # "对价"为对价报单，"报价"为挂价报单

# === 初始化API ===
acc = TqKq()
api = TqApi(account=acc, auth=TqAuth("快期账号", "快期密码"))
quote = api.get_quote(SYMBOL)

# === 初始化变量 ===
base_volume = quote.volume  # 启动时的市场累计成交量
traded_volume = 0           # 已成交手数
last_printed_volume = 0     # 上次打印的成交手数

print(f"POV算法启动，合约: {SYMBOL}，目标: {TOTAL_VOLUME}手，方向: {DIRECTION}，量比比例: {POV_RATIO*100}%")

try:
    while traded_volume < TOTAL_VOLUME:
        api.wait_update()
        new_volume = quote.volume
        delta = new_volume - base_volume
        # 计算本轮应下单手数
        order_volume = int(math.floor(delta * POV_RATIO))
        # 不能超过剩余目标
        order_volume = min(order_volume, TOTAL_VOLUME - traded_volume)
        
        if order_volume > 0:
            print(f"\n市场成交量: {base_volume} -> {new_volume}手")
            print(f"变化量: {delta}手")
            print(f"计算下单手数: {delta} * {POV_RATIO} = {order_volume}手")
            
            # 根据报单类型选择价格
            if ORDER_TYPE == "对价":
                # 对价报单
                price = quote.ask_price1 if DIRECTION == "BUY" else quote.bid_price1
            else:
                # 挂价报单
                price = quote.bid_price1 if DIRECTION == "BUY" else quote.ask_price1
            
            order = api.insert_order(
                symbol=SYMBOL,
                direction=DIRECTION,
                offset=OFFSET,
                volume=order_volume,
                limit_price=price
            )
            print(f"下单: {order_volume}手，价格: {price}，报单类型: {ORDER_TYPE}")
            
            # 记录上一次的状态和剩余量
            last_status = order.status
            last_volume_left = order.volume_left
            
            # 等待订单状态更新
            while order.status == "ALIVE":
                api.wait_update()
                # 只在状态或剩余量发生变化时打印
                if order.status != last_status or order.volume_left != last_volume_left:
                    print(f"订单状态更新: {order.status}, 剩余量: {order.volume_left}")
                    last_status = order.status
                    last_volume_left = order.volume_left
            
            # 检查订单是否出错
            if order.is_error:
                print(f"下单失败: {order.last_msg}")
                break
            
            # 计算实际成交量
            actual_trade = order.volume_orign - order.volume_left
            if actual_trade > 0:
                print(f"本轮成交: {actual_trade}手")
                traded_volume += actual_trade
                base_volume = new_volume  # 更新基准成交量
                
                # 只在成交手数变化时打印进度
                if traded_volume > last_printed_volume:
                    print(f"当前进度: {traded_volume} / {TOTAL_VOLUME}")
                    last_printed_volume = traded_volume
            else:
                print(f"订单未成交: {order.last_msg}")
                
    print("POV算法执行完毕")
except Exception as e:
    print(f"捕获到异常: {e}")
finally:
    api.close()
```

---