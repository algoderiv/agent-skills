Example 4 (python):
```python
# m1901开多3手
from tqsdk import TqApi, TqAuth
from contextlib import closing

with closing(TqApi(auth=TqAuth("快期账户", "账户密码")) as api:
    api.insert_order(symbol="DCE.m1901", direction="BUY", offset="OPEN", volume=3)
```

---

