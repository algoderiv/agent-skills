# 预计的输出是这样的:
[..., 143.0, 48.0, 80.0, ...]
[..., 95.20000000000005, 92.0571428571429, 95.21428571428575, ...]
```

Example 2 (swift):
```swift
# 获取 CFFEX.IF1903 合约的乖离率
from tqsdk import TqApi, TqAuth, TqSim
from tqsdk.ta import BIAS

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial("CFFEX.IF1903", 24 * 60 * 60)
bias = BIAS(klines, 6)
print(list(bias["bias"]))  # 乖离率

# 预计的输出是这样的:
[..., 2.286835533357118, 2.263301549041151, 0.7068445823271412, ...]
```

Example 3 (swift):
```swift
# 获取 CFFEX.IF1903 合约的布林线
from tqsdk import TqApi, TqAuth, TqSim
from tqsdk.ta import BOLL

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial("CFFEX.IF1903", 24 * 60 * 60)
boll=BOLL(klines, 26, 2)
print(list(boll["mid"]))
print(list(boll["top"]))
print(list(boll["bottom"]))

# 预计的输出是这样的:
[..., 3401.338461538462, 3425.600000000001, 3452.3230769230777, ...]
[..., 3835.083909752222, 3880.677579320277, 3921.885406954584, ...]
[..., 2967.593013324702, 2970.5224206797247, 2982.760746891571, ...]
```

Example 4 (swift):
```swift
# 获取 CFFEX.IF1903 合约的动向指标
from tqsdk import TqApi, TqAuth, TqSim
from tqsdk.ta import DMI

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial("CFFEX.IF1903", 24 * 60 * 60)
dmi=DMI(klines, 14, 6)
print(list(dmi["atr"]))
print(list(dmi["pdi"]))
print(list(dmi["mdi"]))
print(list(dmi["adx"]))
print(list(dmi["adxr"]))

# 预计的输出是这样的:
[..., 95.20000000000005, 92.0571428571429, 95.21428571428575, ...]
[..., 51.24549819927972, 46.55493482309126, 47.14178544636161, ...]
[..., 6.497599039615802, 6.719428926132791, 6.4966241560389655, ...]
[..., 78.80507786697127, 76.8773544355082, 75.11662664555287, ...]
[..., 70.52493837227118, 73.28531799111778, 74.59341569051983, ...]
```

---

