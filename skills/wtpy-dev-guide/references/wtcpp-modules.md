# Wtpy-Github - Wtcpp

**Pages:** 23

---

## QuoteFactory模块(中泰XTP)

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder50/file01.html

**Contents:**
- QuoteFactory模块(中泰XTP)
- 执行程序配置
  - dtcfg.yaml
  - mdparsers.yaml
- 调试配置
- QuoteFactory main.cpp主要代码解析

source: wtcpp/folder50/file01.md

说明：行情接口为中泰证券的XTP，该接口为市场常用，WT已经实现了它的parser，开发环境为WIndows。

按如下的配置说明进行配置即可以使用QuoteFactory.exe来接收行情，作者建议早上9点15分启动，下午4点半关闭。 首先是配置文件，以下的文件要放在QuoteFactory可执行文件的同目录下，具体包括dtcfg.yaml、mdparsers.yaml、logcfgdt.yaml和statemonitor.yaml，这些配置文件都可以从WT主目录下的dist/QuoteFactory文件夹下复制到，其中要修改的是dtcfg.yaml和mdparsers.yaml两个文件，具体如下，配置项如果没有特别说明的则可以直接复制粘贴。

**Examples:**

Example 1 (unknown):
```unknown
basefiles:
    # 这下面对应的四个文件要在../common/下面放好，哪个不需要放目前还没确定过，但holidays.json和stk_sessions.json是确定需要的，stk_sessions.json文件缺失的话会导致服务没法保存行情数据，这些文件可以从wtpy的dev分支下的demos/commons找到。
    commodity: ../common/stk_comms.json
    contract: ../common/stocks.json
    holiday: ../common/holidays.json
    session: ../common/stk_sessions.json
# 顶点广播配置，所以本机WTRunner接收数据的时候可以采用ParserUDP通过UDP获取数据，但win11调试的时候会提示权限不足（调试的时候可以换成其他Parser），但exe运行的时候可以用管理员运行解决该问题
broadcaster:
    active: true
    bport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
parsers: mdparsers.yaml
# statemonitor.yaml定义的历史数据的处理时间
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 20
    # 行情数据的保存目录
    path: ../STK_Data
    savelog: false
```

Example 2 (unknown):
```unknown
basefiles:
    # 这下面对应的四个文件要在../common/下面放好，哪个不需要放目前还没确定过，但holidays.json和stk_sessions.json是确定需要的，stk_sessions.json文件缺失的话会导致服务没法保存行情数据，这些文件可以从wtpy的dev分支下的demos/commons找到。
    commodity: ../common/stk_comms.json
    contract: ../common/stocks.json
    holiday: ../common/holidays.json
    session: ../common/stk_sessions.json
# 顶点广播配置，所以本机WTRunner接收数据的时候可以采用ParserUDP通过UDP获取数据，但win11调试的时候会提示权限不足（调试的时候可以换成其他Parser），但exe运行的时候可以用管理员运行解决该问题
broadcaster:
    active: true
    bport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
parsers: mdparsers.yaml
# statemonitor.yaml定义的历史数据的处理时间
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 20
    # 行情数据的保存目录
    path: ../STK_Data
    savelog: false
```

Example 3 (unknown):
```unknown
parsers:
-   active: true
    buffsize: 128
    clientid: 1
    #你想要订阅的股票代码，不做code的配置则认为是订阅全市场代码    code: SSE.000001,SSE.600009,SSE.600036,SSE.600276,SZSE.000001
    hbinterval: 15
    host: xtp行情服务地址
    id: parser
    module: ParserXTP
    pass: 你的xtp密码
    port: xtp行情服务端口
    protocol: 1
    user: 你的xtp用户名
```

Example 4 (unknown):
```unknown
parsers:
-   active: true
    buffsize: 128
    clientid: 1
    #你想要订阅的股票代码，不做code的配置则认为是订阅全市场代码    code: SSE.000001,SSE.600009,SSE.600036,SSE.600276,SZSE.000001
    hbinterval: 15
    host: xtp行情服务地址
    id: parser
    module: ParserXTP
    pass: 你的xtp密码
    port: xtp行情服务端口
    protocol: 1
    user: 你的xtp用户名
```

---

## CTA仿真完整篇4: 配置文件

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file12.html

**Contents:**
- CTA仿真完整篇4: 配置文件
- 修改配置文件
- 初始化
  - 信号过滤器 filters.yaml
  - 手续费 common->fees.json
  - 风控管理 config.ymal->riskmon
  - 开平仓管理 actpolicy.yaml
  - 流量风控 tdtraders.yaml->riskmo
- 运行
  - 流量风控

source: wtcpp/folder03/file12.md

本章内容将讲述5个配置文件相关内容, 主要包括

书接上文, 首先打开"QuoteFactory.exe", 然后将断点打在"WtRunner.cpp"下的 initEngine() 函数内的 _cta_engine.init, 启动程序

进入 WtCtaEngine::init, 执行 WtEngine::init, 进入 WtEngine::init

开平仓管理和上述三个配置文件初始化地方不大一致. 将断点打在 WtRunner::initActionPolicy 下.

1.进入 ActionPolicyMgr::init, 所有开平规则和ID映射存放在 _rules 中, 品种名称和ID映射存放在 _comm_rule_map 中.

同时还包括四种动作类型(action 字段)

流量风控的配置隐藏在交易端口内. 将断点打在 WtRunner::initTraders 下的 adapter->init

进入 TraderAdapter::init, 可以看到"解析流量风控参数"部分, 对应着 "tdtraders.yaml" 配置文件下的 riskmon 部分, 所有配置内容保存在 _risk_params_map 中.

流量风控主要检查下单和取消订单的限制. 它的调用包含在 TraderAdapter::checkOrderLimits 和 TraderAdapter::checkCancelLimits 中, 因此每次下单和撤单前, 只要调用这两个方法, 都会去检查流量控制.

下单流程很长, 但真正的下单动作只在最后几步, 回看前一篇文章, 下单动作进入到 WtLocalExecuter::set_position 这一步时, 就会调用 checkOrderLimits.

风控管理会启动单独的线程. 将断点打在 "WtCtaEngine.cpp" 下的 _risk_mon->self()->run();, 进入 WtSimpleRiskMon::run()

这里包含了风控管理的所有逻辑, 当风控触发时, 会修改引擎中的 _risk_volscale 参数, 每次下单前都会对 _risk_volscale 做检查, 以此达到风控目的.

打开 "WtCtaEngine.cpp" 直接 Ctrl+f 搜索 "_filter" 就可以看到那些地方调用了信号过滤器, 除了初始化部分, 在 WtCtaEngine::on_schedule 和 WtCtaEngine::handle_pos_change 执行时会调用.

然后打开 "CtaStraBaseCtx", 搜索 handle_pos_change 即可知道信号过滤器的调用过程.

打开 "WtEngine.cpp" 直接 Ctrl+f 搜索 "_fee_map" 就可以看到那些地方计算手续费, 除了初始化部分, 在 WtEngine::calc_fee 执行时会计算.

然后打开 "CtaStraBaseCtx", 搜索 calc_fee, 就知道下单过程中那些步骤会计算手续费了.

打开 "TraderAdapter.cpp" 直接 Ctrl+f 搜索 "_policy" 就可以看到那些地方调用该方法, 除了初始化部分, 在 TraderAdapter::sell 和 TraderAdapter::buy 执行时会调用.

流量风控和开平仓管理都在 "TraderAdapter.cpp" 中被调用, 风控管理和信号过滤器主要在 "WtCtaEngine.cpp" 中被调用, 手续费主要在 "WtEngine.cpp" 中计算.

因为上一章内容讲述了下单流程, 所以这一些配置的调用过程没有按照流程表述, 而是看起来很零碎. 建议两章内容互相查看, 更容易理解.

**Examples:**

Example 1 (unknown):
```unknown
#基础配置文件
basefiles:
    commodity: ../common/commodities.json   #品种列表
    contract: ../common/contracts.json      #合约列表
    holiday: ../common/holidays.json        #节假日列表
    hot: ../common/hots.json                #主力合约映射表
    session: ../common/sessions.json        #交易时间模板
#数据存储
data:
    store:
        path: ../FUT_Data/      #数据存储根目录

#环境配置
env:
    name: cta               #引擎名称：cta/hft/sel
    product:
        session: ALLDAY    #驱动交易时间模板，TRADING是一个覆盖国内全部交易品种的最大的交易时间模板，从夜盘21点到凌晨1点，再到第二天15:15，详见sessions.json
    filters: filters.yaml       #过滤器配置文件，这个主要是用于盘中不停机干预的
    fees: ../common/fees.json   #佣金配置文件
    riskmon:                #组合风控设置
        active: true            #是否开启
        module: WtRiskMonFact   #风控模块名，会根据平台自动补齐模块前缀和后缀
        name: SimpleRiskMon     #风控策略名，会自动创建对应的风控策略
        #以下为风控指标参数，该风控策略的主要逻辑就是日内和多日的跟踪止损风控，如果回撤超过阈值，则降低仓位
        base_amount: 5000000    #组合基础资金，WonderTrader只记录资金的增量，基础资金是用来模拟组合的基本资金用的，和增量相加得到动态权益
        basic_ratio: 101        #日内高点百分比，即当日最高动态权益是上一次的101%才会触发跟踪侄止损
        calc_span: 5            #计算时间间隔，单位s
        inner_day_active: true  #日内跟踪止损是否启用
        inner_day_fd: 20.0      #日内跟踪止损阈值，即如果收益率从高点回撤20%，则触发风控
        multi_day_active: false #多日跟踪止损是否启用
        multi_day_fd: 60.0      #多日跟踪止损阈值
        risk_scale: 0.3         #风控系数，即组合给执行器的目标仓位，是组合理论仓位的0.3倍，即真实仓位是三成仓
        risk_span: 30           #风控触发时间间隔，单位s。因为风控计算很频繁，如果已经触发风控，不需要每次重算都输出风控日志，加一个时间间隔，友好一些

strategies:
    # CTA策略配置，当mocker为cta时会读取该配置项
    cta:
    -   active: true       # 模块名，linux下为xxxx.so
        id: dt_if          # 策略ID，自定义的
        name: WtCtaStraFact.DualThrust   # 策略名，要和factory中的匹配
        params:                         # 策略初始化参数，这个根据策略的需要提供
            code: SHFE.au.2206
            count: 1
            days: 30
            k1: 0.6
            k2: 0.6
            period: m1
            stock: false

executers: executers.yaml   #执行器配置文件
parsers: tdparsers.yaml     #行情通达配置文件
traders: tdtraders.yaml     #交易通道配置文件
bspolicy: actpolicy.yaml    #开平策略配置文件
```

Example 2 (unknown):
```unknown
#基础配置文件
basefiles:
    commodity: ../common/commodities.json   #品种列表
    contract: ../common/contracts.json      #合约列表
    holiday: ../common/holidays.json        #节假日列表
    hot: ../common/hots.json                #主力合约映射表
    session: ../common/sessions.json        #交易时间模板
#数据存储
data:
    store:
        path: ../FUT_Data/      #数据存储根目录

#环境配置
env:
    name: cta               #引擎名称：cta/hft/sel
    product:
        session: ALLDAY    #驱动交易时间模板，TRADING是一个覆盖国内全部交易品种的最大的交易时间模板，从夜盘21点到凌晨1点，再到第二天15:15，详见sessions.json
    filters: filters.yaml       #过滤器配置文件，这个主要是用于盘中不停机干预的
    fees: ../common/fees.json   #佣金配置文件
    riskmon:                #组合风控设置
        active: true            #是否开启
        module: WtRiskMonFact   #风控模块名，会根据平台自动补齐模块前缀和后缀
        name: SimpleRiskMon     #风控策略名，会自动创建对应的风控策略
        #以下为风控指标参数，该风控策略的主要逻辑就是日内和多日的跟踪止损风控，如果回撤超过阈值，则降低仓位
        base_amount: 5000000    #组合基础资金，WonderTrader只记录资金的增量，基础资金是用来模拟组合的基本资金用的，和增量相加得到动态权益
        basic_ratio: 101        #日内高点百分比，即当日最高动态权益是上一次的101%才会触发跟踪侄止损
        calc_span: 5            #计算时间间隔，单位s
        inner_day_active: true  #日内跟踪止损是否启用
        inner_day_fd: 20.0      #日内跟踪止损阈值，即如果收益率从高点回撤20%，则触发风控
        multi_day_active: false #多日跟踪止损是否启用
        multi_day_fd: 60.0      #多日跟踪止损阈值
        risk_scale: 0.3         #风控系数，即组合给执行器的目标仓位，是组合理论仓位的0.3倍，即真实仓位是三成仓
        risk_span: 30           #风控触发时间间隔，单位s。因为风控计算很频繁，如果已经触发风控，不需要每次重算都输出风控日志，加一个时间间隔，友好一些

strategies:
    # CTA策略配置，当mocker为cta时会读取该配置项
    cta:
    -   active: true       # 模块名，linux下为xxxx.so
        id: dt_if          # 策略ID，自定义的
        name: WtCtaStraFact.DualThrust   # 策略名，要和factory中的匹配
        params:                         # 策略初始化参数，这个根据策略的需要提供
            code: SHFE.au.2206
            count: 1
            days: 30
            k1: 0.6
            k2: 0.6
            period: m1
            stock: false

executers: executers.yaml   #执行器配置文件
parsers: tdparsers.yaml     #行情通达配置文件
traders: tdtraders.yaml     #交易通道配置文件
bspolicy: actpolicy.yaml    #开平策略配置文件
```

Example 3 (unknown):
```unknown
code_filters:
    SHFE.au:
        action: ignore
        target: 0
strategy_filters:
    dt_if:
        action: redirect
        target: 0
executer_filters:
    exec: true
```

Example 4 (unknown):
```unknown
code_filters:
    SHFE.au:
        action: ignore
        target: 0
strategy_filters:
    dt_if:
        action: redirect
        target: 0
executer_filters:
    exec: true
```

---

## HFT仿真进阶2: 策略初始化

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file04.html

**Contents:**
- HFT仿真进阶2: 策略初始化
- 解决目标
  - stra_get_ticks
  - stra_sub_ticks
  - stra_get_bars
- 总结

source: wtcpp/folder03/file04.md

既然是进阶篇, 基础的内容就不赘述了. 本文的环境配置承接上篇环境准备篇

本文主要解决的问题是策略初始化时 on_init 中调用的三个方法及相关的判断逻辑

HftStraBaseCtx::stra_get_ticks -> WtEngine::get_tick_slice -> WtDtMgr::get_tick_slice -> WtDataReader::readTickSlice

2.WtDataReader 加载Tick数据逻辑

如果加载成功, 就通过引擎订阅tick行情, 如果没有tick数据则tick行情不会被订阅

stra_sub_ticks 是主动订阅Tick行情, 但是和通过 stra_get_ticks 被动订阅行情略有不同, stra_sub_ticks 会首先将品种名称添加到 _tick_subs 中, 如此在策略其他地方都可以通过 ctx-> 接口调用

HftStraBaseCtx::stra_get_bars -> WtEngine::get_kline_slice -> WtDtMgr::get_kline_slice -> WtDataReader::readKlineSlice

2.WtDataReader 加载Bar数据逻辑

Bar数据加载逻辑和tick数据加载逻辑非常相似, 应该很容易理解.

**Examples:**

Example 1 (javascript):
```javascript
// 加载Tick切片数据
WTSTickSlice* WtDataReader::readTickSlice(const char* stdCode, uint32_t count, uint64_t etime /* = 0 */)
{
	// 1. 获取合约标准码(合约类)
	CodeHelper::CodeInfo cInfo = CodeHelper::extractStdCode(stdCode);
	// 2. 获取合约对应的商品信息
	WTSCommodityInfo* commInfo = _base_data_mgr->getCommodity(cInfo._exchg, cInfo._product);
	// 3. 获取合约名称
	std::string stdPID = StrUtil::printf("%s.%s", cInfo._exchg, cInfo._product);
	// 4. 获取当前时间, 精确到毫秒
	uint32_t curDate, curTime, curSecs;
	if (etime == 0)
	{
		curDate = _sink->get_date();
		curTime = _sink->get_min_time();
		curSecs = _sink->get_secs();

		etime = (uint64_t)curDate * 1000000000 + curTime * 100000 + curSecs;
	}
	else
	{
		//20190807124533900
		curDate = (uint32_t)(etime / 1000000000);
		curTime = (uint32_t)(etime % 1000000000) / 100000;
		curSecs = (uint32_t)(etime % 100000);
	}
	/*
	如果不指定etime, 默认为 0, curDate和curTime是最新时间和日期,
	endTDate 会获取最新时间的交易日期
	curTDate也会获取最新时间的交易日期

	如果指定e_time, curDate和curTime是历史时间和日期
	endTDate 是历史时间的交易日期
	curTDate 是最新时间的交易日期

	之后如果两者不在同一天则从dsb加载历史Tick数据, 若在同一天则从 dmb 加载缓存Tick数据
	*/
	uint32_t endTDate = _base_data_mgr->calcTradingDate(stdPID.c_str(), curDate, curTime, false);
	uint32_t curTDate = _base_data_mgr->calcTradingDate(stdPID.c_str(), 0, 0, false);

	bool isToday = (endTDate == curTDate);
	// 6. 主力合约单独处理
	std::string curCode = cInfo._code;
	if (cInfo.isHot() && commInfo->isFuture())
		curCode = _hot_mgr->getRawCode(cInfo._exchg, cInfo._product, endTDate);
	else if (cInfo.isSecond() && commInfo->isFuture())
		curCode = _hot_mgr->getSecondRawCode(cInfo._exchg, cInfo._product, endTDate);

	//比较时间的对象
	WTSTickStruct eTick;
	eTick.action_date = curDate;
	eTick.action_time = curTime * 100000 + curSecs;
	// 如果在同一天(盘中只有dmb缓存文件)
	if (isToday)
	{
		// 7. 获取dmb文件的Tick数据映射, 如果没有就返回NULL, 如果有就返回切片数据
		TickBlockPair* tPair = getRTTickBlock(cInfo._exchg, curCode.c_str());
		if (tPair == NULL)
			return NULL;

		// ... 代码略
	}
	// 若不在同一天(收盘后会有dsb历史文件)
	else
	{
		std::string key = StrUtil::printf("%s-%d", stdCode, endTDate);
		// 9. 从dsb中加载数据切片
		auto it = _his_tick_map.find(key);
		// 代码略...
```

Example 2 (javascript):
```javascript
// 加载Tick切片数据
WTSTickSlice* WtDataReader::readTickSlice(const char* stdCode, uint32_t count, uint64_t etime /* = 0 */)
{
	// 1. 获取合约标准码(合约类)
	CodeHelper::CodeInfo cInfo = CodeHelper::extractStdCode(stdCode);
	// 2. 获取合约对应的商品信息
	WTSCommodityInfo* commInfo = _base_data_mgr->getCommodity(cInfo._exchg, cInfo._product);
	// 3. 获取合约名称
	std::string stdPID = StrUtil::printf("%s.%s", cInfo._exchg, cInfo._product);
	// 4. 获取当前时间, 精确到毫秒
	uint32_t curDate, curTime, curSecs;
	if (etime == 0)
	{
		curDate = _sink->get_date();
		curTime = _sink->get_min_time();
		curSecs = _sink->get_secs();

		etime = (uint64_t)curDate * 1000000000 + curTime * 100000 + curSecs;
	}
	else
	{
		//20190807124533900
		curDate = (uint32_t)(etime / 1000000000);
		curTime = (uint32_t)(etime % 1000000000) / 100000;
		curSecs = (uint32_t)(etime % 100000);
	}
	/*
	如果不指定etime, 默认为 0, curDate和curTime是最新时间和日期,
	endTDate 会获取最新时间的交易日期
	curTDate也会获取最新时间的交易日期

	如果指定e_time, curDate和curTime是历史时间和日期
	endTDate 是历史时间的交易日期
	curTDate 是最新时间的交易日期

	之后如果两者不在同一天则从dsb加载历史Tick数据, 若在同一天则从 dmb 加载缓存Tick数据
	*/
	uint32_t endTDate = _base_data_mgr->calcTradingDate(stdPID.c_str(), curDate, curTime, false);
	uint32_t curTDate = _base_data_mgr->calcTradingDate(stdPID.c_str(), 0, 0, false);

	bool isToday = (endTDate == curTDate);
	// 6. 主力合约单独处理
	std::string curCode = cInfo._code;
	if (cInfo.isHot() && commInfo->isFuture())
		curCode = _hot_mgr->getRawCode(cInfo._exchg, cInfo._product, endTDate);
	else if (cInfo.isSecond() && commInfo->isFuture())
		curCode = _hot_mgr->getSecondRawCode(cInfo._exchg, cInfo._product, endTDate);

	//比较时间的对象
	WTSTickStruct eTick;
	eTick.action_date = curDate;
	eTick.action_time = curTime * 100000 + curSecs;
	// 如果在同一天(盘中只有dmb缓存文件)
	if (isToday)
	{
		// 7. 获取dmb文件的Tick数据映射, 如果没有就返回NULL, 如果有就返回切片数据
		TickBlockPair* tPair = getRTTickBlock(cInfo._exchg, curCode.c_str());
		if (tPair == NULL)
			return NULL;

		// ... 代码略
	}
	// 若不在同一天(收盘后会有dsb历史文件)
	else
	{
		std::string key = StrUtil::printf("%s-%d", stdCode, endTDate);
		// 9. 从dsb中加载数据切片
		auto it = _his_tick_map.find(key);
		// 代码略...
```

Example 3 (javascript):
```javascript
WTSTickSlice* HftStraBaseCtx::stra_get_ticks(const char* stdCode, uint32_t count)
{
	WTSTickSlice* ticks = _engine->get_tick_slice(_context_id, stdCode, count);

	if (ticks)
		_engine->sub_tick(id(), stdCode);
	return ticks;
}
```

Example 4 (javascript):
```javascript
WTSTickSlice* HftStraBaseCtx::stra_get_ticks(const char* stdCode, uint32_t count)
{
	WTSTickSlice* ticks = _engine->get_tick_slice(_context_id, stdCode, count);

	if (ticks)
		_engine->sub_tick(id(), stdCode);
	return ticks;
}
```

---

## 对接 openctp 仿真

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder02/file02.html

**Contents:**
- 对接 openctp 仿真
- openctp 介绍:
- 对接openctp仿真注意事项
- 部署openctp环境
  - 准备工作
    - 1. 配置文件
    - 2. 添加 tts_thostmduserapi_se.dll
    - 3. 确认修改成功

source: wtcpp/folder02/file02.md

CTP开放平台提供A股、港股、美股、期货、期权等全品种接入通道，通过提供中泰证券XTP、华鑫证券奇点、东方证券OST、盈透证券TWS等各通道的CTPAPI接口，CTP程序可以无缝对接各股票柜台。平台也提供了一套基于TTS交易系统的模拟环境，同样提供了CTPAPI兼容接口，可以替代Simnow，为CTP量化交易开发者提供7x24可用的模拟环境。

openctp 是极为优秀的仿真测试项目, 原本只要将ctp的两个dll文件替换掉就可以直接做仿真交易, 但 openctp的行情数据返回没有 trading_date 这个字段, 除此之外, 在7*24小时的测试环境中, 由于非交易时段的 action_date 和 action_time 小于本地时间, 因此 WonderTrader 项目中专门添加了配置字段 "localtime", 用本地时间填充对应的时间字段.

statemonitor.yaml 样式 (如果使用7*24小时环境可能需要修改)

1.进入 openctp 项目网站, 下载对应版本的 dll文件

2.将 thostmduserapi_se.dll 改名为 tts_thostmduserapi_se.dll(这里只用到行情dll), 并放到 "Debug\QuoteFactory\parsers" 目录下

运行 "QuoteFactory/main.cpp" 程序

**Examples:**

Example 1 (unknown):
```unknown
basefiles:
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
broadcaster:
    active: true
    bport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
parsers:
 # 一般环境
-   active: true   # 是否启动改环境
    broker: ""
    id: tts
    module: ParserCTP
    front: tcp://121.36.146.182:20004
    ctpmodule: tts_thostmduserapi_se
    localtime: true     # 这个字段用本地时间填充对应的字段, 仅供测试使用, 如simnow全天候行情，openctp等环境, 实盘一定要关闭
    # 去 openctp 项目网站查看申请方式
    pass: ******
    user: ******
    # 只接收 au 行情
    code: SHFE.au2204,SHFE.au2205
# 7*24 小时环境
-   active: false
    broker: ""
    id: tts24       
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    localtime: true     # 这个字段用本地时间填充对应的字段, 仅供测试使用, 如simnow全天候行情，openctp等环境, 实盘一定要关闭
    # 去 openctp 项目网站查看申请方式
    pass: ******
    user: ******
    # 只接收 au 行情
    code: SHFE.au2204,SHFE.au2205

statemonitor: statemonitor.yaml
writer:
    module: WtDataStorage
    async: true
    groupsize: 20       # 每次接收20个tick
    path: ./FUT_Data    # 数据保存路径
    savelog: false
```

Example 2 (unknown):
```unknown
basefiles:
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
broadcaster:
    active: true
    bport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
parsers:
 # 一般环境
-   active: true   # 是否启动改环境
    broker: ""
    id: tts
    module: ParserCTP
    front: tcp://121.36.146.182:20004
    ctpmodule: tts_thostmduserapi_se
    localtime: true     # 这个字段用本地时间填充对应的字段, 仅供测试使用, 如simnow全天候行情，openctp等环境, 实盘一定要关闭
    # 去 openctp 项目网站查看申请方式
    pass: ******
    user: ******
    # 只接收 au 行情
    code: SHFE.au2204,SHFE.au2205
# 7*24 小时环境
-   active: false
    broker: ""
    id: tts24       
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    localtime: true     # 这个字段用本地时间填充对应的字段, 仅供测试使用, 如simnow全天候行情，openctp等环境, 实盘一定要关闭
    # 去 openctp 项目网站查看申请方式
    pass: ******
    user: ******
    # 只接收 au 行情
    code: SHFE.au2204,SHFE.au2205

statemonitor: statemonitor.yaml
writer:
    module: WtDataStorage
    async: true
    groupsize: 20       # 每次接收20个tick
    path: ./FUT_Data    # 数据保存路径
    savelog: false
```

Example 3 (unknown):
```unknown
parser:
    async: false
    level: debug
    sinks:
    -   filename: DtLogs/Parser.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: false
        type: daily_file_sink
root:
    async: false
    level: debug
    sinks:
    -   filename: DtLogs/QuoteFact.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: false
        type: daily_file_sink
    -   pattern: '[%m.%d %H:%M:%S - %^%-5l%$] %v'
        type: console_sink
```

Example 4 (unknown):
```unknown
parser:
    async: false
    level: debug
    sinks:
    -   filename: DtLogs/Parser.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: false
        type: daily_file_sink
root:
    async: false
    level: debug
    sinks:
    -   filename: DtLogs/QuoteFact.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: false
        type: daily_file_sink
    -   pattern: '[%m.%d %H:%M:%S - %^%-5l%$] %v'
        type: console_sink
```

---

## HFT仿真详解

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file02.html

**Contents:**
- HFT仿真详解
- 准备工作
  - 需要先完成生成解决方案
  - 1. 配置文件
  - 2. 配置 openctp 仿真
  - 3. 修改策略文件
- 逐步解析
  - 1. 初始化
  - 2. 加载配置
    - 1. 初始化交易引擎

source: wtcpp/folder03/file02.md

为了方便测试, 建议对策略文件 WtHftStraDemo.cpp 做小小的修改

3.移动文件 将 "src\x64\Debug" 目录下的 "WtHftStraFact.dll" 移动到 "src\x64\Debug\WtRunner\hft" 目录

仿真交易的主代码很简单, WtRunner项目下的main.cpp 文件

runner.config();函数前半部分都是加载配置文件, 已经老生常谈了, 详情可参看文件"CTA回测流程逐步解析"和"数据落地"

跟中代码进入 bool WtDtMgr::initStore(WTSVariant* cfg) 该函数首先加载了 WtDataStorage.dll 文件, 然后创建了数据读取器 _reader, 最重要的只有末尾一句代码

_reader 实际指向了 WtDataReader, 最终执行了 WtDataReader::init, 该方法中主要设置文件保存地址, 及添加数据管理器等对象

主要是加载交易规则配置文件 actpolicy.yaml, 该文件主要针对特定品种对开仓平仓方向及数量做限制

1.ParserAdapter::init() 方法前半部分加载 ParserCTP.dll, 不再赘述, 对应配置文件中

2.中间部分 _exchg_filter 过滤交易所行情, _code_filter 过滤具体合约行情, 对应配置文件中

主逻辑通行情通道, 只不过中间多了初始化交易风控部分(略), 对应配置文件中

策略管理器 HftStrategyMgr 中的 _factories 存放所有策略工厂

通过策略工厂创建对应的策略, 并保存到策略管理器 HftStrategyMgr 中的 _strategies

将 WtHftEngine 传入到上下文管理器中

上下文管理器中的 _strategy 即策略指针

上下文管理器中的 _trader 即交易适配器

交易管理器中的 _sinks 即上下文管理器集合

HFT引擎中的 _ctx_map 即上下文管理器集合

执行步骤: 1.进入HftStraContext::on_init() 2.回调策略 _strategy->on_init(this); 3.策略文件 WtHftStraDemo.cpp 中的 on_init 方法如下

4.因此首先进入HftStraBaseCtx::stra_get_ticks 获取数据 5.然后进入 ` HftStraBaseCtx::stra_sub_ticks` 订阅行情 6.策略上下文回调策略初始化完毕

**Examples:**

Example 1 (unknown):
```unknown
basefiles:
    session: ./common/sessions.json
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    hot: ./common/hots.json
    utf-8: false
env:
    name: hft       # 确定交易引擎类型
    mode: product
    filters: filters.yaml
    fees: ./common/fees.json
    product: 
        session: FN0230   # 为时间步进器提供
bspolicy: actpolicy.yaml
data:
    store:
        path: ./FUT_Data/
strategies:
    hft:
    -   active: true
        id: hft_demo
        name: WtHftStraFact.HftDemoStrategy
        params:
            code: SHFE.au.2204	#  注意格式
            count: 10
            second: 10
            offset: 1
            count: 50
            stock: false
        trader: tts24
traders:
-   active: false
    id: tts
    module: TraderCTP
    ctpmodule: tts_thosttraderapi_se
    front: tcp://121.36.146.182:20002
    broker: ""      # 可以为空, 但必须要有
    user: ******
    pass: ******
    appid:
    authcode:
    quick: true
    riskmon:
        active: false
        policy:
            default:
                order_times_boundary: 20
                order_stat_timespan: 10
                cancel_times_boundary: 20
                cancel_stat_timespan: 10
                cancel_total_limits: 470
-   active: true
    id: tts24
    module: TraderCTP
    ctpmodule: tts_thosttraderapi_se
    front: tcp://122.51.136.165:20002
    broker: ""
    user: ******
    pass: ******
    appid:
    authcode:
    quick: true
    riskmon:
        active: false
        policy:
            default:
                order_times_boundary: 20
                order_stat_timespan: 10
                cancel_times_boundary: 20
                cancel_stat_timespan: 10
                cancel_total_limits: 470
parsers:
-   active: false
    broker: ""
    id: tts
    module: ParserCTP
    front: tcp://121.36.146.182:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    code: SHFE.au2204,SHFE.au2205
-   active: true
    broker: ""
    id: tts24
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    code: SHFE.au2204,SHFE.au2205
```

Example 2 (unknown):
```unknown
basefiles:
    session: ./common/sessions.json
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    hot: ./common/hots.json
    utf-8: false
env:
    name: hft       # 确定交易引擎类型
    mode: product
    filters: filters.yaml
    fees: ./common/fees.json
    product: 
        session: FN0230   # 为时间步进器提供
bspolicy: actpolicy.yaml
data:
    store:
        path: ./FUT_Data/
strategies:
    hft:
    -   active: true
        id: hft_demo
        name: WtHftStraFact.HftDemoStrategy
        params:
            code: SHFE.au.2204	#  注意格式
            count: 10
            second: 10
            offset: 1
            count: 50
            stock: false
        trader: tts24
traders:
-   active: false
    id: tts
    module: TraderCTP
    ctpmodule: tts_thosttraderapi_se
    front: tcp://121.36.146.182:20002
    broker: ""      # 可以为空, 但必须要有
    user: ******
    pass: ******
    appid:
    authcode:
    quick: true
    riskmon:
        active: false
        policy:
            default:
                order_times_boundary: 20
                order_stat_timespan: 10
                cancel_times_boundary: 20
                cancel_stat_timespan: 10
                cancel_total_limits: 470
-   active: true
    id: tts24
    module: TraderCTP
    ctpmodule: tts_thosttraderapi_se
    front: tcp://122.51.136.165:20002
    broker: ""
    user: ******
    pass: ******
    appid:
    authcode:
    quick: true
    riskmon:
        active: false
        policy:
            default:
                order_times_boundary: 20
                order_stat_timespan: 10
                cancel_times_boundary: 20
                cancel_stat_timespan: 10
                cancel_total_limits: 470
parsers:
-   active: false
    broker: ""
    id: tts
    module: ParserCTP
    front: tcp://121.36.146.182:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    code: SHFE.au2204,SHFE.au2205
-   active: true
    broker: ""
    id: tts24
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    code: SHFE.au2204,SHFE.au2205
```

Example 3 (unknown):
```unknown
dyn_pattern:
    executer:
        async: false
        level: debug
        sinks:
        -   filename: Logs/Executer_%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: daily_file_sink
    parser:
        async: false
        level: debug
        sinks:
        -   filename: Logs/Parser_%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: daily_file_sink
    strategy:
        async: false
        level: debug
        sinks:
        -   filename: Logs/Strategy/%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: daily_file_sink
    trader:
        async: false
        level: debug
        sinks:
        -   filename: Logs/Trader_%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: daily_file_sink
risk:
    async: false
    level: debug
    sinks:
    -   filename: Logs/Riskmon.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: true
        type: daily_file_sink
root:
    async: false
    level: debug
    sinks:
    -   filename: Logs/Runner.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: true
        type: daily_file_sink
    -   pattern: '[%m.%d %H:%M:%S - %-5l] %v'
        type: ostream_sink
```

Example 4 (unknown):
```unknown
dyn_pattern:
    executer:
        async: false
        level: debug
        sinks:
        -   filename: Logs/Executer_%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: daily_file_sink
    parser:
        async: false
        level: debug
        sinks:
        -   filename: Logs/Parser_%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: daily_file_sink
    strategy:
        async: false
        level: debug
        sinks:
        -   filename: Logs/Strategy/%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: daily_file_sink
    trader:
        async: false
        level: debug
        sinks:
        -   filename: Logs/Trader_%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: daily_file_sink
risk:
    async: false
    level: debug
    sinks:
    -   filename: Logs/Riskmon.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: true
        type: daily_file_sink
root:
    async: false
    level: debug
    sinks:
    -   filename: Logs/Runner.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: true
        type: daily_file_sink
    -   pattern: '[%m.%d %H:%M:%S - %-5l] %v'
        type: ostream_sink
```

---

## 数据落地详解

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder02/file01.html

**Contents:**
- 数据落地详解
- 准备工作
  - 1. 配置文件
- 源码分析
  - 3. 初始化基础数据管理器
  - 4. 初始化UDP广播对象
  - 5. 初始化状态机
  - 6. 初始化数据管理器
  - 7. 初始化行情通道
  - 8.1 启动行情适配管理器

source: wtcpp/folder02/file01.md

执行 QuoteFactory 下的 main.cpp

程序主逻辑都在 initialize() 函数中

代码中的 g_baseDataMgr 是基础数据管理器, 主要加载各类配置文件

UDP广播内容大致能明白, 但描述可能不精确

主要将初始化的状态数据填充至 _map 中

主要是加载输出写入dll文件, 并初始化数据写入器

主要是根据配置初始化行情适配器, 并将行情适配器添加到行情适配管理器中

在开盘期间运行程序, 如果出现如下界面, 说明程序运行成功

**Examples:**

Example 1 (unknown):
```unknown
# 配置文件目录
basefiles:        
    commodity: ./common/commodities.json
    contract: ./common/contract.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
# UDP配置
broadcaster:
    active: true
    sport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
# 行情配置
parsers:
-   active: true        # 该行情适配器是否可用
    id: parser          # 当前行情适配器id
    broker: '9999'    
    code: ''
    front: tcp://180.168.146.187:10211
    module: ParserCTP   # CTP行情解析dll文件
    pass: ******        # simnow 密码
    user: ******        # simnow 用户名
statemonitor: statemonitor.yaml
writer:
    async: true         # 是否异步执行
    groupsize: 100      # 每次存多少条tick数据
    path: ./FUT_Data    # 文件存储目录
    savelog: false
```

Example 2 (unknown):
```unknown
# 配置文件目录
basefiles:        
    commodity: ./common/commodities.json
    contract: ./common/contract.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
# UDP配置
broadcaster:
    active: true
    sport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
# 行情配置
parsers:
-   active: true        # 该行情适配器是否可用
    id: parser          # 当前行情适配器id
    broker: '9999'    
    code: ''
    front: tcp://180.168.146.187:10211
    module: ParserCTP   # CTP行情解析dll文件
    pass: ******        # simnow 密码
    user: ******        # simnow 用户名
statemonitor: statemonitor.yaml
writer:
    async: true         # 是否异步执行
    groupsize: 100      # 每次存多少条tick数据
    path: ./FUT_Data    # 文件存储目录
    savelog: false
```

Example 3 (unknown):
```unknown
parser:
    async: false
    level: debug
    sinks:
    -   filename: DtLogs/Parser.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: false
        type: daily_file_sink
root:
    async: false
    level: debug
    sinks:
    -   filename: DtLogs/QuoteFact.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: false
        type: daily_file_sink
    -   pattern: '[%m.%d %H:%M:%S - %^%-5l%$] %v'
        type: console_sink
```

Example 4 (unknown):
```unknown
parser:
    async: false
    level: debug
    sinks:
    -   filename: DtLogs/Parser.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: false
        type: daily_file_sink
root:
    async: false
    level: debug
    sinks:
    -   filename: DtLogs/QuoteFact.log
        pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
        truncate: false
        type: daily_file_sink
    -   pattern: '[%m.%d %H:%M:%S - %^%-5l%$] %v'
        type: console_sink
```

---

## HFT仿真进阶1: 环境准备

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file03.html

**Contents:**
- HFT仿真进阶1: 环境准备
- 准备工作
  - 准备HFT策略配置文件
  - 准备TestDtPorter
  - 注意事项
  - 重要的配置示例
  - 示例策略
- 程序运行
- 成功验证

source: wtcpp/folder03/file03.md

之前写的 "HFT仿真详解" 仍然不够详细, 有很多细节仍然没有注意到, 比如何时回调策略中的 "on_bar" 接口?

延续我们的优良传统, 首先介绍环境准备内容, 然后再深入到源码部分.

HFT仿真运行 "WtRunner"目录, 行情数据运行 "WtDtPorter"目录

为了方便测试, 我们需要自己准备策略, 策略源码如下.

如果上述步骤都无误, 程序应该会在每个Tick打印3个品种的价格, 并在每分钟结束定时回调2个 "on_bar"

如果只是出现on_tick, 而on_bar并未正常调用也没关系, 接下来源码分析的文章会阐述为何需要这些配置以及为何会出现上述现象.

**Examples:**

Example 1 (unknown):
```unknown
basefiles:
    utf-8: true     # 配置文件是否使用utf-8编码
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
broadcaster:
    active: true
    bport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
parsers: mdparsers.yaml
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 300
    path: ../Storage        # 确保和WtRunner一致
    savelog: true
```

Example 2 (unknown):
```unknown
basefiles:
    utf-8: true     # 配置文件是否使用utf-8编码
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
broadcaster:
    active: true
    bport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
parsers: mdparsers.yaml
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 300
    path: ../Storage        # 确保和WtRunner一致
    savelog: true
```

Example 3 (unknown):
```unknown
basefiles:
    session: ./common/sessions.json
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    hot: ./common/hots.json
    utf-8: true
env:
    name: hft		    # 确定交易引擎类型
    mode: product
    filters: filters.yaml
    fees: ./common/fees.json
    product: 
        session: TTS24   	# 为时间步进器提供
bspolicy: actpolicy.yaml
data:
    store:
        path: ../Storage/   # 确保和WtDtPorter一致
strategies:
    hft:
    -   active: true
        id: hft_demo
        name: WtHftStraFact.HftDemoStrategy
        params:
            code: CFFEX.IC.2203
            count: 10
            second: 10
            offset: 1
            count: 50
            stock: false
        trader: tts24
traders:
-   active: true
    id: tts24
    module: TraderCTP
    ctpmodule: tts_thosttraderapi_se
    front: tcp://122.51.136.165:20002
    broker: ""
    user: 1493
    pass: 123456
    appid:
    authcode:
    quick: true
    riskmon:
        active: false
        policy:
            default:
                order_times_boundary: 20
                order_stat_timespan: 10
                cancel_times_boundary: 20
                cancel_stat_timespan: 10
                cancel_total_limits: 470
parsers:
-   active: true
    broker: ""
    id: tts24
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    filter: CFFEX
```

Example 4 (unknown):
```unknown
basefiles:
    session: ./common/sessions.json
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    hot: ./common/hots.json
    utf-8: true
env:
    name: hft		    # 确定交易引擎类型
    mode: product
    filters: filters.yaml
    fees: ./common/fees.json
    product: 
        session: TTS24   	# 为时间步进器提供
bspolicy: actpolicy.yaml
data:
    store:
        path: ../Storage/   # 确保和WtDtPorter一致
strategies:
    hft:
    -   active: true
        id: hft_demo
        name: WtHftStraFact.HftDemoStrategy
        params:
            code: CFFEX.IC.2203
            count: 10
            second: 10
            offset: 1
            count: 50
            stock: false
        trader: tts24
traders:
-   active: true
    id: tts24
    module: TraderCTP
    ctpmodule: tts_thosttraderapi_se
    front: tcp://122.51.136.165:20002
    broker: ""
    user: 1493
    pass: 123456
    appid:
    authcode:
    quick: true
    riskmon:
        active: false
        policy:
            default:
                order_times_boundary: 20
                order_stat_timespan: 10
                cancel_times_boundary: 20
                cancel_stat_timespan: 10
                cancel_total_limits: 470
parsers:
-   active: true
    broker: ""
    id: tts24
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    filter: CFFEX
```

---

## HFT回测进阶1: 环境准备

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder01/file02.html

**Contents:**
- HFT回测进阶1: 环境准备
- 准备数据
- 准备配置文件
  - 1. 配置文件
- 准备策略
- 成功验证

source: wtcpp/folder01/file02.md

请使用最新版0.9dev源码(20220303)

1.为了方便回测, 建议修改策略文件 WtHftStraDemo.cpp , 主要是在策略中添加一些日志输出信息, 方便调试. 这里就直接给源码, 不再解释(主要添加了 _ctx->stra_log_info... )

2.修改策略工厂文件 WtHftStraFact.cpp 或配置文件 configbt.yaml->strategy->name 字段 一定要保证上述两个位置和configbt.yaml中策略配置下的策略名称一致

3.修改完毕后右键点击WtHftStraFact, 重新生成

4.将重新生成的Debug文件夹下 WtHftStraFact.dll 放到 WtBtRunner 文件夹下

执行 WtBtRunner 程序, 成功截图如下

**Examples:**

Example 1 (unknown):
```unknown
# 回测参数
replayer:
    basefiles:
        commodity: ./common/commodities.json
        contract: ./common/contracts.json
        holiday: ./common/holidays.json
        session: ./common/sessions.json
    mode: csv       # 回测模式, 可自动加载csv文件下的bar数据
    path: ./storage/
    fees: ./common/fees.json
    stime: 202202210800    # tick回测一天数据量足够测试了
    etime: 202202220900
    tick: true      # 使用tick数据, 如果为false, 会自动生成模拟tick数据
env:
    mocker: hft     # 模式
    slippage: 5     # 滑点
hft:
    error_rate: 30
    # 策略工厂dll文件名称
    module: WtHftStraFact
    # 策略配置
    strategy:
        id: hft_id
        # 策略名称(在策略工厂源码中有用)
        name: HftDemoStrategy
        # 策略参数
        params:
            code: CUSTOM.FX.EURUSD
            count: 6
            second: 5
            freq: 20
            offset: 0
            reserve: 0
            stock: false
    use_newpx: true
```

Example 2 (unknown):
```unknown
# 回测参数
replayer:
    basefiles:
        commodity: ./common/commodities.json
        contract: ./common/contracts.json
        holiday: ./common/holidays.json
        session: ./common/sessions.json
    mode: csv       # 回测模式, 可自动加载csv文件下的bar数据
    path: ./storage/
    fees: ./common/fees.json
    stime: 202202210800    # tick回测一天数据量足够测试了
    etime: 202202220900
    tick: true      # 使用tick数据, 如果为false, 会自动生成模拟tick数据
env:
    mocker: hft     # 模式
    slippage: 5     # 滑点
hft:
    error_rate: 30
    # 策略工厂dll文件名称
    module: WtHftStraFact
    # 策略配置
    strategy:
        id: hft_id
        # 策略名称(在策略工厂源码中有用)
        name: HftDemoStrategy
        # 策略参数
        params:
            code: CUSTOM.FX.EURUSD
            count: 6
            second: 5
            freq: 20
            offset: 0
            reserve: 0
            stock: false
    use_newpx: true
```

Example 3 (unknown):
```unknown
dyn_pattern:
    strategy:
        async: false
        level: debug
        sinks:
        -   filename: BtLogs/Strategy/%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: basic_file_sink
root:
    async: false
    level: debug
    sinks:
    -   filename: BtLogs/BtRunner.log
        pattern: '[%Y.%m.%d %H:%M:%S,%F - %-5l] %v'
        truncate: true
        type: basic_file_sink
    -   pattern: '[%m.%d %H:%M:%S - %^%-5l%$] %v'
        type: console_sink
```

Example 4 (unknown):
```unknown
dyn_pattern:
    strategy:
        async: false
        level: debug
        sinks:
        -   filename: BtLogs/Strategy/%s.log
            pattern: '[%Y.%m.%d %H:%M:%S - %-5l] %v'
            truncate: true
            type: basic_file_sink
root:
    async: false
    level: debug
    sinks:
    -   filename: BtLogs/BtRunner.log
        pattern: '[%Y.%m.%d %H:%M:%S,%F - %-5l] %v'
        truncate: true
        type: basic_file_sink
    -   pattern: '[%m.%d %H:%M:%S - %^%-5l%$] %v'
        type: console_sink
```

---

## CTA仿真完整篇3: 下单流程

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file11.html

**Contents:**
- CTA仿真完整篇3: 下单流程
- stra_get_position
- set_positions
- WtLocalExecuter::set_position
  - WtMinImpactExeUnit::set_position
- 总结

source: wtcpp/folder03/file11.md

本节内容开始阐述CTA策略下单流程, 其实"CTA完整篇"系列内容本应该在"CTA进阶篇"之后, 无奈工作繁忙, 正准备接着"进阶篇3"之后继续写下单流程时被搁置了, 那几天恰好又赶上WT新版本发布, 于是乎就重新整理了"CTA完整篇".

可能有人会问为什么不选择"HFT下单流程", 因为"HFT"整个逻辑比较简单, "CTA"逻辑复杂, 要想写一个完整流程, 干脆直接挑一个"刺儿头"(经常看群里人抱怨没文档, 说实话, 我看WT源码也很苦逼啊…就是一点点磨), 如果你把CTA下单流程看明白了, "HFT下单流程"自然不在话下.

1.将断点打在 WtStraDualThrust::on_schedule 下的 if(decimal::eq(curPos,0)). (注意, 保证"QuoteFactory"在开启状态), 当分钟结束时, 程序回调 on_schedule 会在断点处自动停止

2.执行 ctx->stra_enter_long, 进入 CtaStraBaseCtx::stra_enter_long, 这里通过 limitprice 和 stopprice 设置了三种下单方式: 市价单(最新价), 限价单和停止单

3.我们默认是市价单, 执行 append_signal, 进入 CtaStraBaseCtx::append_signal, 填充 sInfo 字段(信号信息), 然后存放在 _sig_map 中.

然后就结束了, 是的, stra_get_position 就这么结束了.

可是我们只是记录了信号, 还没有将订单发送到交易所.

1.继续执行程序, 直到结束策略中的 on_schedule, 回到 CtaStraContext::on_calculate, 执行至结束, 回到 CtaStraBaseCtx::on_schedule, 执行至结束, 回到 WtCtaEngine::on_schedule.

2.执行 ctx->enum_position, 这里通过匿名函数方式将品种及对应目标仓位存放在 target_pos中. 注意, 这里会通过信号过滤器检查(不是本文重点).

3.向下执行至"//处理组合理论部位", 在这里会遍历所有策略中的 target_pos, 执行 append_signal, 进入 WtEngine::append_signal. 这里会将品种名称和下单量保存在 _sig_map 中(和前面的 CtaStraBaseCtx 中的 _sig_map 很像).然后执行结束.

4.向下执行至 _exec_mgr.set_positions, 进入 WtExecuterMgr::set_positions, 首先通过信号过滤器校验.然后开始遍历所有的执行器(每个CTA策略都有对应的执行器)

5.执行至 executer->set_position, 进入 WtLocalExecuter::set_position, 这里才真正开始准备向市场发送订单.

1.执行 unit->self()->set_position, 才真正进入执行单元, 这里经过一番检查, 然后走到 do_calc

2.undone: 未完成订单, realPos: 当前真正持仓, newVol: 新的目标持仓, diffPos: 即将要发送的持仓.

3.中间检查订单和持仓状态等做了大量判断, 这里不做详细说明, 感兴趣可以自己看看.

4.最后的最后, 调用 _ctx->buy

5.然后进入 WtLocalExecuter::buy, 调用 _trader->buy, 订单终于被发送到交易所.

流程终于走完了, 但是很多人应该已经被绕晕了, 在CTA从策略中发送一笔订单过程中, 主要经历了以下几个类.(还不包含一些数据管理, 行情和交易等类)

**Examples:**

Example 1 (unknown):
```unknown
if (isBuy)
{
    OrderIDs ids = _ctx->buy(stdCode, buyPx, this_qty, bForceClose);
    _orders_mon.push_order(ids.data(), ids.size(), _ctx->getCurTime(), isCanCancel);
}
```

Example 2 (unknown):
```unknown
if (isBuy)
{
    OrderIDs ids = _ctx->buy(stdCode, buyPx, this_qty, bForceClose);
    _orders_mon.push_order(ids.data(), ids.size(), _ctx->getCurTime(), isCanCancel);
}
```

---

## 关于WT使用一个礼拜后的一些积累

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder50/file02.html

**Contents:**
- 关于WT使用一个礼拜后的一些积累
- 直接使用执行器做算法交易
- WT的架构理解

source: wtcpp/folder50/file02.md

开始使用WT到现在有一个礼拜了，目前已经跑通了QuoteFactory，然后根据当前不通过策略直接做算法交易的需求，开始使用WTExeMon做算法交易，目前有两种方法，一个是在WTExecMon上自己建main.cpp，代码如下：

代码里面使用的cofig.yaml文件，Excecutors目前只能直接使用Excecutors配置，而不能指定Excecutors.ymal，配置参考如下：

另外一种方法就是修改WTExecPorter，所谓的porter就是dll提供的外部接口。这里面的代码其实本质上和第一种方法一致，但WTExecPorter的实现是通过调用WTExecMon的dll文件对外提供的接口，使用WTExecPorter的时候也要注意加上：

否则同样在程序执行完main函数后会直接推出，而不是等待行情的传入、交易的执行。

以前没做过这种量化交易系统，但通过这几天的使用和代码调试，终于理解了这类系统的根本业务逻辑还是使用行情和交易回调函数来驱动策略（或者直接交易），理解了这一点，策略或者执行器的入口都可以通过在ParserAdapter和TraderAdapter适配的具体的Parser和Trader中找到，举个例子，执行器中的on_tick触发，可以从Parser的接受行情回调函数中去找，像ParserXTP中的onDepthMarketData函数中就有handlequote的调用，跟下代码你就会发现有handletick的逻辑，handletick就会去调用策略或者执行器中的on_tick。

**Examples:**

Example 1 (python):
```python
#include <boost/asio.hpp>
#include "WtExecRunner.h"


 //#include <vld.h>

#ifdef _MSC_VER
#include "../Common/mdump.h"
#endif

//#include <vld.h>
boost::asio::io_service g_asyncIO;
int main()
{
#ifdef _MSC_VER
 CMiniDumper::Enable("WtExecMon.exe", true);
#endif

 WtExecRunner runner;
 std::string cfgFile = "config.json";
 if (!StdFile::exists(cfgFile.c_str()))
  cfgFile = "config.yaml";

 runner.init();
 runner.config(cfgFile.c_str());


 runner.run();
 //开仓1万股
 runner.setPosition("SSE.600276", 10000);
 //等待IO，如果下面两句话不写main在执行完之后就会直接退出控制台
 boost::asio::io_service::work work(g_asyncIO);
 g_asyncIO.run();

 return 0;
}
```

Example 2 (python):
```python
#include <boost/asio.hpp>
#include "WtExecRunner.h"


 //#include <vld.h>

#ifdef _MSC_VER
#include "../Common/mdump.h"
#endif

//#include <vld.h>
boost::asio::io_service g_asyncIO;
int main()
{
#ifdef _MSC_VER
 CMiniDumper::Enable("WtExecMon.exe", true);
#endif

 WtExecRunner runner;
 std::string cfgFile = "config.json";
 if (!StdFile::exists(cfgFile.c_str()))
  cfgFile = "config.yaml";

 runner.init();
 runner.config(cfgFile.c_str());


 runner.run();
 //开仓1万股
 runner.setPosition("SSE.600276", 10000);
 //等待IO，如果下面两句话不写main在执行完之后就会直接退出控制台
 boost::asio::io_service::work work(g_asyncIO);
 g_asyncIO.run();

 return 0;
}
```

Example 3 (unknown):
```unknown
basefiles:
    commodity: ../common/stk_comms.json
    contract: ../common/stocks.json
    holiday: ../common/holidays.json
    session: ../common/stk_sessions.json
data:
    store:
        #这个session不一定要指定，否则会报莫名其妙的错误然后粗
        session: SD0930
        path: ../STK_Data/
executers:
-   active: true    #是否启用
    id: exec        #执行器id，不可重复
    trader: simnow  #执行器绑定的交易通道id，如果不存在，无法执行
    scale: 1        #数量放大倍数，即该执行器的目标仓位，是组合理论目标仓位的多少倍，可以为小数 
    policy:         #执行单元分配策略，系统根据该策略创建对一个的执行单元
        default:    #默认策略，根据品种ID设置，如SHFE.rb，如果没有针对品种设置，则使用默认策略
            name: WtExeFact.WtMinImpactExeUnit #执行单元名称
            offset: 0      #委托价偏移跳数
            expire: 5      #订单超时没秒数
            pricemode: 1   #基础价格模式，-1-己方最优，0-最新价，1-对手价
            span: 500      #下单时间间隔（tick驱动的）
            byrate: false  #是否按对手盘挂单量的比例挂单，配合rate使用
            lots: 100       #固定数量
            rate: 0         #挂单比例，配合byrate使用

    clear:                  #过期主力自动清理配置
        active: false       #是否启用
        excludes:           #排除列表
        - CFFEX.IF
        - CFFEX.IC
        includes:           #包含列表
        - SHFE.rb

fees: ../common/fees_stk.json
filters: filters.yaml       #过滤器配置文件，这个主要是用于盘中不停机干预的
parsers: tdparsers.yaml     #行情通达配置文件
traders: tdtraders.yaml     #交易通道配置文件
bspolicy: actpolicy.yaml
```

Example 4 (unknown):
```unknown
basefiles:
    commodity: ../common/stk_comms.json
    contract: ../common/stocks.json
    holiday: ../common/holidays.json
    session: ../common/stk_sessions.json
data:
    store:
        #这个session不一定要指定，否则会报莫名其妙的错误然后粗
        session: SD0930
        path: ../STK_Data/
executers:
-   active: true    #是否启用
    id: exec        #执行器id，不可重复
    trader: simnow  #执行器绑定的交易通道id，如果不存在，无法执行
    scale: 1        #数量放大倍数，即该执行器的目标仓位，是组合理论目标仓位的多少倍，可以为小数 
    policy:         #执行单元分配策略，系统根据该策略创建对一个的执行单元
        default:    #默认策略，根据品种ID设置，如SHFE.rb，如果没有针对品种设置，则使用默认策略
            name: WtExeFact.WtMinImpactExeUnit #执行单元名称
            offset: 0      #委托价偏移跳数
            expire: 5      #订单超时没秒数
            pricemode: 1   #基础价格模式，-1-己方最优，0-最新价，1-对手价
            span: 500      #下单时间间隔（tick驱动的）
            byrate: false  #是否按对手盘挂单量的比例挂单，配合rate使用
            lots: 100       #固定数量
            rate: 0         #挂单比例，配合byrate使用

    clear:                  #过期主力自动清理配置
        active: false       #是否启用
        excludes:           #排除列表
        - CFFEX.IF
        - CFFEX.IC
        includes:           #包含列表
        - SHFE.rb

fees: ../common/fees_stk.json
filters: filters.yaml       #过滤器配置文件，这个主要是用于盘中不停机干预的
parsers: tdparsers.yaml     #行情通达配置文件
traders: tdtraders.yaml     #交易通道配置文件
bspolicy: actpolicy.yaml
```

---

## CTA仿真进阶篇1: 环境部署

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file06.html

**Contents:**
- CTA仿真进阶篇1: 环境部署
- 启动数据落地程序
  - 文件配置
  - 成功截图如下
- 重写CTA策略
- CTA策略配置
  - 配置概述
- 全部配置成功示例如下

source: wtcpp/folder03/file06.md

相比HFT仿真, CTA的仿真更加复杂点, 因此我们之前先讲解了HFT仿真交易, 如果对此不熟悉的小伙伴建议看之前的文章. 只有熟悉了HFT仿真逻辑之后才能较容易理解CTA仿真逻辑.

本系列文章建立在你对WT项目已有一定了解的基础上. 我将会逐步探索CTA仿真交易的各个细节, 包括风控管理, 佣金配置, 执行器配置, 过滤器配置, 开平策略配置等. 因此对其他基础的配置包括文件加载等将会一笔带过.

CTA仿真策略一般需要获取K线数据, 这就要求必须打开数据落地程序. (即接收交易所数据保存到本地, 并将数据广播到本地端口, 供其他程序使用)

其实有两个程序都可以实现这一功能, 一个是 QuoteFactory, 另一个是 TestDtPorter, 而 TestDtPorter 主要是方便上层调用的, 因此建议大家使用 QuoteFactory 打开数据落地程序.

这里给出我使用的相关配置(7*24小时, openctp).

若出现warning, 则将 "Storage" 文件目录删掉然后重新启动即可

为了方便调试, 我对 "WtStraDualThrust" 做了细微改动, 主要是打印回调记录, 源码如下

1."common" 文件夹内容和 "QuoteFactory/common" 保持一致即可

不仅要保证 on_tick 回调成功, 更要保证分钟闭合后 on_schedule 和 on_bar 回调成功

**Examples:**

Example 1 (unknown):
```unknown
{
    "CFFEX": {
        "IC": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 200,
            "name": "中证֤",
            "exchg": "CFFEX",
            "session": "TTS24",
            "holiday": "CHINA"
        },
        "IF": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 300,
            "name": "沪深",
            "exchg": "CFFEX",
            "session": "TTS24",
            "holiday": "CHINA"
        },
        "IH": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 300,
            "name": "上证",
            "exchg": "CFFEX",
            "session": "TTS24",
            "holiday": "CHINA"
        }
    }
}
```

Example 2 (unknown):
```unknown
{
    "CFFEX": {
        "IC": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 200,
            "name": "中证֤",
            "exchg": "CFFEX",
            "session": "TTS24",
            "holiday": "CHINA"
        },
        "IF": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 300,
            "name": "沪深",
            "exchg": "CFFEX",
            "session": "TTS24",
            "holiday": "CHINA"
        },
        "IH": {
            "covermode": 0,
            "pricemode": 0,
            "category": 1,
            "precision": 1,
            "pricetick": 0.2,
            "volscale": 300,
            "name": "上证",
            "exchg": "CFFEX",
            "session": "TTS24",
            "holiday": "CHINA"
        }
    }
}
```

Example 3 (unknown):
```unknown
{
    "CFFEX": {
        "IC2203": {
            "name": "中证2203",
            "code": "IC2203",
            "exchg": "CFFEX",
            "product": "IC",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IC2206": {
            "name": "中证֤2206",
            "code": "IC2206",
            "exchg": "CFFEX",
            "product": "IC",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IC2209": {
            "name": "中证֤2209",
            "code": "IC2209",
            "exchg": "CFFEX",
            "product": "IC",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IF2203": {
            "name": "沪深2203",
            "code": "IF2203",
            "exchg": "CFFEX",
            "product": "IF",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IF2206": {
            "name": "沪深2206",
            "code": "IF2206",
            "exchg": "CFFEX",
            "product": "IF",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IF2209": {
            "name": "沪深2209",
            "code": "IF2209",
            "exchg": "CFFEX",
            "product": "IF",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IH2203": {
            "name": "上证֤2203",
            "code": "IH2203",
            "exchg": "CFFEX",
            "product": "IH",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IH2206": {
            "name": "上证֤2206",
            "code": "IH2206",
            "exchg": "CFFEX",
            "product": "IH",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IH2209": {
            "name": "上证֤֤2209",
            "code": "IH2209",
            "exchg": "CFFEX",
            "product": "IH",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        }
    }
}
```

Example 4 (unknown):
```unknown
{
    "CFFEX": {
        "IC2203": {
            "name": "中证2203",
            "code": "IC2203",
            "exchg": "CFFEX",
            "product": "IC",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IC2206": {
            "name": "中证֤2206",
            "code": "IC2206",
            "exchg": "CFFEX",
            "product": "IC",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IC2209": {
            "name": "中证֤2209",
            "code": "IC2209",
            "exchg": "CFFEX",
            "product": "IC",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IF2203": {
            "name": "沪深2203",
            "code": "IF2203",
            "exchg": "CFFEX",
            "product": "IF",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IF2206": {
            "name": "沪深2206",
            "code": "IF2206",
            "exchg": "CFFEX",
            "product": "IF",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IF2209": {
            "name": "沪深2209",
            "code": "IF2209",
            "exchg": "CFFEX",
            "product": "IF",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IH2203": {
            "name": "上证֤2203",
            "code": "IH2203",
            "exchg": "CFFEX",
            "product": "IH",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IH2206": {
            "name": "上证֤2206",
            "code": "IH2206",
            "exchg": "CFFEX",
            "product": "IH",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        },
        "IH2209": {
            "name": "上证֤֤2209",
            "code": "IH2209",
            "exchg": "CFFEX",
            "product": "IH",
            "maxlimitqty": 20,
            "maxmarketqty": 10
        }
    }
}
```

---

## CTA仿真进阶篇3: 策略回调

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file08.html

**Contents:**
- CTA仿真进阶篇3: 策略回调
- on_tick
  - trigger_price
- on_bar
- on_schedule

source: wtcpp/folder03/file08.md

关于策略中 on_tick 触发流程参考文章 "HFT仿真", 这里直接进入 WtCtaRtTicker.cpp.

WtCtaRtTicker 被称为"生产时间步进器"(我也不知道为何叫如此拗口的名字), 它主要的功能就是处理tick事件.

将断点打在 WtCtaRtTicker::on_tick 首行, 然后逐步运行到 trigger_price, 这里 on_tick 首先对数据时间进行了一番检查, 最后调用了 trigger_price, 实际上所有 on_tick 被触发后的动作都在 trigger_price 中完成.

策略文件中除了触发 on_tick, 最重要的就是触发 on_bar 和 on_schedule, 这两个接口是在分钟结束后才会触发

这里和 HFT 的回调流程基本一致, 但是由于 CTA 只能订阅一个K线, 因此只会有一个K线闭合.

on_bar 执行完毕, 回到 "WtCtaTicker.cpp", 执行 _engine->on_schedule. 这里对应 HFT 的 on_schedule 方法被取消了.

进入 CtaStraContext::on_calculate, 回调策略的 on_schedule

---

## CTA仿真进阶篇2: 策略初始化

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file07.html

**Contents:**
- CTA仿真进阶篇2: 策略初始化
- on_init
- stra_get_ticks
  - 运行流程
- stra_get_bars
- stra_sub_ticks
- 总结 CTA 和 HFT 策略初始化区别

source: wtcpp/folder03/file07.md

为甚么策略初始化总是要单独讲解呢, 因为策略初始化对后续触发 on_tick 和 on_shcedule 和 on_bar 很重要

为什么控制台显示的 on_tick 回调只有 CFFEX.IH.2203, on_bar 回调只有 CFFEX.IC.2203 呢?(这里和 HFT 仿真区别很大) 接下来详细解析这三步都做了哪些事情

切记程序调试时打开 QuoteFactory

这里和之前对比, 主要多出一步, 将品种名称放在 CtaStraBaseCtx 下的 _tick_subs 中, 这点很重要.

订阅数据路径都已一模一样的, 唯一的区别是 "CtaStraBaseCtx.cpp" 和 "HftStraBaseCtx.cpp" 下的 stra_get_bars 方法中, 很明显 CTA 的更复杂, 首先会对主 K 做判断, 这点限制了 CTA 只能订阅一个品种的 K 线, 其次对数据是否获取成功做判断, 这点限制了 CTA 必须要有数据. HFT 对此都无要求.

**Examples:**

Example 1 (unknown):
```unknown
void WtStraDualThrust::on_init(ICtaStraCtx* ctx)
{
	ctx->stra_log_info(fmt::format("回调 on_init, date: {}, time: {}", ctx->stra_get_date(), ctx->stra_get_time()).c_str());

	std::string code = _code;
	if (_isstk)
		code += "-";

	// 获取 CFFEX.IC.2203 的tick数据
	WTSTickSlice* ticks = ctx->stra_get_ticks(_code.c_str(), 30);
	if (ticks)
		ticks->release();

	// 获取 CFFEX.IC.2203 的bar数据
	WTSKlineSlice *kline1 = ctx->stra_get_bars(code.c_str(), _period.c_str(), _count, true);
	if (kline1 == NULL)
	{
		//这里可以输出一些日志
		return;
	}
	kline1->release();

	// 主动订阅 CFFEX.IH.2203 tick数据
	ctx->stra_sub_ticks("CFFEX.IH.2203");
}
```

Example 2 (unknown):
```unknown
void WtStraDualThrust::on_init(ICtaStraCtx* ctx)
{
	ctx->stra_log_info(fmt::format("回调 on_init, date: {}, time: {}", ctx->stra_get_date(), ctx->stra_get_time()).c_str());

	std::string code = _code;
	if (_isstk)
		code += "-";

	// 获取 CFFEX.IC.2203 的tick数据
	WTSTickSlice* ticks = ctx->stra_get_ticks(_code.c_str(), 30);
	if (ticks)
		ticks->release();

	// 获取 CFFEX.IC.2203 的bar数据
	WTSKlineSlice *kline1 = ctx->stra_get_bars(code.c_str(), _period.c_str(), _count, true);
	if (kline1 == NULL)
	{
		//这里可以输出一些日志
		return;
	}
	kline1->release();

	// 主动订阅 CFFEX.IH.2203 tick数据
	ctx->stra_sub_ticks("CFFEX.IH.2203");
}
```

Example 3 (javascript):
```javascript
WTSKlineSlice* CtaStraBaseCtx::stra_get_bars(const char* stdCode, const char* period, uint32_t count, bool isMain /* = false */)
{
	std::string key = StrUtil::printf("%s#%s", stdCode, period);
	if (isMain)
	{
		if (_main_key.empty())
		{
			_main_key = key;
			log_debug("Main KBars confirmed：%s", key.c_str());
		}
		else if (_main_key != key)
			throw std::runtime_error("Main KBars already confirmed");
	}

	std::string basePeriod = "";
	uint32_t times = 1;
	if (strlen(period) > 1)
	{
		basePeriod.append(period, 1);
		times = strtoul(period + 1, NULL, 10);
	}
	else
	{
		basePeriod = period;
	}

	WTSKlineSlice* kline = _engine->get_kline_slice(_context_id, stdCode, basePeriod.c_str(), count, times);
	if(kline)
	{
	// 代码略
	}
```

Example 4 (javascript):
```javascript
WTSKlineSlice* CtaStraBaseCtx::stra_get_bars(const char* stdCode, const char* period, uint32_t count, bool isMain /* = false */)
{
	std::string key = StrUtil::printf("%s#%s", stdCode, period);
	if (isMain)
	{
		if (_main_key.empty())
		{
			_main_key = key;
			log_debug("Main KBars confirmed：%s", key.c_str());
		}
		else if (_main_key != key)
			throw std::runtime_error("Main KBars already confirmed");
	}

	std::string basePeriod = "";
	uint32_t times = 1;
	if (strlen(period) > 1)
	{
		basePeriod.append(period, 1);
		times = strtoul(period + 1, NULL, 10);
	}
	else
	{
		basePeriod = period;
	}

	WTSKlineSlice* kline = _engine->get_kline_slice(_context_id, stdCode, basePeriod.c_str(), count, times);
	if(kline)
	{
	// 代码略
	}
```

---

## HFT仿真进阶3: 策略回调

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file05.html

**Contents:**
- HFT仿真进阶3: 策略回调
- 解决目标
- 仿真运行流程
  - 结构梳理
  - Tick数据传递流程
  - on_bar闭合流程
    - onMinuteEnd
    - on_minute_end
    - on_session_end
- 仿真流程总结

source: wtcpp/folder03/file05.md

承接上文 "HFT仿真进阶2: 策略初始化"

本文主要解决的问题是仿真时的策略运行逻辑, 包括如何回调各类策略接口

在第5步 WtHftRtTicker::on_tick 中

WtHftRtTicker::run() 中线程死循环执行中… 当出现新的一分钟, 正常会顺序执行三句代码

在第1步 WtDataReader::onMinuteEnd 中

在第4步 WtHftEngine::on_bar中

原本会执行 on_schedule, 在 HFT策略中被去掉了

**Examples:**

Example 1 (unknown):
```unknown
_store->onMinuteEnd(_date, thisMin);
_engine->on_minute_end(_date, thisMin);
_engine->on_session_end();
```

Example 2 (unknown):
```unknown
_store->onMinuteEnd(_date, thisMin);
_engine->on_minute_end(_date, thisMin);
_engine->on_session_end();
```

---

## 数据压缩/解压

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder02/file03.html

**Contents:**
- 数据压缩/解压
- WonderTrader数据存储方式
  - 文件存储
  - 数据库存储
  - csv历史数据
- 准备数据
- 创建项目环境
- 编写压缩解压文件代码
  - 数据压缩和解压
    - 1. 准备目录

source: wtcpp/folder02/file03.md

数据转换功能主要在 WtDtHelper.cpp

所有日期和时间都是数值型时间, 其中bar数据time字段需要计算,精确到分; tick数据action_time字段需要计算, 精确到毫秒

WonderTrader实盘环境下支持两种数据存储方式：文件存储和数据库存储。而回测环境下，还支持直接从csv读取数据（仅限于历史K线数据）。

历史K线数据文件，采用zstd压缩后存放。高频历史数据，包括tick数据，股票level2的委托明细、成交明细、委托队列，也采用压缩存放的方式。A股全市场一天的level2数据，压缩以后也就是大概2G不到的样子，对于硬盘来说是相当友好的。 实时数据文件，因为需要实时读写，所以不压缩数据结构，并采用mmap的方式映射到内存中，直接对文件进行读写，提高读写效率。

数据库存储的方式，只针对历史K线数据。实时数据和高频数据的存储方式还是和文件存储模式一致的。 主要考虑到高频数据量非常巨大，如果采用数据库，整个数据库的运行效率会大大降低。而实时数据对延迟要求非常高，数据库则不适合这样的应用场景了。目前支持的数据库是mysql/mariadb，以后如果有需求的话，可能会扩展到一些nosql数据库。

很多用户通过各种渠道获取到的历史数据，供应商为了便于用户直接查看数据，一般都会提供csv格式的。但是csv文件格式的数据，占用空间非常大，而且直接从csv文件读取数据的开销也是非常大的。 WonderTrader的回测框架为了尽量减少这种不必要的开销，在处理csv文件时，第一次会直接从csv文件读取，将读取的数据转成WonderTrader内部数据结构之后，会将数据转储为WonderTrader自有的压缩存放格式。这样下次在使用该数据的时候，读取压缩存放的数据以后，直接解压就可以得到结构化的历史数据，这样就可以直接进行访问了。

建议使用Python, 处理数据用Python最方便. 若不是本人不熟悉cpp和py混合编程, 也许本章数据处理内容全部会使用Python代码

wt数据向右对齐, 即5分钟bar数据9:10对应的是9:05-9:10这段时间的高开低收价.(如果你的数据是左对齐, 需自己修改, 参考如下代码)

在TestDtHelper.cpp添加测试代码确保项目环境配置无误(如果编译不通过建议检查环境, 不要再往下了)

WtDtHelper.cpp 文件有几个重要函数

在 "Debug/TestDtHelper" 目录下新建

将准备的分钟csv文件放入csv对应目录下

bin下将会生成csv转dsb文件, csv/out会保存dsb转csv结果

在 TestDtHelper.cpp 中编写代码, 主要逻辑是创建bar结构体vector std::vector<WTSBarStruct> bars;, 然后填充结构体, 最后调用 store_bars 即可. 注意填充bar结构体时对应列名.

**Examples:**

Example 1 (unknown):
```unknown
if timeframe == TIMEFRAME_M1:
    delta_min = 1
elif timeframe == TIMEFRAME_M5:
    delta_min = 5
rates["time"] = rates["time"].map(lambda x: x + timedelta(minutes=delta_min))
```

Example 2 (unknown):
```unknown
if timeframe == TIMEFRAME_M1:
    delta_min = 1
elif timeframe == TIMEFRAME_M5:
    delta_min = 5
rates["time"] = rates["time"].map(lambda x: x + timedelta(minutes=delta_min))
```

Example 3 (cpp):
```cpp
#pragma once
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include "../../WtDtHelper/WtDtHelper.cpp"
#include "../../WTSTools/CsvHelper.h"
#include "../../Includes/WTSStruct.h"

int main()
{
	return 0;
}
```

Example 4 (cpp):
```cpp
#pragma once
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include "../../WtDtHelper/WtDtHelper.cpp"
#include "../../WTSTools/CsvHelper.h"
#include "../../Includes/WTSStruct.h"

int main()
{
	return 0;
}
```

---

## CTA仿真完整篇2: 初始化

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file10.html

**Contents:**
- CTA仿真完整篇2: 初始化
- 修改策略文件
- 文件初始化
- 策略初始化
  - 确定主K
  - 获取K线数据
  - 订阅tick

source: wtcpp/folder03/file10.md

给的demo配置文件有错误, 因此上一篇文章中出现的配置文件内容尽量复制粘贴

源码也有错误, 具体在 “WtRunner.cpp” 文件222行, 应该是 initExecuters(时间20220320, 该问题已提交issue, 注意检查自己下载的源码是否已修改)

为了方便测试(这句话已经重复N遍), 首先需要修改策略文件.

修改项目"Plugins/WtCtaStraFact/DualThrust/WtStraDualThrust.cpp", 文件内容如下(在上一篇文章的基础上, 简化on_schedule内容, 直接下单.)

修改完策略源码后, 右击项目"WtCtaStraFact", 重新生成, 然后将"Debug"目录下的"WtCtaStraFact.dll"复制到"Debug/WtRunner/cta"目录下.

启动"QuoteFactory.exe", 准备测试"WtRunner"

其他文件初始化在之前许多文章中都讲过, 因此不再赘述. 将断点打在"WtRunner.cpp"222行(对, 就是前面提示源码有错误的那一行), 然后进入WtRunner::initExecuters

"Executers"是CTA策略的执行器, 主要是为了实现多个CTA策略组合(这是CTA策略组合管理的核心, 也是比HFT复杂的原因)

这部分内容在之前的文章也重复过多次, 这里再简要啰嗦一下.

将断点打在WtStraDualThrust::on_init下的WTSKlineSlice *kline = ctx->stra_get_bars这句代码上

进入 CtaStraBaseCtx::stra_get_bars , 首先会检查是否有主K. 如果主K不存在(首次进入), 默认当前品种是主K; 如果主K存在, 会抛出异常.

因此CTA策略中只能有一个品种作为主K, 如果想订阅多个品种K线, 注意isMain, 这个参数, 订阅其他K线需要设置为false, (默认也是false)

当执行到 _engine->get_kline_slice 这句代码时, 会从本地读取历史K线数据, 如果读取不到, 后边逻辑也就不会实现, 因此在需要K线的策略中(不仅CTA, 还包括HFT), 必须打开 "QuoteFactory"

注意揣摩这句话, 在我看来CTA和HFT只是策略形式的符号, 并不是某种特定策略类型的称谓.

结束 CtaStraBaseCtx::stra_get_bars 函数, 回到 WtStraDualThrust::on_init, 走到 ctx->stra_sub_ticks, 这里会将品种代码添加到_tick_subs中.

如果不主动订阅tick, 该品种后续就不会执行策略中的 on_tick 回调.(逻辑在之前的文章中有详细描述)

**Examples:**

Example 1 (javascript):
```javascript
#include "WtStraDualThrust.h"

#include "../Includes/ICtaStraCtx.h"

#include "../Includes/WTSContractInfo.hpp"
#include "../Includes/WTSVariant.hpp"
#include "../Includes/WTSDataDef.hpp"
#include "../Share/decimal.h"

extern const char* FACT_NAME;

//By Wesley @ 2022.01.05
#include "../Share/fmtlib.h"

WtStraDualThrust::WtStraDualThrust(const char* id)
	: CtaStrategy(id)
{
}

WtStraDualThrust::~WtStraDualThrust()
{
}

const char* WtStraDualThrust::getFactName()
{
	return FACT_NAME;
}

const char* WtStraDualThrust::getName()
{
	return "DualThrust";
}

bool WtStraDualThrust::init(WTSVariant* cfg)
{
	if (cfg == NULL)
		return false;

	_days = cfg->getUInt32("days");
	_k1 = cfg->getDouble("k1");
	_k2 = cfg->getDouble("k2");

	_period = cfg->getCString("period");
	_count = cfg->getUInt32("count");
	_code = cfg->getCString("code");

	_isstk = cfg->getBoolean("stock");

	return true;
}

void WtStraDualThrust::on_schedule(ICtaStraCtx* ctx, uint32_t curDate, uint32_t curTime)
{
	ctx->stra_log_info(fmt::format("策略回调on_schedule, {}", ctx->stra_get_time()).c_str());

	std::string code = _code;
	if (_isstk)
		code += "-";
	WTSKlineSlice *kline = ctx->stra_get_bars(code.c_str(), _period.c_str(), _count, true);
	if(kline == NULL)
	{
		//这里可以输出一些日志
		return;
	}

	if (kline->size() == 0)
	{
		kline->release();
		return;
	}

	//这个释放一定要做
	kline->release();

	// 每笔交易手数
	uint32_t trdUnit = 1;
	if (_isstk)
		trdUnit = 100;

	double curPos = ctx->stra_get_position(_code.c_str()) / trdUnit;
	if(decimal::eq(curPos,0))
	{
		ctx->stra_enter_long(_code.c_str(), 2 * trdUnit, "DT_EnterLong");
		//向上突破
		ctx->stra_log_info(fmt::format("多仓进场").c_str());

	}
	else if (decimal::gt(curPos, 1))
	{

		//多仓出场
		ctx->stra_exit_long(_code.c_str(), 2 * trdUnit, "DT_ExitLong");
		ctx->stra_log_info(fmt::format("多单离场").c_str());

		ctx->stra_save_user_data("test", "waht");
	}

}

void WtStraDualThrust::on_init(ICtaStraCtx* ctx)
{
	ctx->stra_log_info(fmt::format("策略回调on_init, {}", ctx->stra_get_time()).c_str());

	std::string code = _code;
	if (_isstk)
		code += "-";
	WTSKlineSlice *kline = ctx->stra_get_bars(code.c_str(), _period.c_str(), _count, true);
	if (kline == NULL)
	{
		//这里可以输出一些日志
		return;
	}

	ctx->stra_sub_ticks(code.c_str());

	kline->release();
}

void WtStraDualThrust::on_tick(ICtaStraCtx* ctx, const char* stdCode, WTSTickData* newTick)
{
	//没有什么要处理
	ctx->stra_log_info(fmt::format("策略回调on_tick, {}", ctx->stra_get_time()).c_str());
}
```

Example 2 (javascript):
```javascript
#include "WtStraDualThrust.h"

#include "../Includes/ICtaStraCtx.h"

#include "../Includes/WTSContractInfo.hpp"
#include "../Includes/WTSVariant.hpp"
#include "../Includes/WTSDataDef.hpp"
#include "../Share/decimal.h"

extern const char* FACT_NAME;

//By Wesley @ 2022.01.05
#include "../Share/fmtlib.h"

WtStraDualThrust::WtStraDualThrust(const char* id)
	: CtaStrategy(id)
{
}

WtStraDualThrust::~WtStraDualThrust()
{
}

const char* WtStraDualThrust::getFactName()
{
	return FACT_NAME;
}

const char* WtStraDualThrust::getName()
{
	return "DualThrust";
}

bool WtStraDualThrust::init(WTSVariant* cfg)
{
	if (cfg == NULL)
		return false;

	_days = cfg->getUInt32("days");
	_k1 = cfg->getDouble("k1");
	_k2 = cfg->getDouble("k2");

	_period = cfg->getCString("period");
	_count = cfg->getUInt32("count");
	_code = cfg->getCString("code");

	_isstk = cfg->getBoolean("stock");

	return true;
}

void WtStraDualThrust::on_schedule(ICtaStraCtx* ctx, uint32_t curDate, uint32_t curTime)
{
	ctx->stra_log_info(fmt::format("策略回调on_schedule, {}", ctx->stra_get_time()).c_str());

	std::string code = _code;
	if (_isstk)
		code += "-";
	WTSKlineSlice *kline = ctx->stra_get_bars(code.c_str(), _period.c_str(), _count, true);
	if(kline == NULL)
	{
		//这里可以输出一些日志
		return;
	}

	if (kline->size() == 0)
	{
		kline->release();
		return;
	}

	//这个释放一定要做
	kline->release();

	// 每笔交易手数
	uint32_t trdUnit = 1;
	if (_isstk)
		trdUnit = 100;

	double curPos = ctx->stra_get_position(_code.c_str()) / trdUnit;
	if(decimal::eq(curPos,0))
	{
		ctx->stra_enter_long(_code.c_str(), 2 * trdUnit, "DT_EnterLong");
		//向上突破
		ctx->stra_log_info(fmt::format("多仓进场").c_str());

	}
	else if (decimal::gt(curPos, 1))
	{

		//多仓出场
		ctx->stra_exit_long(_code.c_str(), 2 * trdUnit, "DT_ExitLong");
		ctx->stra_log_info(fmt::format("多单离场").c_str());

		ctx->stra_save_user_data("test", "waht");
	}

}

void WtStraDualThrust::on_init(ICtaStraCtx* ctx)
{
	ctx->stra_log_info(fmt::format("策略回调on_init, {}", ctx->stra_get_time()).c_str());

	std::string code = _code;
	if (_isstk)
		code += "-";
	WTSKlineSlice *kline = ctx->stra_get_bars(code.c_str(), _period.c_str(), _count, true);
	if (kline == NULL)
	{
		//这里可以输出一些日志
		return;
	}

	ctx->stra_sub_ticks(code.c_str());

	kline->release();
}

void WtStraDualThrust::on_tick(ICtaStraCtx* ctx, const char* stdCode, WTSTickData* newTick)
{
	//没有什么要处理
	ctx->stra_log_info(fmt::format("策略回调on_tick, {}", ctx->stra_get_time()).c_str());
}
```

Example 3 (javascript):
```javascript
// 1. executers.yaml 配置文件内容
WTSVariant* cfgItem = cfgExecuter->get(idx);
if (!cfgItem->getBoolean("active"))
    continue;

const char* id = cfgItem->getCString("id");
bool bLocal = cfgItem->getBoolean("local");

if (bLocal)
{
    // 2. 创建本地执行器对象
    WtLocalExecuter* executer = new WtLocalExecuter(&_exe_factory, id, &_data_mgr);
    if (!executer->init(cfgItem))
        return false;

    // 3. 将执行器和交易接口相互绑定
    TraderAdapterPtr trader = _traders.getAdapter(cfgItem->getCString("trader"));
    executer->setTrader(trader.get());
    trader->addSink(executer);
    // 4. 将执行器添加到cta引擎中
    _cta_engine.addExecuter(ExecCmdPtr(executer));
}
```

Example 4 (javascript):
```javascript
// 1. executers.yaml 配置文件内容
WTSVariant* cfgItem = cfgExecuter->get(idx);
if (!cfgItem->getBoolean("active"))
    continue;

const char* id = cfgItem->getCString("id");
bool bLocal = cfgItem->getBoolean("local");

if (bLocal)
{
    // 2. 创建本地执行器对象
    WtLocalExecuter* executer = new WtLocalExecuter(&_exe_factory, id, &_data_mgr);
    if (!executer->init(cfgItem))
        return false;

    // 3. 将执行器和交易接口相互绑定
    TraderAdapterPtr trader = _traders.getAdapter(cfgItem->getCString("trader"));
    executer->setTrader(trader.get());
    trader->addSink(executer);
    // 4. 将执行器添加到cta引擎中
    _cta_engine.addExecuter(ExecCmdPtr(executer));
}
```

---

## HFT回测进阶2: 源码解析

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder01/file03.html

**Contents:**
- HFT回测进阶2: 源码解析
- 本文主要解决问题
- replayer.prepare()
  - 关键代码 handle_init()
    - 数据加载路径
- replayer.run()
  - 主K回放

source: wtcpp/folder01/file03.md

建议先阅读 “HFT回测进阶1: 源码解析”

请使用最新版0.9dev源码(20220303)

WtBtRunner.cpp 文件中两句代码最重要

进入 bool HisDataReplayer::prepare()有一句代码最重要

1.进入策略文件 HftMocker::handle_init() -> HftMocker::on_init() -> WtHftStraDemo::on_init

2.加载tick数据 WtHftStraDemo::on_init -> HftMocker::stra_get_ticks -> HisDataReplayer::get_tick_slice -> HisDataReplayer::checkTicks -> HisDataReplayer::cacheRawTicksFromCSV(这里已经开始从csv文件加载tick数据了)

这段csv转dsb逻辑似乎有点问题, csv数据列和tick结构体不好对齐, 最好还是按照文章 “数据压缩/解压” 自己将csv转dsb并放在对应位置

继续往下执行直到再次进入 WtHftStraDemo.cpp

3.加载bar数据 WtHftStraDemo::on_init -> ` HftMocker::stra_get_bars -> HisDataReplayer::get_kline_slice -> HisDataReplayer::cacheRawBarsFromCSV`

继续往下执行直到再次进入 WtHftStraDemo.cpp

4.主动订阅tick数据(也比较关键, 策略中记得订阅) WtHftStraDemo::on_init -> HftMocker::stra_sub_ticks -> HisDataReplayer::sub_tick

1.进入策略文件on_session_begin HisDataReplayer::run_by_bars -> HftMocker::handle_session_begin -> HftMocker::on_session_begin -> _strategy->on_session_begin (策略文件没有实现该方法, 到头了)

继续往下执行直到再次进入HisDataReplayer.cpp

2.进入策略文件on_tick HisDataReplayer::run_by_bars -> HisDataReplayer::replayHftDatas -> HftMocker::handle_tick -> HftMocker::on_tick -> HftMocker::on_tick_updated -> WtHftStraDemo::on_tick

继续往下执行直到第一阶段tick数据回放完毕并再次进入HisDataReplayer::run_by_bars 3.进入策略文件on_bar HisDataReplayer::run_by_bars -> HisDataReplayer::onMinuteEnd -> HftMocker::handle_bar_close -> HftMocker::on_bar -> WtHftStraDemo::on_bar

**Examples:**

Example 1 (unknown):
```unknown
replayer.prepare();
replayer.run();
```

Example 2 (unknown):
```unknown
replayer.prepare();
replayer.run();
```

Example 3 (unknown):
```unknown
_listener->handle_init();
```

Example 4 (unknown):
```unknown
_listener->handle_init();
```

---

## CTA仿真完整篇1: 环境准备

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder03/file09.html

**Contents:**
- CTA仿真完整篇1: 环境准备
- 生成解决方案
- QuoteFactory
  - 添加文件
  - 修改配置
- WtRunner
  - 添加文件
- 修改文件
  - 修改策略
- 测试成功

source: wtcpp/folder03/file09.md

之前的文章都是基于0.9dev版本的, 最近WT0.9稳定版更新了, 趁着这个机会, 我下载了WT0.9master版本, 并开始重新梳理WT项目逻辑.

首先需要先配置好"QuoteFactory"

进入"wondertrader-master/dist/QuoteFactory"目录下, 将所有yaml文件复制粘贴到"wondertrader-master/src/x64/Debug/QuoteFactory"目录下.

进入"wondertrader-master/dist/"目录下, 将"common"整个文件夹复制粘贴到"wondertrader-master/src/x64/Debug/"目录下.

进入openctp官网下载对应版本(6.3.15)的"thostmduserapi_se.dll"放到"QuoteFactory/parser"目录下

dtcfg.yaml 这个是该程序主要配置项, 添加一个字段 allday: true 即可

mdparsers.yaml 本地行情端口(localtime: true)

为方便随时测试, 我们需要添加一个全天候盘的交易时段

../common/session.json

然后将对应的品种也修改为该交易时段 "session": "ALLDAY"

../common/commodities.json

运行程序即可, 运行结果应该大致如下(如出现警告, 删除"FUT_Data"文件夹即可)

wtRunner配置文件参考 "wondertrader-master/dist/WtRunnerCta", 依旧是全部复制

进入openctp官网下载对应版本(6.3.15)的"thostmduserapi_se.dll"放到"WtRunner/parser"目录下, "thosttraderapi_se.dll"放到"WtRunner/traders"目录下

config.yaml 是该程序主要配置文件, 修改品种为 "SHFE.au.2206"(建议复制粘贴)

tdparsers.yaml 配置文件如下

tdtraders.yaml 配置文件如下

executers.yaml(建议复制粘贴)

WT自带的CTA策略输出的信息太少, 我们需要略微修改下, 方便调试

打开 "Plugins/WtCtaStraFact/WtStraDualThrust.cpp"

4.右击项目"Plugins/WtCtaStraFact", 重新生成, 然后在"Debug/"目录下找到"WtCtaStraFact.dll", 放到"WtRunner/cta/"目录下(替换)

**Examples:**

Example 1 (unknown):
```unknown
basefiles:
    commodity: ../common/commodities.json
    contract: ../common/contracts.json
    holiday: ../common/holidays.json
    session: ../common/sessions.json
broadcaster:
    active: true
    bport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
allday: true
parsers: mdparsers.yaml
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 100
    path: ../FUT_Data
    savelog: true
```

Example 2 (unknown):
```unknown
basefiles:
    commodity: ../common/commodities.json
    contract: ../common/contracts.json
    holiday: ../common/holidays.json
    session: ../common/sessions.json
broadcaster:
    active: true
    bport: 3997
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
allday: true
parsers: mdparsers.yaml
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 100
    path: ../FUT_Data
    savelog: true
```

Example 3 (unknown):
```unknown
parsers:
-   active: true
    broker: ''
    code: SHFE.au2206,SHFE.au2208  # 品种过滤
    front: ttcp://122.51.136.165:20004
    id: parser
    localtime: true
    module: ParserCTP
    pass: ******        # openctp 账户密码
    user: ******
```

Example 4 (unknown):
```unknown
parsers:
-   active: true
    broker: ''
    code: SHFE.au2206,SHFE.au2208  # 品种过滤
    front: ttcp://122.51.136.165:20004
    id: parser
    localtime: true
    module: ParserCTP
    pass: ******        # openctp 账户密码
    user: ******
```

---

## CTA回测详解

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder01/file01.html

**Contents:**
- CTA回测详解
- 准备工作
  - 需要先完成生成解决方案
  - 1. 配置文件
  - 2. 数据文件
  - 3. 策略工程(dll文件，需要完成WtCtaStraFact工程的编译)
  - 文件结构
  - 编译运行程序（WtBtRunner工程）
- 逐步解析
  - 1. 加载 logcfgbt.yaml

source: wtcpp/folder01/file01.md

以X64 Debug编译为例，需要添加的配置文件的文件结构如下

将WtBtRunner工程设置为启动项目，并将项目属性-配置属性-调试-工作目录设置为$(OutDir)

总结: 调度器中的 _replayer 是回放器, 回放器中 _listener 是调度器

WTCPP中的日期和时间常用整数型, 如整数 20220211 表示 2022年2月11日, 整数 0910 表示 9:10, 在阅读源码过程中需要注意这种日期时间格式的精度

逻辑比较复杂, 可查看源码注释(搜索框输入: WTSBaseDataMgr::calcTradingDate, 如果注释有误, 欢迎QQ群提出)

调度器中的 handle_init 会调用 this-&gt;on_init();, 而 on_init() 又调用 _strategy-&gt;on_init(this);(注意: 这里又把调度器对象传递到策略中去了, 所以记得策略文件中的参数 CtaMocker 是怎么来的了吧。。。山路十八弯~)

数据加载逻辑重要但是复杂(见下一部分内容)

实际就是K线周期大小的比较过程, 最小的K线周期作为主K.(这里的逻辑可能有点小问题)

checkUnbars函数第一句代码就是遍历 _tick_sub_map, 但是 _tick_sub_map从哪来?

数据回放器准备阶段 bool HisDataReplayer::prepare() 中有两句代

整个逻辑在回放器 HisDataReplayer, 调度器 CtaMocker 和策略对象 WtStraDualThrust 中来回切换, 感兴趣建议自己打断点细细品味.

还是 _tick_sub_map 订阅的那条路径, 不过从第4步开始分叉, 在 WTSKlineSlice* kline = _replayer->get_kline_slice中进入到了回放器获取K线切片. 这里在加载数据的过程中填充了 bars_cache.

在replayer.run()中，如果订阅了主K线，则按照主K线进行回放，执行run_by_bars。

run_by_bars位于HisDataReplayer.cpp中，位于WtBtCore工程下，下面将逐行解析run_by_bars的回测逻辑。

如果没有订阅K线，且tick回测是打开的，则按照每日的tick进行回放

run_by_ticks位于HisDataReplayer.cpp中，位于WtBtCore工程下，run_by_ticks较run_by_bars简单，下面将逐行解析run_by_ticks的回测逻辑。

时间调度任务不为空,则按照时间调度任务回放

run_by_tasks位于HisDataReplayer.cpp中，位于WtBtCore工程下，下面将逐行解析run_by_tasks的回测逻辑。

回测中的订单撮合主要在on_tick事件中执行，这里的on_tick事件不是指策略中的on_tick，而是指调度器CtaMocker中的on_tick，由handle_tick触发。主要完成以下工作

当在策略中调用stra_enter_long/stra_enter_short/stra_exit_long/stra_exit_short 等函数时，会进行以下流程：

如果没有指定条件单价格，则视为动态下单，直接添加到信号列表中。

如果指定了条件单价格，则视为条件下单，根据信号方向生成条件单对象，记录目标价格以及触发规则

WCT_Equal, //等于 WCT_Larger, //大于 WCT_Smaller, //小于 WCT_LargerOrEqual, //大于等于 WCT_SmallerOrEqual //小于等于

信号列表以及停止单列表，将会依次在on_tick事件中处理。

在on_tick中，首先处理信号列表，通过执行do_set_position函数，将当前持仓调整到信号期望的目标仓位。

除了在策略代码中，直接调用stra_enter_long/stra_enter_short/stra_exit_long/stra_exit_short进行动态下单直接添加到信号列表中外，在on_tick中对停止单进行检查，如果判断触发，生成信号并添加到信号列表中。这个信号会在下一轮on_tick中被执行。

**Examples:**

Example 1 (unknown):
```unknown
replayer:                   # 回放器配置，包块数据格式、存放路径、交易费用等
    mode: csv               # 获取数据的模式，如csv，bin等
    path: ./storage/        # 数据存放的路径
    stime: 201905010900     # 回测开始时间
    etime: 201910011500     # 回测结束时间
    basefiles:              # common下的配置文件路径，(ToDo:每个文件的意义单独展开介绍)   
        session: ./common/sessions.json
        commodity: ./common/commodities.json
        contract: ./common/contracts.json
        holiday: ./common/holidays.json
        hot: ./common/hots.json
    fees: ./common/fees.json
env:                # 环境设置，与交易品种和撮合有关
    mocker: cta     # 模式
    slippage: 1     # 滑点
cta:                # 策略工厂配置
    module: WtCtaStraFact.dll       # 策略生成器模块名
    strategy:                       # 策略配置
        id: dt_if                   # 策略id
        name: DualThrust            # 策略名
        params:                     # 策略参数
            code: CFFEX.IF.HOT
            count: 50
            period: m5
            days: 30
            k1: 0.6
            k2: 0.6
            stock: false
```

Example 2 (unknown):
```unknown
replayer:                   # 回放器配置，包块数据格式、存放路径、交易费用等
    mode: csv               # 获取数据的模式，如csv，bin等
    path: ./storage/        # 数据存放的路径
    stime: 201905010900     # 回测开始时间
    etime: 201910011500     # 回测结束时间
    basefiles:              # common下的配置文件路径，(ToDo:每个文件的意义单独展开介绍)   
        session: ./common/sessions.json
        commodity: ./common/commodities.json
        contract: ./common/contracts.json
        holiday: ./common/holidays.json
        hot: ./common/hots.json
    fees: ./common/fees.json
env:                # 环境设置，与交易品种和撮合有关
    mocker: cta     # 模式
    slippage: 1     # 滑点
cta:                # 策略工厂配置
    module: WtCtaStraFact.dll       # 策略生成器模块名
    strategy:                       # 策略配置
        id: dt_if                   # 策略id
        name: DualThrust            # 策略名
        params:                     # 策略参数
            code: CFFEX.IF.HOT
            count: 50
            period: m5
            days: 30
            k1: 0.6
            k2: 0.6
            stock: false
```

Example 3 (unknown):
```unknown
root:                   # 根日志对象的配置，通常是框架信息
    level: debug        # 日志等级
    async: false        #是否异步
    sinks:              # 输出流的配置，即日志信息会被输出到哪些位置
    -   type: basic_file_sink                           # 类型，基本的文件输出
        filename: BtLogs/Runner.log                     # 日志文件保存位置
        pattern: '[%Y.%m.%d %H: %M: %S - %-5l] %v'      # 日志输出模板
        truncate: true                                  # 日志是否自动截断，截断可以避免日志文件过大
        type: console_sink                              # 输出到控制台
        pattern: '[%m.%d %H: %M: %S - %^%-5l%$] %v'
dyn_pattern: 
    strategy: 
        level: debug
        async: false
        sinks: 
        -   type: basic_file_sink
            filename: BtLogs/Strategy_%s.log
            pattern: '[%Y.%m.%d %H: %M: %S - %-5l] %v'
            truncate: false
```

Example 4 (unknown):
```unknown
root:                   # 根日志对象的配置，通常是框架信息
    level: debug        # 日志等级
    async: false        #是否异步
    sinks:              # 输出流的配置，即日志信息会被输出到哪些位置
    -   type: basic_file_sink                           # 类型，基本的文件输出
        filename: BtLogs/Runner.log                     # 日志文件保存位置
        pattern: '[%Y.%m.%d %H: %M: %S - %-5l] %v'      # 日志输出模板
        truncate: true                                  # 日志是否自动截断，截断可以避免日志文件过大
        type: console_sink                              # 输出到控制台
        pattern: '[%m.%d %H: %M: %S - %^%-5l%$] %v'
dyn_pattern: 
    strategy: 
        level: debug
        async: false
        sinks: 
        -   type: basic_file_sink
            filename: BtLogs/Strategy_%s.log
            pattern: '[%Y.%m.%d %H: %M: %S - %-5l] %v'
            truncate: false
```

---

## 数据进阶: 全天候环境准备

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder02/file06.html

**Contents:**
- 数据进阶: 全天候环境准备
- 规避状态机检测
  - 如何规避状态机检测呢?
  - 节假日规避
- 注意事项
- 规避仿真时间校验

source: wtcpp/folder02/file06.md

如果你只是在盘中正常交易时间段做测试, 本章内容可忽略

本章内容多处牵涉到非正常修改源码部分, 小白慎入!

WT对数据时间的检查极为严苛, 这对实盘自然是非常好的, 但是却给非交易时段做测试带来诸多不便, 本章内容是本人在非交易时段做测试时的踩坑记录.

执行 TestDtPorter 下的 main.cpp

在之前的文章 "UDP数据转发" 中提到了如何随时获取数据, 之后在 "状态机详解" 中又提到了状态机工作机制. 随着测试的深入, 我发现自己可能随时需要某个品种的 dmb和dsb数据文件(仿真测试需要), 这就不得不使用状态机的收盘作业机制帮我准备数据(即我需要随时模拟收盘作业)

我们仍然使用openctp的7*24小时环境, 同时在 "状态机详解"的基础上修改三个文件

offset 即将所有时间向前偏移480分钟, 因为wt处理夜盘机制是通过时间偏移实现的.

auction 集合竞价偏移后变为 1-2(0点01分用数字表示)

sections 交易时段偏移后变为 2-2359(0点02分到23点59分), 这样几乎覆盖全天交易时段

closetime 偏移后变为 2358, proctime 偏移后变为 2359(和上面错了一分钟, 不要觉得奇怪, 代码里计算是如此的)

当电脑时间大于 closetime 时便停止获取数据, 当时间大于proctime 时便开始收盘作业

将你想使用的品种 session 字段修改和 session.json 一一对应, 同时注意 dtcfg.yaml 文件需要将 allday: 配置字段删掉或设为 false

之后运行程序即可, 成功运行截图参考文章"状态机详解"

上述配置还是只能做到5*24小时, 因为状态机还会自动过滤周末和节日

如果是在周末测试, 通过简单的文件配置是行不通的, 还必须修改源码.

在 "WtDtCore/StateMonitor.cpp" 中修改 StateMonitor::run() 方法, 将 isAllHoliday 初始状态由 true 改为 false.

每次修改完源码记得重新生成, 然后将对应的dll文件放到对应的文件夹下

旧版本: 在文章 "对接openctp" 中我提到如何在wt中使用openctp做仿真交易, 当时只是填充了 trading_date 字段, 然而随着源码阅读的深入, 简单的填充已经无法满足 7*24 小时环境测试了, 因为在仿真交易中会对行情的 action_time 字段和电脑当前时间字段做校验, 试想在非交易时段, 服务器回放的都是历史数据, action_time 自然是小于当前时间的, 于是程序便会一直卡住, 不再继续往下了.(虽然给测试造成了很大阻碍, 但是在实盘中还是非常必要的)

新版本: WonderTrader 项目中专门添加了配置字段 "localtime", 用本地时间填充对应的时间字段.

仅仅修改一下行情配置文件 "parsers.yaml" 即可

**Examples:**

Example 1 (unknown):
```unknown
{
    "TTS24":{
        "name":"TTS24测试",
        "offset": -480,
        "auction":{
            "from": 802,
            "to": 803
        },
        "sections":[
            {
                "from": 803,
                "to": 758
            }
        ]
    }
}
```

Example 2 (unknown):
```unknown
{
    "TTS24":{
        "name":"TTS24测试",
        "offset": -480,
        "auction":{
            "from": 802,
            "to": 803
        },
        "sections":[
            {
                "from": 803,
                "to": 758
            }
        ]
    }
}
```

Example 3 (unknown):
```unknown
TTS24:
    closetime: 758     # 停止获取行情时间(会偏移)
    inittime: 802      # 开盘前准备时间(会偏移)
    name: TTS24测试
    proctime: 759      # 处理收盘作业时间(会偏移)
```

Example 4 (unknown):
```unknown
TTS24:
    closetime: 758     # 停止获取行情时间(会偏移)
    inittime: 802      # 开盘前准备时间(会偏移)
    name: TTS24测试
    proctime: 759      # 处理收盘作业时间(会偏移)
```

---

## WonderTrader-ubuntu安装

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder50/file03.html

**Contents:**
- WonderTrader-ubuntu安装

source: wtcpp/folder50/file03.md

sudo apt install build-essential -y

sudo apt install cmake -y

sudo apt-get install p7zip-full -y

从共享资源中将mydeps_ubt1804_1204.7z上传到虚拟机，解压，注意解压后的文件一定要放在/home目录下。因为编译的CMakeList里面将所用到的boost库文件目录写的是 /home/mydeps 。解压命令如下：sudo 7z x mydeps_ubt1804_1204.7z。结果如下:

sudo git clone https://github.com/wondertrader/wondertrader.git

进入src目录，执行编译脚本。这儿第一次build_debug.sh没有权限，需要手动进行chmod 777 build_debug.sh。

sudo ./build_debug.sh

---

## 状态机详解

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder02/file05.html

**Contents:**
- 状态机详解
- 准备工作
  - 配置文件
- 配置详解
  - dtcfg.yaml
  - mdparsers.yaml
  - statemonitor.yaml
- 代码注释
  - _state_mon.initialize
  - _state_mon.run

source: wtcpp/folder02/file05.md

执行 TestDtPorter 下的 main.cpp

在文章 "UDP数据转发" 中我们使用的是7*24小时环境, 没有执行状态机, 因此无法模拟收盘作业

顾名思义，在每个交易日结束以后，会对行情数据做一个盘后处理，这个处理的过程就叫做收盘作业。收盘作业主要包括以下工作：

由于状态机设了很多限制, 对节日或收盘时间检查非常严格(这在实盘当然是好事), 但是对测试而言十分不友好, 因此以下内容建议在开盘期间测试.

注意 proctime 这个时间点, 当前时间大于这个时间便开始收盘作业

main.cpp 主要通过 WtDtPorter.cpp 调用了 WtDtRunner.cpp 中的 WtDtRunner::initialize 和 WtDtRunner::start 方法.其他代码注释参考文章 "UDP数据转发", 使用状态机只有两句代码

主要操作就是加载 statemonitor.yaml 文件, 并和 session.json 文件中的内容一一对应起来, 并将所有信息添加到映射 _map 中

这段代码较长, 基本上就是通过时间判断当前所处的交易状态, 与收盘作业密切相关的只有以下两段代码. 不管是盘中还是收盘作业中, 牵涉到数据写入都与 WtDataWriter.cpp 有关

_dt_mgr->isSessionProceeded -> _writer->isSessionProceeded -> WtDataWriter::isSessionProceeded

_dt_mgr->transHisData -> _writer->transHisData -> WtDataWriter::transHisData

这里会启动一个线程, 执行 proc_loop, 进行真正的收盘作业

进入 WtDataWriter.cpp, 处理清理缓存内容, 还会更新 Storage/marker.ini, 说明收盘作业已完成

成功执行收盘作业后会出现 Storage/his 文件夹, 保存各类 dsb文件

**Examples:**

Example 1 (unknown):
```unknown
basefiles:
    utf-8: true     # 配置文件是否使用utf-8编码
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
broadcaster:
    active: true
    bport: 3997         # UDP转发端口
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
# allday: true        # 注释或删除, 否则不执行状态机
parsers: mdparsers.yaml
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 100          # 每次接收多少条数据
    path: ./Storage        # 数据保存文件夹
    savelog: true
```

Example 2 (unknown):
```unknown
basefiles:
    utf-8: true     # 配置文件是否使用utf-8编码
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
broadcaster:
    active: true
    bport: 3997         # UDP转发端口
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
# allday: true        # 注释或删除, 否则不执行状态机
parsers: mdparsers.yaml
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 100          # 每次接收多少条数据
    path: ./Storage        # 数据保存文件夹
    savelog: true
```

Example 3 (unknown):
```unknown
parsers:
-   active: true
    broker: ""
    id: tts24
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    code: CFFEX.IC2203,CFFEX.IF2203,CFFEX.IH2203 # 只接收指定合约数据
    # filter: SHFE,CZCE   # 只接收指定交易所数据
```

Example 4 (unknown):
```unknown
parsers:
-   active: true
    broker: ""
    id: tts24
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    code: CFFEX.IC2203,CFFEX.IF2203,CFFEX.IH2203 # 只接收指定合约数据
    # filter: SHFE,CZCE   # 只接收指定交易所数据
```

---

## UDP数据转发

**URL:** https://dumengru.github.io/docs_wondertrader/wtcpp/folder02/file04.html

**Contents:**
- UDP数据转发
- 准备工作
  - 1. 7*24小时环境
  - 2. 配置文件
- 配置详解
  - dtcfg.yaml
  - mdparsers.yaml
  - statemonitor.yaml
- 代码注释
  - WtDtRunner::initialize

source: wtcpp/folder02/file04.md

执行 TestDtPorter 下的 main.cpp

这一步非必要, 因为本人经常做测试, 又不喜欢用simnow, 同时要确保随时能获取数据做测试, 所以才使用了openctp.

如果使用7*24小时环境, 忽略即可, 如果使用一般环境, 用群里提供的配置文件即可

main.cpp 主要通过 WtDtPorter.cpp 调用了 WtDtRunner.cpp 中的 WtDtRunner::initialize 和 WtDtRunner::start 方法. 代码其实很简单, 所以只注释下

若出现如下 warning, 将原本保存的数据文件夹删掉即可

**Examples:**

Example 1 (unknown):
```unknown
basefiles:
    utf-8: true     # 配置文件是否使用utf-8编码
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
broadcaster:
    active: true
    bport: 3997         # UDP转发端口
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
allday: true        # 是否启动7*24环境
parsers: mdparsers.yaml
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 100          # 每次接收多少条数据
    path: ./FUT_Data        # 数据保存文件夹
    savelog: true
```

Example 2 (unknown):
```unknown
basefiles:
    utf-8: true     # 配置文件是否使用utf-8编码
    commodity: ./common/commodities.json
    contract: ./common/contracts.json
    holiday: ./common/holidays.json
    session: ./common/sessions.json
broadcaster:
    active: true
    bport: 3997         # UDP转发端口
    broadcast:
    -   host: 255.255.255.255
        port: 9001
        type: 2
    multicast_:
    -   host: 224.169.169.169
        port: 9002
        sendport: 8997
        type: 0
    -   host: 224.169.169.169
        port: 9003
        sendport: 8998
        type: 1
    -   host: 224.169.169.169
        port: 9004
        sendport: 8999
        type: 2
allday: true        # 是否启动7*24环境
parsers: mdparsers.yaml
statemonitor: statemonitor.yaml
writer:
    async: true
    groupsize: 100          # 每次接收多少条数据
    path: ./FUT_Data        # 数据保存文件夹
    savelog: true
```

Example 3 (unknown):
```unknown
parsers:
-   active: true
    broker: ""
    id: tts24
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    code: SHFE.au2206,SHFE.au2208 # 只接收指定合约数据
    # filter: SHFE,CZCE   # 只接收指定交易所数据
```

Example 4 (unknown):
```unknown
parsers:
-   active: true
    broker: ""
    id: tts24
    module: ParserCTP
    front: tcp://122.51.136.165:20004
    ctpmodule: tts_thostmduserapi_se
    pass: ******
    user: ******
    code: SHFE.au2206,SHFE.au2208 # 只接收指定合约数据
    # filter: SHFE,CZCE   # 只接收指定交易所数据
```

---
