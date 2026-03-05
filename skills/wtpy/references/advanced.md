# Wondertrader-Rtd - Advanced

**Pages:** 8

---

## 自定义执行单元

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/cpp/excunit.html

**Contents:**
- 自定义执行单元

© 版权所有 WonderTrader。 版本 280b46ec.

---

## C++开发

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/cpp/index.html

**Contents:**
- C++开发

© 版权所有 WonderTrader。 版本 280b46ec.

---

## 扩展行情解析器ExtParser

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/levelup/extpaser.html

**Contents:**
- 扩展行情解析器ExtParser
- ExtParser是什么
- ExtParser怎么用

WonderTrader全部底层组件都采用C++开发，即使使用wtpy，其实也是作为一个入口，调用底层组件进行回测交易。所以如果想要对WonderTrader做二次开发，对于使用者的C++水平是有较高的要求的。一方面需要熟练运用C++实现业务逻辑，另外一方面还需要有足够的经验解决C++中遇到的各种编程和设计的问题。

可是在实践中，我们还是会遇到很多场景下，需要自己实现行情的接入。但是如果一定要从C++底层开发新的行情接入模块，可行性也不是特别高：

一方面，很难要求每个使用者都有非常高的C++开发水平

另一方面，有一些柜台系统运行模式偏互联网风格，这种柜台系统一般会提供多种不同类型的接口，如http、websocket，数据包的封装，也采用互联网行业较流行的格式，如json等，而这类接口的形式，如果直接采用C++实现，开发效率相对较低，而且如果接口调整的时候，C++底层修改不够方便

ExtParser的目标，就是允许用户在wtpy中实现一个python版本的行情接入模块，通过在python中直接分配C++兼容的数据结构内存块，传递给底层直接使用，避免二次转码，降低开销，提升解析效率。

语言更灵活，可以很容易解析json、xml等各种数据格式

低延时场景无法使用 主要还是因为Python运行效率相对较低，而且还有GIL的问题，所以高并发、大数据量、低延时的场景下，都应该慎用ExtParser。

要使用ExtParser，只需要在Python中实现一个BaseExtParser，并传递给WtDtEngine，就可以使用了，代码如下：

wtpy/demos/datakit_fut/testExtParser.py

wtpy/demos/datakit_allday

wtpy/demos/test_extmodules

如果考虑使用C++实现新的行情解析模块，也可以参考文档对接新的行情接口。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
from wtpy import BaseExtParser
from wtpy import WTSTickStruct
from ctypes import byref
import threading
import time

from wtpy import WtDtEngine

class MyParser(BaseExtParser):
    def __init__(self, id: str):
        super().__init__(id)
        self.__worker__ = None

    def init(self, engine:WtEngine):
        '''
        初始化
        '''
        super().init(engine)

    def random_sim(self):
        while True:
            curTick = WTSTickStruct()
            curTick.code = bytes("IF2106", encoding="UTF8")
            curTick.exchg = bytes("CFFEX", encoding="UTF8")

            self.__engine__.push_quote_from_extended_parser(self.__id__, byref(curTick), True)
            time.sleep(1)


    def connect(self):
        '''
        开始连接
        '''
        print("connect")
        if self.__worker__ is None:
            self.__worker__ = threading.Thread(target=self.random_sim, daemon=True)
            self.__worker__.start()
        return

    def disconnect(self):
        '''
        断开连接
        '''
        print("disconnect")
        return

    def release(self):
        '''
        释放，一般是进程退出时调用
        '''
        print("release")
        return

    def subscribe(self, fullCode:str):
        '''
        订阅实时行情\n
        @fullCode   合约代码，格式如CFFEX.IF2106
        '''
        # print("subscribe: " + fullCode)
        return

    def unsubscribe(self, fullCode:str):
        '''
        退订实时行情\n
        @fullCode   合约代码，格式如CFFEX.IF2106
        '''
        # print("unsubscribe: " + fullCode)
        return


if __name__ == "__main__":
    #创建一个运行环境，并加入策略
    myParser = MyParser("test")
    engine = WtDtEngine()
    engine.initialize("dtcfg.yaml", "logcfgdt.yaml")
    engine.add_exetended_parser(myParser)
    engine.run()
    kw = input('press any key to exit\n')
```

---

## 扩展执行器ExtExecuter

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/levelup/extexec.html

**Contents:**
- 扩展执行器ExtExecuter
- ExtExecuter是什么
- ExtExecuter怎么用

WonderTrader全部底层组件都采用C++开发，即使使用wtpy，其实也是作为一个入口，调用底层组件进行回测交易。所以如果想要对WonderTrader做二次开发，对于使用者的C++水平是有较高的要求的。一方面需要熟练运用C++实现业务逻辑，另外一方面还需要有足够的经验解决C++中遇到的各种编程和设计的问题。

可是在实践中，我们还是会遇到很多场景下，需要自己实现交易接口。但是如果一定要从C++底层开发新的交易接口模块，可行性也不是特别高：

首先，很难要求每个使用者都有非常高的C++开发水平

其次，有一些柜台系统运行模式偏互联网风格，这种柜台系统一般会提供多种不同类型的接口，如http、websocket，数据包的封装，也采用互联网行业较流行的格式，如json等，而这类接口的形式，如果直接采用C++实现，开发效率相对较低，而且如果接口调整的时候，C++底层修改不够方便

再次，在股票交易交易中，有很多交易终端以文件扫单的方式提供下单接口，而这种方式只需要用户在使用的时候按照指定的规则提供一个下单文件就可以了

ExtExecuter的目标，就是允许用户在wtpy中实现一个python版本的信号执行模块，C++底层会通过接口直接向ExtExecuter传递指定标的的目标仓位，ExtExecuter就可以根据自身适配的交易接口，灵活的实现底层信号的传递。

ExtExecuter和C++底层实现的执行器互相不冲突，可以同时使用

要使用ExtExecuter，只需要在交易引擎中注册一个自己扩展的执行器模块，交易引擎就会自动调用该执行器，代码如下：

更多详情可以参考demo: wtpy/demos/test_extmodules

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
from wtpy import  BaseExtExecuter

from wtpy import WtEngine,EngineType
from Strategies.DualThrust import StraDualThrust

class MyExecuter(BaseExtExecuter):
    def __init__(self, id: str, scale: float):
        super().__init__(id, scale)

    def init(self):
        print("inited")

    def set_position(self, stdCode: str, targetPos: float):
        print("position confirmed: %s -> %f " % (stdCode, targetPos))

if __name__ == "__main__":
    #创建一个运行环境，并加入策略
    engine = WtEngine(EngineType.ET_CTA)
    engine.init('../common/', "config.yaml")
    
    straInfo = StraDualThrust(name='pydt_au', code="SHFE.au.HOT", barCnt=50, period="m5", days=30, k1=0.2, k2=0.2, isForStk=False)
    engine.add_cta_strategy(straInfo)
    
    myExecuter = MyExecuter('exec', 1)
    engine.commitConfig()
    engine.add_exetended_executer(myExecuter)

    engine.run()

    kw = input('press any key to exit\n')
```

---

## 自定义数据存储模块

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/cpp/data.html

**Contents:**
- 自定义数据存储模块

© 版权所有 WonderTrader。 版本 280b46ec.

---

## C++开发环境搭建

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/cpp/devenv.html

**Contents:**
- C++开发环境搭建
- 前言
- 共享资源
- Linux本地开发环境搭建
  - 1、系统准备
  - 2、安装开发工具
  - 3、从共享资源中下载mydes_gcc8.4.0.7z并上传到linux
  - 4、将mydes_gcc8.4.0.7z解压到/home下
  - 5、拉取WonderTrader的源码
  - 6、进入src目录，执行编译脚本

WonderTrader开源以来，虽然没有正式做过推广，但是逐渐有了第一批吃螃蟹的用户。在此WonderTrader团队对这些用户深表感谢，希望WonderTrader能够获得大家更多的认可。 在用户的反馈中，我们逐渐发现，除了直接使用wtpy进行回测和交易的用户，还有一批对源码有直接需求的用户。他们在使用过程中遇到的最大问题是如何编译源码。WonderTrader的官方文档中，对开发环境做了简单的介绍，但是并不深入，所以这些用户在搭建开发环境的过程中遇到了不少问题。

本文的主要目的就是帮助用户解决WonderTrader的源码的编译问题。

因为笔者长期在开发一线，平时在工作中会用到很多方便易用的工具，WonderTrader的开发中也有涉及。为了方便大家下载使用，本文中涉及到的一些好用的工具，笔者都统一放到百度云盘里共享出来，大家可以根据需要自行下载。文中会有多出涉及到从共享资源中下载文件，后面就不再赘述，统一用共享资源代称。

下载连接:https://pan.baidu.com/s/1Bdxh_PgjqHMzuGjl9ernhg

WonderTrader官方的Linux开发环境为Ubuntu Server 18.04 LTS，可以使用WSL、虚拟机或者Docker进行系统初始化的工作。

WSL可以直接在应用商店，安装Ubuntu 18.04.6 LTS，如下图：

如果使用Docker，可以直接使用以下命令拉取ubuntu:18.04的镜像：

直接执行以下命令，安装gcc8，ubuntu 18.04，对应的gcc8的版本为gcc8.4.0

由于Ubuntu 18.04默认安装的gcc版本是7.5.0，安装cmake的时候会自动安装gcc7.5.0，所以我们还需要对gcc命令符号做一个处理

使用cmake --version查看cmake的版本号

如果是通过terminal工具连接的话，推荐使用WindTerm上传

如果是WSL的话，可以直接访问Windows的文件系统

编译完成以后，就可以直接从src目录下的build/bin中下载需要的文件即可。

如果要将linux下编译的二进制文件，复制到wtpy中，还可以使用文件复制脚本copy_bins_linux.sh，命令行如下：

首先下载vs2017的安装器，下载地址如下： https://visualstudio.microsoft.com/zh-hans/vs/older-downloads/

从共享资源中下载环境变量编辑器RapidEE到本地，解压以后运行。 新建一个名为MyDepends141的环境变量，并将值设置为依赖库解压目录E:/vc141depends

设置好了以后，保存环境变量，即可生效，如果没有生效，可以重启电脑。win10下要注意一下使用管理员身份运行RapidEE，不然保存可能会失败！

下载git并安装：https://git-scm.com/download/win

下载tortoisegit并安装：https://tortoisegit.org/download/

选择一个目录，然后右键点击该目录，选择“Git克隆”菜单

URL设置为:https://github.com/wondertrader/wondertrader.git

进入到wondertrader的src目录下

双击运行wondertrader.sln，打开以后在解决方案上右键点击，选择“生成解决方案”就进入了编译过程

进入到wondertrader目录，双击运行copy_bins_win.bat批处理文件，就可以自动将编译好的二进制文件复制到wtpy对应的目录下了，运行截图如下：

WonderTrader的0.9.8版本，将C++标准升级到了C++17。主要因为0.9.8版本，引入了一个新的hash容器ankerl::unordered_dense，该容器相比以前的robin_map，性能可以提升1/3左右，但是该容器必须要求C++17以上的标准。 鉴于此，WonderTrader的标准也升级到了C++17。但是原来的编译环境Centos7 + gcc 4.8.5，只支持C++11，gcc8以上才支持C++17。经过测试，Centos7下的的gcc8的版本为gcc8.3.1，该版本下编译还是不成功，而Ubuntu 18.04下的gcc8为gcc8.4.0，该版本下就能够编译成功。 由于gcc版本的要求，开发环境搭建会变得更加复杂，为了方便大家使用，就提供了内置的Dockerfile。 在wondertrader/docker下，有两个Dockerfile：

这个dockerfile直接拉取dockerhub上提交的wondertrader镜像，镜像大小约500M

wondertrader镜像，基于ubuntu18.04进行构建，安装了gcc8.4.0等编译工具

基于该文件构建镜像可以使用命令： docker build -t yourimagename -f Dockerfile .

构建完成以后，要启动容器，可以使用命令： docker run -it yourimagename /bin/bash

这个dockerfile直接基于ubuntu18.04镜像构建，镜像大小约66M，拉取速度较快

拉取完以后，再安装gcc8.4.0等编译工具

基于该文件构建镜像可以使用命令：docker build -t yourimagename -f Dockerfile_ubt .

构建完成以后，要启动容器，可以使用命令：docker run -it yourimagename /bin/bash

用户可以根据自己的需要，通过这两个Dockerfile直接构建自己的开发镜像，命令行如下：

本地镜像构建好了，就可以使用以下命令运行容器了

将目录转到/home/wondertrader/src，执行编译脚本即可

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (unknown):
```unknown
$ docker pull ubuntu:18.04
```

Example 2 (unknown):
```unknown
$ docker run -it ubuntu:18.04 /bin/bash
```

Example 3 (unknown):
```unknown
# 安装编译工具
$ apt-get install -y git 
$ apt-get install -y gcc-8 
$ apt-get install -y g++-8 
$ apt-get install -y cmake
```

Example 4 (unknown):
```unknown
# 将gcc符号链接到gcc8
$ rm /usr/bin/gcc
$ ln -s /usr/bin/gcc-8 /usr/bin/gcc
$ ln -s /usr/bin/g++-8 /usr/bin/g++
```

---

## WtDataHelper

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/tools/wtdthelper.html

**Contents:**
- WtDataHelper
- WtDatahelper是什么
- WtDataHelper怎么用

作为一个内核使用C++开发的量化交易框架，WonderTrader是天然追求更高的性能的。作为一个量化交易框架，如果高效地读写数据就成了追求高性能的必须要解决的问题。

WonderTrader采用自定义的数据结构对文件进行存储，为了提高访问性能，实时数据直接采用不原始数据结构连续内存排列的方式存储数据，而历史数据则采用zstd进行压缩存放，在读取的时候做一次解压就可以全部加载到内存中。采用这样的机制，WonderTrader在数据读写的性能上可以达到非常高的效率。

我们在使用的过程中，新的数据可以通过datakit来落地，历史数据我们也可以通过WtDHFacor来拉取。但是WonderTrader毕竟不可能覆盖全部的数据源，另外在做数据维护和管理的时候，也会面临着检查指定数据文件的场景。

WtDataHelper就是为了给更多场景下的数据读写提供灵活的访问接口的。WtDataHelper主要提供的就是数据文件的读写能力。具体如下：

WtDataHelper的使用相对简单，就是调用接口读写文件即可，下面是代码示例：

更多详情可以参考demo: wtpy/demos/test_datahelper

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
class WtDataHelper:

    def dump_bars(self, binFolder:str, csvFolder:str, strFilter:str=""):
        '''
        将目录下的.dsb格式的历史K线数据导出为.csv格式
        @binFolder  .dsb文件存储目录
        @csvFolder  .csv文件的输出目录
        @strFilter  代码过滤器(暂未启用)
        '''
        pass

    def dump_ticks(self, binFolder: str, csvFolder: str, strFilter: str=""):
        '''
        将目录下的.dsb格式的历史Tik数据导出为.csv格式
        @binFolder  .dsb文件存储目录
        @csvFolder  .csv文件的输出目录
        @strFilter  代码过滤器(暂未启用)
        '''
        pass

    def trans_csv_bars(self, csvFolder: str, binFolder: str, period: str):
        '''
        将目录下的.csv格式的历史K线数据转成.dsb格式
        @csvFolder  .csv文件的输出目录
        @binFolder  .dsb文件存储目录
        @period     K线周期，m1-1分钟线，m5-5分钟线，d-日线
        '''
        pass

    def read_dsb_ticks(self, tickFile: str) -> WtTickRecords:
        '''
        读取.dsb格式的tick数据
        @tickFile   .dsb的tick数据文件
        @return     WtTickRecords
        '''
        pass


    def read_dsb_bars(self, barFile: str, isDay:bool = False) -> WtBarRecords:
        '''
        读取.dsb格式的K线数据
        @tickFile   .dsb的K线数据文件
        @return     WtBarRecords
        '''
        pass

    def read_dmb_ticks(self, tickFile: str) -> WtTickRecords:
        '''
        读取.dmb格式的tick数据
        @tickFile   .dmb的tick数据文件
        @return     WTSTickStruct的list
        '''
        pass

    def read_dmb_bars(self, barFile: str) -> WtBarRecords:
        '''
        读取.dmb格式的K线数据
        @tickFile   .dmb的K线数据文件
        @return     WTSBarStruct的list
        '''
        pass

    def store_bars(self, barFile:str, firstBar:POINTER(WTSBarStruct), count:int, period:str) -> bool:
        '''
        将K线转储到dsb文件中
        @barFile    要存储的文件路径
        @firstBar   第一条bar的指针
        @count      一共要写入的数据条数
        @period     周期，m1/m5/d
        '''
        pass

    def store_ticks(self, tickFile:str, firstTick:POINTER(WTSTickStruct), count:int) -> bool:
        '''
        将Tick数据转储到dsb文件中
        @tickFile   要存储的文件路径
        @firstTick  第一条tick的指针
        @count      一共要写入的数据条数
        '''
        pass

    def resample_bars(self, barFile:str, period:str, times:int, fromTime:int, endTime:int, sessInfo:SessionInfo) -> WtBarRecords:
        '''
        重采样K线
        @barFile    dsb格式的K线数据文件
        @period     基础K线周期，m1/m5/d
        @times      重采样倍数，如利用m1生成m3数据时，times为3
        @fromTime   开始时间，日线数据格式yyyymmdd，分钟线数据为格式为yyyymmddHHMMSS
        @endTime    结束时间，日线数据格式yyyymmdd，分钟线数据为格式为yyyymmddHHMMSS
        @sessInfo   交易时间模板
        '''
        pass
```

Example 2 (python):
```python
from wtpy.wrapper import WtDataHelper
from wtpy.WtCoreDefs import WTSBarStruct, WTSTickStruct
from ctypes import POINTER
from wtpy.SessionMgr import SessionMgr
import pandas as pd

dtHelper = WtDataHelper()

def test_store_bars():
    df = pd.read_csv('../storage/csv/CFFEX.IF.HOT_m5.csv')
    df = df.rename(columns={
        '<Date>':'date',
        ' <Time>':'time',
        ' <Open>':'open',
        ' <High>':'high',
        ' <Low>':'low',
        ' <Close>':'close',
        ' <Volume>':'vol',
        })
    df['date'] = df['date'].astype('datetime64').dt.strftime('%Y%m%d').astype('int64')
    df['time'] = (df['date']-19900000)*10000 + df['time'].str.replace(':', '').str[:-2].astype('int')

    BUFFER = WTSBarStruct*len(df)
    buffer = BUFFER()

    def assign(procession, buffer):
        tuple(map(lambda x: setattr(buffer[x[0]], procession.name, x[1]), enumerate(procession)))


    df.apply(assign, buffer=buffer)
    print(df)
    print(buffer[0].to_dict)
    print(buffer[-1].to_dict)

    dtHelper.store_bars(barFile="./CFFEX.IF.HOT_m5.bin", firstBar=buffer, count=len(df), period="m5")
    
def test_store_ticks():

    df = pd.read_csv('../storage/csv/rb主力连续_20201030.csv')
    BUFFER = WTSTickStruct*len(df)
    buffer = BUFFER()

    tags = ["一","二","三","四","五"]

    for i in range(len(df)):
        curTick = buffer[i]

        curTick.exchg = b"SHFE"
        curTick.code = b"SHFE.rb.HOT"

        curTick.price = float(df[i]["最新价"])
        curTick.open = float(df[i]["今开盘"])
        curTick.high = float(df[i]["最高价"])
        curTick.low = float(df[i]["最低价"])
        curTick.settle = float(df[i]["本次结算价"])
        
        curTick.total_volume = float(df[i]["数量"])
        curTick.total_turnover = float(df[i]["成交额"])
        curTick.open_interest = float(df[i]["持仓量"])

        curTick.trading_date = int(df[i]["交易日"])
        curTick.action_date = int(df[i]["业务日期"])
        curTick.action_time = int(df[i]["最后修改时间"].replace(":",""))*1000 + int(df[i]["最后修改毫秒"])

        curTick.pre_close = float(df[i]["昨收盘"])
        curTick.pre_settle = float(df[i]["上次结算价"])
        curTick.pre_interest = float(df[i]["昨持仓量"])

        for x in range(5):
            setattr(curTick, f"bid_price_{x}", float(df[i]["申买价"+tags[x]]))
            setattr(curTick, f"bid_qty_{x}", float(df[i]["申买量"+tags[x]]))
            setattr(curTick, f"ask_price_{x}", float(df[i]["申卖价"+tags[x]]))
            setattr(curTick, f"ask_qty_{x}", float(df[i]["申卖量"+tags[x]]))

    dtHelper.store_ticks(tickFile="./SHFE.rb.HOT_ticks.dsb", firstTick=buffer, count=len(df))

def test_resample():
    # 测试重采样
    sessMgr = SessionMgr()
    sessMgr.load("sessions.json")
    sInfo = sessMgr.getSession("SD0930")
    ret = dtHelper.resample_bars("IC2009.dsb",'m1',5,202001010931,202009181500,sInfo)
    print(ret)
```

---

## 使用进阶

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/levelup/index.html

**Contents:**
- 使用进阶

© 版权所有 WonderTrader。 版本 280b46ec.

---
