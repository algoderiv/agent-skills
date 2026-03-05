# Wondertrader-Rtd - Api

**Pages:** 4

---

## 对接新的交易接口

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/cpp/trader.html

**Contents:**
- 对接新的交易接口

© 版权所有 WonderTrader。 版本 280b46ec.

---

## 策略API详解

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/apis/index.html

**Contents:**
- 策略API详解

© 版权所有 WonderTrader。 版本 280b46ec.

---

## 对接新的行情接口

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/cpp/parser.html

**Contents:**
- 对接新的行情接口
- 行情接口简介
- 行情解析模块结构
- 行情解析模块调用流程
- 行情解析模块开发调试流程
- ParserCTP简单讲解
- 行情解析模块的延伸

顾名思义，行情接口就是用于接入实时行情数据的接口。无论是什么量化框架，行情接入都是一个必不可少的步骤。为了适配不同的行情源，几乎所有的量化框架都会做一级抽象，然后根据不同的行情源，实现不同的行情接口。

WonderTrader作为以C++为核心的量化平台，为了最大化C++语言的速度优势，目前所有的行情源，都采用C++开发的行情接口模块来实现对接。

目前WonderTrader已经对接的行情源如下：

虽然WonderTrader已经覆盖了市面上常见的行情源，但是总是有一些不常见的行情源会需要重新对接。这个时候最好的方式就是自己实现一个行情解析模块。

WonderTrader抽象了一个行情解析器接口类IParserApi，定义如下：

为了解耦行情解析模块与调用模块，还定义了一个回调接口类IParserSpi，定义如下：

行情解析模块就通过IParserSpi和调用者完成交互，具体可以参考WtCore/ParserAdapter的实现，代码片段如下：

前面介绍了行情解析模块涉及到的几个接口的定义，在运行的时候行情解析模块的调用流程如下：

行情解析模块ParserXXX.dll加载

调用createParser接口创建一个IParserApi对象

向IParserApi注册IParserSpi对象指针

调用IParserApi->init传入行情解析模块配置的参数

调用IParserApi->connect开始连接行情源的服务端，并完成账户验证等流程

连接成功以后，触发IParserSpi->handleEvent告知调用者行情接口初始化完成

调用者收到初始化完成的事件通知以后，调用IParserSpi->subscribe订阅所需要的合约列表

订阅成功以后，开始进入持续的行情数据接收流程，收到tick数据以后，调用IParserSpi->handleQuote通知调用者

调用者收到tick行情以后，根据自身的业务逻辑进行处理

前面的流程介绍中，主要围绕tick数据展开，而像股票的Level2行情源，还涉及到逐笔成交、逐笔委托以及委托队列等高频数据。

不同的调用者，实现了IParserSpi的目的是不同的。比如WtDtCore作为数据组件的核心模块，派生了IParserSpi，收到handleQuote回调以后，就会触发IDataWriter进行数据的重采样以及落地文件等操作。而WtCore作为实盘交易的核心模块，收到handleQuote以后主要是向交易引擎的各个策略分发行情数据，驱动策略计算。

前面已经将行情解析模块相关的内容都介绍了，最后我们来看一下，开发的具体流程：

首先根据目标策略复制一个工程，如ParserCTP，并将文件夹和工程名改成自己需要的工程名ParserXXX

然后，修改Parser类的类名为自己需要的类名，如将ParserCTP改成ParserXXX

第三，修改ParserXXX的代码，如引用目标行情源的头文件以及静态库等等，再修改行情解析部分的接口

第四，修改sln解决方案，将新的行情解析模块添加进去(linux下，需要修改CMakeLists.txt)

最后，编译该行情解析模块，生成最终的模块文件ParserXXX.dll(linux为libParserXXX.so)

经过以上步骤，生成的模块就可以进行调试了。可以使用QuoteFactory进行调用调试。模块调试稳定以后，就可以编译release的版本，放到各种环境下使用新的行情解析模块了。

Windows下引用静态库xxxx.lib以后，在行情解析模块加载的时候，会到工作目录下去寻找对应的xxxx.dll，如果动态库不在工作目录下，则会导致行情解析模块加载失败。这个时候，可以采用两种方式来规避这种问题：

在vs工程中配置延迟加载某个dll的编译指令/DELAYLOAD:"xxxx.dll"，然后在代码中显式调用LoadLibrary直接加载指定位置的xxxx.dll

将行情源SDK提供的动态库全部复制到工作目录下

还有一种方式，就是不在编译的时候使用xxxx.lib文件，而采用隐式加载的方式。这种方式相对比较复杂，WonderTrader中很多Parser和Trader都采用这种方式来实现的，这个方式的好处就是不需要xxxx.lib文件，只需要头文件即可编译成功。缺点就是有一些门槛，不熟悉的人运用会比较难。

首先是模块的接口部分，主要是createParser和deleteParser，代码如下：

核心部分还是在如何处理行情数据的逻辑上，代码如下：

总的来说，自定义行情解析模块还是比较简单的，因为一旦连接建立成功，都是接收主推数据，单向推送给调用者，交互相对来说没有那么多。

前面简单介绍了C++实现一个新的行情解析模块的大致流程，实际上在wtpy中还提供了Python版本的扩展行情解析器ExtParser，感兴趣的朋友也可以参考文档ExtParser。

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (javascript):
```javascript
/*
 *	行情解析模块接口
 */
class IParserApi
{
public:
	/*
	 *	初始化解析模块
	 *	@config	模块配置
	 *	返回值	是否初始化成功
	 */
	virtual bool init(WTSVariant* config) { return false; }

	/*
	 *	释放解析模块
	 *	用于退出时
	 */
	virtual void release(){}

	/*
	 *	开始连接服务器
	 *	@返回值	连接命令是否发送成功
	 */
	virtual bool connect() { return false; }

	/*
	 *	断开连接
	 *	@返回值	命令是否发送成功
	 */
	virtual bool disconnect() { return false; }

	/*
	 *	是否已连接
	 *	@返回值	是否已连接
	 */
	virtual bool isConnected() { return false; }

	/*
	 *	订阅合约列表
	 */
	virtual void subscribe(const CodeSet& setCodes){}

	/*
	 *	退订合约列表
	 */
	virtual void unsubscribe(const CodeSet& setCodes){}

	/*
	 *	注册回调接口
	 */
	virtual void registerSpi(IParserSpi* spi) {}
};
```

Example 2 (javascript):
```javascript
/*
 *	行情解析模块回调接口
 */
class IParserSpi
{
public:
	/*
	 *	处理模块事件
	 *	@e	事件类型,如连接、断开、登录、登出
	 *	@ec	错误码,0为没有错误
	 */
	virtual void handleEvent(WTSParserEvent e, int32_t ec){}

	/*
	 *	处理合约列表
	 *	@aySymbols	合约列表,基础元素为WTSContractInfo,WTSArray的用法请参考定义
	 */
	virtual void handleSymbolList(const WTSArray* aySymbols)		= 0;

	/*
	 *	处理实时行情
	 *	@quote		实时行情
	 *	@procFlag	处理标记，0-切片行情，无需处理(ParserUDP)；1-完整快照，需要切片(国内各路通道)；2-极简快照，需要缓存累加（主要针对日线、tick，m1和m5都是自动累加的，虚拟货币行情）
	 */
	virtual void handleQuote(WTSTickData *quote, uint32_t procFlag)	= 0;

	/*
	 *	处理解析模块的日志
	 *	@ll			日志级别
	 *	@message	日志内容
	 */
	virtual void handleParserLog(WTSLogLevel ll, const char* message)	= 0;
};
```

Example 3 (unknown):
```unknown
class ParserAdapter : public IParserSpi,
					private boost::noncopyable
{
    //这里是ParserAdapter的实现
}
```

Example 4 (unknown):
```unknown
extern "C"
{
	EXPORT_FLAG IParserApi* createParser()
	{
		ParserCTP* parser = new ParserCTP();
		return parser;
	}

	EXPORT_FLAG void deleteParser(IParserApi* &parser)
	{
		if (NULL != parser)
		{
			delete parser;
			parser = NULL;
		}
	}
};
```

---

## 策略API详解

**URL:** https://wtdocs.readthedocs.io/zh/latest/docs/apis/interfaces.html

**Contents:**
- 策略API详解
- 接口概述
- 基础数据接口
- 交易数据接口
- 信号接口
- 其他接口

策略API接口主要分成4类：基础数据接口、交易数据接口、信号接口和其他接口。

交易数据接口 主要用于获取行情相关数据和交易数据

需要注意的是这里的时间指的是当前所属的1分钟K线对应的时间，而WonderTrader里对分钟线的处理，是以尾部时间为准。如9:30:15，这个时候调用该接口，获取到的时间为931。

调用API输出日志会记录在日志文件中，系统会自动根据策略ID生成日志文件，这样用户不用自己搭建日志模块，就可以轻松管理好策略的日志。

如果策略内有连续多次开仓的逻辑，且需要知道某个特定进场信号的价格，则可以调用该接口。如果没有连续开仓的逻辑，但是需要知道最后一次进场信号的价格，也可以使用该接口。

如果策略内有连续多次开仓的逻辑，且需要知道某个特定进场信号的分钟线时间，则可以调用该接口。如果没有连续开仓的逻辑，但是需要知道最后一次进场信号的分钟线时间，也可以使用该接口。

如果策略内有连续多次开仓的逻辑，且需要知道某个特定进场信号的持仓盈亏，则可以调用该接口。如果没有连续开仓的逻辑，但是需要知道最后一次进场信号的持仓盈亏，也可以使用该接口。 flag的意义重点在于风控，最大浮盈和最大亏损都是该笔进场的最大潜在盈利和最大潜在亏损，很多策略可以通过该数据确定一些风控的逻辑。

逐笔数据进来时调用 生产环境中，每笔行情进来就直接调用；回测环境中，是模拟的逐笔数据。

进场接口有一个需要注意的地方，就是当持仓头寸为反向时，进场接口要先平掉反向头寸，再开仓到参数中指定的仓位，即qty手。这样的好处就是方便用户在开发策略的时候，只需要关心目的，而不需要考虑过程，简化了代码逻辑，还减少了出错的可能。 limitprice是下单的时候设置一个限价触发条件，而stopprice则是设置一个止价触发条件。当条件满足的时候，才会产生信号。另外，limitprice和stopprice为0时，则不会设置限价单和止价单，但是如果同时不为0的时候，只有限价单生效。

出场接口用法和入场接口相同。但是出场也有一个需要注意的地方：如果持仓方向和出场方向不一致，即持有空头，而调用多头出场，系统将不会执行；如果持仓数量不够时，全部出场以后也不会再反向开仓。

除了上述几个信号接口，wtpy还提供了一个非常直接的信号接口：即直接设置目标部位。用户可以不用关心多头空头进场出场的细节问题，只需要直接设置目标仓位即可。

主K线闭合时调用，一般作为策略的核心计算模块。

重算结束回调，只有在异步模式下才会触发，目前主要针对强化学习的训练场景，需要在重算以后将智能体的信号传递给底层

© 版权所有 WonderTrader。 版本 280b46ec.

**Examples:**

Example 1 (python):
```python
def stra_get_comminfo(self, stdCode:str):
```

Example 2 (unknown):
```unknown
stdCode    品种代码，如CFFEX.IF.HOT
返回值  ProductInfo对象，可以参考ProductMgr.py模块
```

Example 3 (python):
```python
def stra_get_sessinfo(self, stdCode:str):
```

Example 4 (unknown):
```unknown
stdCode   合约代码如SHFE.ag.HOT，或者品种代码如SHFE.ag
返回值     品种信息，结构请参考SessionMgr中的SessionInfo
```

---
