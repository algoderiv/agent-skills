# Api Reference - CTP 6.7.8 API

本节包含 107 个主题。

## 目录

    - [常见FAQ](#常见FAQ) (第 172 页)
    - [CTP-GetSystemInfo](#CTP-GetSystemInfo) (第 178 页)
      - [GetTradingDay](#GetTradingDay) (第 216 页)
      - [Init](#Init) (第 221 页)
      - [Join](#Join) (第 227 页)
      - [RegisterFensUserInfo](#RegisterFensUserInfo) (第 232 页)
      - [RegisterFront](#RegisterFront) (第 238 页)
      - [RegisterNameServer](#RegisterNameServer) (第 244 页)
      - [RegisterSpi](#RegisterSpi) (第 250 页)
      - [Release](#Release) (第 255 页)
      - [ReqQryMulticastInstrument](#ReqQryMulticastInstrument) (第 260 页)
      - [ReqUserLogout](#ReqUserLogout) (第 274 页)
      - [OnFrontConnected](#OnFrontConnected) (第 309 页)
      - [OnFrontDisconnected](#OnFrontDisconnected) (第 314 页)
      - [OnHeartBeatWarning](#OnHeartBeatWarning) (第 319 页)
  - [交易接口](#交易接口) (第 378 页)
      - [OnErrRtnBankToFutureByFuture](#OnErrRtnBankToFutureByFuture) (第 427 页)
      - [OnErrRtnCombActionInsert](#OnErrRtnCombActionInsert) (第 441 页)
      - [OnErrRtnFutureToBankByFuture](#OnErrRtnFutureToBankByFuture) (第 466 页)
      - [OnErrRtnOptionSelfCloseAction](#OnErrRtnOptionSelfCloseAction) (第 474 页)
      - [OnErrRtnOptionSelfCloseInsert](#OnErrRtnOptionSelfCloseInsert) (第 481 页)
      - [OnErrRtnQueryBankBalanceByFuture](#OnErrRtnQueryBankBalanceByFuture) (第 501 页)
      - [OnErrRtnRepealBankToFutureByFutureManual](#OnErrRtnRepealBankToFutureByFutureManual) (第 523 页)
      - [OnErrRtnRepealFutureToBankByFutureManual](#OnErrRtnRepealFutureToBankByFutureManual) (第 531 页)
      - [OnFrontConnected](#OnFrontConnected) (第 539 页)
      - [OnFrontDisconnected](#OnFrontDisconnected) (第 544 页)
      - [OnHeartBeatWarning](#OnHeartBeatWarning) (第 549 页)
      - [GetTradingDay](#GetTradingDay) (第 1469 页)
      - [GetFrontInfo](#GetFrontInfo) (第 1475 页)
      - [Init](#Init) (第 1481 页)
      - [Join](#Join) (第 1487 页)
      - [RegisterFensUserInfo](#RegisterFensUserInfo) (第 1492 页)
      - [RegisterFront](#RegisterFront) (第 1498 页)
      - [RegisterNameServer](#RegisterNameServer) (第 1504 页)
      - [RegisterSpi](#RegisterSpi) (第 1510 页)
      - [RegisterUserSystemInfo](#RegisterUserSystemInfo) (第 1516 页)
      - [Release](#Release) (第 1525 页)
      - [ReqAuthenticate](#ReqAuthenticate) (第 1531 页)
      - [ReqFromBankToFutureByFuture](#ReqFromBankToFutureByFuture) (第 1578 页)
      - [ReqFromFutureToBankByFuture](#ReqFromFutureToBankByFuture) (第 1587 页)
      - [ReqGenUserCaptcha](#ReqGenUserCaptcha) (第 1596 页)
      - [ReqGenUserText](#ReqGenUserText) (第 1602 页)
      - [ReqOptionSelfCloseInsert](#ReqOptionSelfCloseInsert) (第 1617 页)
      - [ReqQryBrokerTradingAlgos](#ReqQryBrokerTradingAlgos) (第 1670 页)
      - [ReqQryBrokerTradingParams](#ReqQryBrokerTradingParams) (第 1676 页)
      - [ReqQryCombInstrumentGuard](#ReqQryCombInstrumentGuard) (第 1694 页)
      - [ReqQryContractBank](#ReqQryContractBank) (第 1700 页)
      - [ReqQryEWarrantOffset](#ReqQryEWarrantOffset) (第 1712 页)
      - [ReqQryExchange](#ReqQryExchange) (第 1718 页)
      - [ReqQryExchangeMarginRate](#ReqQryExchangeMarginRate) (第 1724 页)
      - [ReqQryExchangeMarginRateAdjust](#ReqQryExchangeMarginRateAdjust) (第 1730 页)
      - [ReqQryExchangeRate](#ReqQryExchangeRate) (第 1736 页)
      - [ReqQryInstrument](#ReqQryInstrument) (第 1754 页)
      - [ReqQryInstrumentCommissionRate](#ReqQryInstrumentCommissionRate) (第 1761 页)
      - [ReqQryInstrumentMarginRate](#ReqQryInstrumentMarginRate) (第 1767 页)
      - [ReqQryInvestUnit](#ReqQryInvestUnit) (第 1811 页)
      - [ReqQryMMInstrumentCommissionRate](#ReqQryMMInstrumentCommissionRate) (第 1817 页)
      - [ReqQryMMOptionInstrCommRate](#ReqQryMMOptionInstrCommRate) (第 1823 页)
      - [ReqQryNotice](#ReqQryNotice) (第 1829 页)
      - [ReqQryOptionInstrCommRate](#ReqQryOptionInstrCommRate) (第 1835 页)
      - [ReqQryOptionSelfClose](#ReqQryOptionSelfClose) (第 1848 页)
      - [ReqQryProduct](#ReqQryProduct) (第 1874 页)
      - [ReqQryProductExchRate](#ReqQryProductExchRate) (第 1880 页)
      - [ReqQryProductGroup](#ReqQryProductGroup) (第 1886 页)
      - [ReqQrySecAgentACIDMap](#ReqQrySecAgentACIDMap) (第 1899 页)
      - [ReqQrySecAgentCheckMode](#ReqQrySecAgentCheckMode) (第 1905 页)
      - [ReqQryTradingCode](#ReqQryTradingCode) (第 1948 页)
      - [ReqQryTradingNotice](#ReqQryTradingNotice) (第 1954 页)
      - [ReqQryTransferBank](#ReqQryTransferBank) (第 1960 页)
      - [ReqQryTransferSerial](#ReqQryTransferSerial) (第 1966 页)
      - [ReqUserAuthMethod](#ReqUserAuthMethod) (第 2035 页)
      - [ReqUserLogout](#ReqUserLogout) (第 2070 页)
      - [ReqUserPasswordUpdate](#ReqUserPasswordUpdate) (第 2076 页)
      - [SubmitUserSystemInfo](#SubmitUserSystemInfo) (第 2082 页)
      - [SubscribePrivateTopic](#SubscribePrivateTopic) (第 2089 页)
      - [SubscribePublicTopic](#SubscribePublicTopic) (第 2095 页)
      - [ReqQryClassifiedInstrument](#ReqQryClassifiedInstrument) (第 2101 页)
      - [ReqQryCombPromotionParam](#ReqQryCombPromotionParam) (第 2107 页)
      - [ReqQryRiskSettleProductStatus](#ReqQryRiskSettleProductStatus) (第 2119 页)
      - [ReqQrySPBMFutureParameter](#ReqQrySPBMFutureParameter) (第 2131 页)
      - [ReqQrySPBMOptionParameter](#ReqQrySPBMOptionParameter) (第 2137 页)
      - [ReqQrySPBMIntraParameter](#ReqQrySPBMIntraParameter) (第 2143 页)
      - [ReqQrySPBMInterParameter](#ReqQrySPBMInterParameter) (第 2149 页)
      - [ReqQrySPBMPortfDefinition](#ReqQrySPBMPortfDefinition) (第 2155 页)
      - [ReqQrySPMMInstParam](#ReqQrySPMMInstParam) (第 2191 页)
      - [ReqQrySPMMProductParam](#ReqQrySPMMProductParam) (第 2197 页)
      - [ReqQrySPBMAddOnInterParameter](#ReqQrySPBMAddOnInterParameter) (第 2203 页)
      - [ReqQryRCAMSCombProductInfo](#ReqQryRCAMSCombProductInfo) (第 2209 页)
      - [ReqQryRCAMSInstrParameter](#ReqQryRCAMSInstrParameter) (第 2215 页)
      - [ReqQryRCAMSIntraParameter](#ReqQryRCAMSIntraParameter) (第 2221 页)
      - [ReqQryRCAMSInterParameter](#ReqQryRCAMSInterParameter) (第 2227 页)
      - [ReqQryRCAMSShortOptAdjustParam](#ReqQryRCAMSShortOptAdjustParam) (第 2233 页)
      - [ReqQryRULEInstrParameter](#ReqQryRULEInstrParameter) (第 2251 页)
      - [ReqQryRULEIntraParameter](#ReqQryRULEIntraParameter) (第 2257 页)
      - [ReqQryRULEInterParameter](#ReqQryRULEInterParameter) (第 2263 页)
  - [其他业务规则](#其他业务规则) (第 2289 页)
    - [做市商询价和报价](#做市商询价和报价) (第 2291 页)
    - [报单回调规则](#报单回调规则) (第 2300 页)
    - [大商所组保](#大商所组保) (第 2311 页)
    - [报价回调规则](#报价回调规则) (第 2322 页)
    - [报单流控、查询流控和会话数控制](#报单流控、查询流控和会话数控制) (第 2361 页)
    - [通讯模式](#通讯模式) (第 2368 页)
    - [TAS介绍](#TAS介绍) (第 2374 页)
    - [条件单规则](#条件单规则) (第 2384 页)
    - [各交易所特殊指令](#各交易所特殊指令) (第 2395 页)
    - [期货期权的行权、自对冲](#期货期权的行权、自对冲) (第 2443 页)
    - [TGate网关接入指南](#TGate网关接入指南) (第 2460 页)

---

## 常见FAQ

常见FAQ
看穿式监管API相关FAQ
1. Q: 在看穿式监管上线之后，是不是必须把CSRC要求的信息
发送给CTP才可以下单正常交易？
A: 上线后，监管上要求自觉上报采集信息，但是你不上报，
也是能登录并交易的，CTP这边不做强制限制，但是该行为
不符合监管要求。
2. Q: 目前测试终端采集，同一个UserID，在多个客户端登录，
中继代理这边建立了多个实例，请问同一UserID允许建立多
少个实例？目前超过4个就会报：用户在线会话超出上限。但
是中继代理模式很有可能出现超过4个客户端同时登录的情
况。
A：这个是系统设置，跟看穿式监管无关，在柜台“用户最大
会话数”中设置。
3. Q：看穿式监管API断线重连是否需要重新发起认证和登录前
的信息上报？
A： 断线重连需要重新认证、信息上报、登录。
4. Q：看穿式监管API登录请求接口中是否可以不再传IP、端
口、登录备注等RegisterUserSystemInfo接口已经传过的入参
数据？
A:登录里的ip和mac无需填写，并且手填无效。


5. Q：一个帐号在多个终端登录时，通过中继代理必须每个终
端和CTP建立一个连接吗？
A: 中继多对多模式下，要为每个客户端建立一个会话；一对
多的模式，所有客户端共享一个操作员会话。
6. Q: 测试发现终端信息为空的话，调用
RegisterUserSystemInfo会返回-1，这种情况，CTP是收到了
空的终端信息，还是根本就不会生成任何记录？
A：收到了空的终端信息，监控中心不允许上报空信息。
7. Q:柜台的认证码，除了可以根据终端产品配置，能不能根据
资金账号配置?
A:不能
8. Q：请问如果期货公司已经升级到看穿式监管版本6.3.13T4，
那么我们软件提供商只是需要改前置地址就可以正常使用登
陆，下单等功能吗？（暂不考虑提供看穿式监管的相关信息
要求）？
A：使用新版本api需要接入到新的se前置，旧版本api的接入
在老前置还存在的情况下无需调整。
9. Q：测试看穿式监管的安卓版API，在获取硬件信息时较慢
A：目前安卓版采集函数会首先进行实时定位，2秒后定位不
到则使用上一次的位置信息。采集硬件信息的速度取决于安
卓版本定位的速度。
10. Q: 发现直连模式API demo中没有用
CTP_GetSystemInfo(pSystemInfo, len)，是否意味着直连模
式的话就不需要提交这个统计系统信息的？


A：直连的模式traderapi已经集成采集功能，在登录的时候
自动调用。CTP_GetSystemInfo函数是给中继模式中的客户
端使用的。
11. Q: 调用中继代理看穿式监管接口RegisterUserSystemInfo
时，一直提示“RegisterUserSystemInfo not permittedt”是什
么原因啊？
A: 如果提示operation not permitted，可能是AppID类型错
误。例如，直连模式的AppID，却错误调用了
SubmitUserSystemInfo。此错误不会通过特定接口返回，只
在标准输出中提示，例如直接在屏幕上打印出来。
12. Q：看穿式监管现在测试环境对应API库版本是6.3.13。后面
升级到6.3.15版本的生产包，API库有变更吗，我们的对接程
序需要改动嘛？
A: pc版的替换dll就行，不需要重新编译；安卓版替换sdk，
然后编译应用。
13. Q: 调用CTP_GetSystemInfo函数获取到的终端加密信息出来
都是乱码，用base64转码就是正常的字符串了，上传终端信
息传的是base64转码后的字符串吗？
A：不是，原始字符数组
14. Q: 6.3.13_T4就是测试环境用的？6.3.15是生产使用的？这2个
都是front_se，期货公司的柜台也必须要升级到对应的版本？
A: 6.3.11API为非看穿式监管版本，只能连接普通前置；由于
评测版本和生产版本使用的监控中心密钥不同，6.3.13评测版
API只能连接评测版看穿式监管前置，6.3.15生产版API只能
连接生产版看穿式监管前置。API、采集库和前置版本必须
一一对应，否则无法连接前置或无法解密采集数据！！


15. Q: 是否可以把测试环境的所有组成API，采集库，前置，都
升级成6.3.15，通过测试后，再在生产做相同版本的升级？
A: 6.3.15采集的信息经过生产密钥加密，只有监控中心能解
密，期货公司没法拿来做评测。
16. Q: 请问下看穿式监管信息采集评测工具（CDP网站上有），
只能采集到当天第一次登录采集的信息吗？
A: 不是，每次登录上报都会采集
17. Q: 中继代理补充获取终端软件“异常标示”，“appid”等信
息，都是由终端传到中继，而不是中继自行获取的吗？
A：
该问题要分开来看
“异常标示”包含在终端采集信息里，由采集函数自动采集，
非中继自行获取，中继只负责上报
ClientPublicIP：用户终端IP，由中继采集和填写
ClientLoginTime：用户登录中继时间，由中继采集和填写
ClientAppid：用户终端的appid，由中继采集和填写
18. Q：采集的大部分信息都在pSystemInfo这个字段里，该字符
串需要加密，这里加密方式是用CTP给的还是自己系统做加
密？
A: 采集到的数据经过监控中心密钥加密+CTP防伪加密 ，给
到ctp。ctp解开自己的防伪加密后给到监控中心（为监控中
心密钥加密后的结果）
19. Q: relayappID与直连的appID设置成一样的，这样可以分别
获得2套encryption key吗？
A: 不行的，两者名称要区分开


20. Q: 新的柜台里面userproductinfo还有用吗？
A: userproductinfo是连接非看穿式监管前置做认证用的，
AppID是连接看穿式监管前置做认证用的。看穿式监管上线
后，仍有部分期货公司需要根据这个字段进行其他统计用
途，具体需要视具体期货公司而定
21. Q: Linux 虚拟机 硬盘序列号、CPU序列号、BIOS序列号3项
指标采集不到怎么处理 ？
A: 信息采集过程中，遇到某些信息没采集到，但又不知道是
否正常，这个时候可以参考监控中心规范文档里的函数手工
调用，判断是否是权限问题或者相应操作系统命令组件没有
部署，采集库需要u+s权限。如果正常情况下手工调用能采集
到，而采集函数采集不到，请联系上期技术。
22. Q: 登陆时报“Decrypt handshake data failed”错误是什么原
因？
A：终端使用的api与前置版本不匹配导致，应核对版本。
23. Q: 如果我们终端自行获取信息（privateIP，terminalType，
MAC等），能否再调用.dll进行加密？
A:不行，必须调用getsysteminfo来采集，不能自行采集
24. Q: 流程是只有通过了测试环境的穿透式，才可以拿到生产的
AuthCode？
A: 必须通过评测
25. Q: 未来是否可以将生产和测试的版本统一起来，还是会保持
2个分支？
A: 目前没有计划统一


26. Q: 关于做市商测试上期所询价功能，目前有同时满足看穿式
监管和询价字段的API吗？哪个版本的报盘可以满足做市商
询价功能的测试？
A: 目前API都支持上期所询价的改动，T6版本报盘已经支持
该功能，报盘所在操作系统必须要6.6以上。
27. Q: OnRspError报错“CTP:API Front shake hand err :
version err”是什么意思
A：6.3.20以上版本，对api版本错误报错有改动，原先报错
Decrypt handshake data failed现在有核心通过OnRspError
返回报错。
28. Q: OnRspError报错“CTP:API Front shake hand err : decode
err”是什么意思
A：同27问。
回到顶部
< 前页 回目录 后页 >




---

## CTP-GetSystemInfo

CTP-GetSystemInfo
获取AES加密和RSA加密的终端信息。该函数来自采集终端信息
的动态链接库（安卓版的函数名和使用方法较windows和linux有所区
别，请注意阅读API包相关说明文档）。
仅中继模式下的客户端需要调用此函数来采集信息。中继需要将
客户端采集到的信息上报给CTP。
直连模式下，登录的时候自动采集并上报终端信息，所以无需调
用。
采集库在win、linux、android上不是线程安全的，不要并发调
用，ios是线程安全的
采集库暂时不采集IPv6地址


◇ 1.函数原型
int CTP_GetSystemInfo(char* pSystemInfo, int& nLen);
回到顶部


◇ 2.参数
pSystemInfo：空间需要调用者自己分配至少270个字节。
要注意这并不是一个字符串，而是数组，因为多次加密后可
能断串，使用memcpy拷贝值而不是strcpy。
nLen：获取到的采集信息的长度。
回到顶部


◇ 3.返回
0 为正确，非0为错误。
具体哪个采集项有问题需要做如下判断：
windows返回值定义:
    从低位开始分别标示 终端信息 ->系统盘分区信息
    返回值 & （0x01 << 0） 不为0 表示终端类型未采集到
    返回值 & （0x01 << 1） 不为0 表示 信息采集时间获取异常
    返回值 & （0x01 << 2） 不为0 表示ip 获取失败  （采集多个相同
类型信息的场景有一个采集到 即表示采集成功）
    返回值 & （0x01 << 3） 不为0 表示mac 获取失败
    返回值 & （0x01 << 4） 不为0 表示 设备名 获取失败
    返回值 & （0x01 << 5） 不为0 表示 操作系统版本 获取失败
    返回值 & （0x01 << 6） 不为0 表示 硬盘序列号 获取失败
    返回值 & （0x01 << 7） 不为0 表示 CPU序列号 获取失败
    返回值 & （0x01 << 8） 不为0 表示 BIOS 获取失败
    返回值 & （0x01 << 9） 不为0 表示 系统盘分区信息 获取失败
Linux返回值定义：
    从低位开始分别标示 终端信息 -> BIOS信息
    返回值 & （0x01 << 0） 不为0 表示终端类型未采集到
    返回值 & （0x01 << 1） 不为0 表示 信息采集时间获取异常
    返回值 & （0x01 << 2） 不为0 表示ip 获取失败  （采集多个相同
类型信息的场景有一个采集到 即表示采集成功）
    返回值 & （0x01 << 3） 不为0 表示mac 获取失败
    返回值 & （0x01 << 4） 不为0 表示 设备名 获取失败
    返回值 & （0x01 << 5） 不为0 表示 操作系统版本 获取失败
    返回值 & （0x01 << 6） 不为0 表示 硬盘序列号 获取失败
    返回值 & （0x01 << 7） 不为0 表示 CPU序列号 获取失败
    返回值 & （0x01 << 8） 不为0 表示 BIOS 获取失败
回到顶部


◇ 4.调用示例
char pSystemInfo[344];
int len;
CTP_GetSystemInfo(pSystemInfo, len);
回到顶部


◇ 5.FAQ
终端采集到的信息，保存到数据库里后显示为乱码，怎
么办？
可以使用BASE64转码成可见字符后再存储。
在windows下调用该函数后，控制台窗口打印出一些日
志，提示找不到wmic之类的信息，这个有关系吗？
有关系，采集函数需要调用相关windows组件来采集
设备信息，如果没有对应组件，则会采集不到相应信息，
不符合监管要求。
在linux下调用该函数后，控制台窗口会输出一些报错日
志，提示没有权限之类的错误，这个有关系吗？
有关系，采集函数需要调用linux命令来采集设备信
息，如果没有对应权限，则会采集不到相应信息，不符合
监管要求。
回到顶部
< 前页 回目录 后页 >


CTP-GetDataCollectApiVersion
获取采集库版本号


◇ 1.函数原型
const char * CTP_GetDataCollectApiVersion(void);
回到顶部


◇ 2.返回
采集库版本号，格式如下：
Sfit + 生产还是测试秘钥(pro/tst) + 秘钥版本 + 编译时间 + 版本
(内部)
回到顶部
< 前页 回目录 后页 >


行情接口
■ 6.7.8_API接口说明
└◆ 行情接口


◇ 1.说明
行情API提供了两个接口，分别为CThostFtdcMdApi和
CThostFtdcMdSpi。这两个接口对FTD协议进行了封装，方便客户端
应用程序的开发。客户端应用程序可以通过CThostFtdcMdApi发出
操作请求，通过继承CThostFtdcMdSpi并重载回调函数来处理后台服
务的响应。
特别注意：CTP系统在早盘系统启动时，会重演夜盘流水，
此时有可能重复推送整个夜盘的行情。如果用户此时连入CTP就
有可能收到这些重复行情，因此建议用户在处理行情时过滤掉重
复行情，以免影响程序逻辑。
自6.6.2P8柜台版本开始frontmd se行情前置带流重启后，不再
推送夜盘行情数据。
回到顶部


◇ 2.代码示例


◇ 2.1.源代码
下面通过一个简单的代码示例，带大家快速了解下行情API
的使用方法。该示例演示了API的初始化、连接前置、登录行情
系统和订阅行情的过程。
01. //以下是mduserhandle.h文件
02. 
03. #include "ThostFtdcMdApi.h"
04. 
05. #include <stdio.h>
06. 
07. #include <Windows.h>
08. 
09.  
10. 
11. class CMduserHandler : public CThostFtdcMdSpi
12. 
13. {
14. 
15. private:
16. 
17.   CThostFtdcMdApi *m_mdApi;
18. 
19.  
20. 
21. public:
22. 
23.   void connect()
24. 
25.   {
26. 


27. //创建并初始化API
28. 
29.       m_mdApi = 
CThostFtdcMdApi::CreateFtdcMdApi(".//flow_md/", true, true);
30. 
31.       m_mdApi->RegisterSpi(this);
32. 
33.       m_mdApi->RegisterFront("tcp://218.28.130.102:41413");
34. 
35.       m_mdApi->Init();
36. 
37.   }
38. 
39.  
40. 
41. //登陆
42. 
43.   void login()
44. 
45.   {
46. 
47.       CThostFtdcReqUserLoginField t = {0};
48. 
49.       while (m_mdApi->ReqUserLogin(&t, 1)!=0) Sleep(1000);
50. 
51.   }
52. 
53.  
54. 
55. // 订阅行情
56. 
57.   void subscribe()
58. 
59.   {


60. 
61.       char **ppInstrument=new char * [50];
62. 
63.       ppInstrument[0] = "IF1809";
64. 
65.       while (m_mdApi->SubscribeMarketData(ppInstrument, 
1)!=0) Sleep(1000);
66. 
67.   }
68. 
69.  
70. 
71.   //接收行情
72. 
73.   void 
OnRtnDepthMarketData(CThostFtdcDepthMarketDataField 
*pDepthMarketData)
74. 
75.   {
76. 
77.           printf("OnRtnDepthMarketData\n");
78. 
79.   }
80. 
81. };
82. 
83.  
84. 
85. // 以下是main.cpp文件
86. 
87. #include "mduserhandle.h"
88. 
89. int main(int argc, char* argv[])
90. 


91. {
92. 
93.   CMduserHandler *mduser = new CMduserHandler;
94. 
95.   mduser->connect();
96. 
97.   mduser->login();
98. 
99.   mduser->subscribe();
100.    
101.  Sleep(INFINITE);
102.    
103. }


◇ 2.2.代码说明
◇ 2.2.1.继承CThostFtdcMdSpi类
代码一开始通过#include "ThostFtdcMdApi.h"，将
thostmduserapi.lib中声明的类和数据类型包括进来，该头文件中
有两个重要的基类：CThostFtdcMdSpi和CThostFtdcMdApi。
CThostFtdcMdSpi类提供了行情相关的回调接口，用户需要
继承该类并重载这些接口，以获取响应数据。
CThostFtdcMdApi类则是提供了行情相关的请求接口，例
如订阅行情请求、订阅询价请求。
第11行我们声明了一个CMduserHandler类，该类正是继承
了CThostFtdcMdSpi类，并且重载了OnRtnDepthMarketData等接
口。通过这种方式，例如当我们API和前置建立连接成功时，
便会触发OnFrontConnected；当我们发起订阅行情请求成功
后，便会触发OnRspSubMarketData。通过这些回调返回的数
据，我们可以得知订阅是否成功，之后交易系统会通过
OnRtnDepthMarketData实时推送行情。
第17行，声明了一个CThostFtdcMdApi类型的变量
m_mdApi，后续我们会实例化它，创建实例后便可以调用
mdapi提供的登录，订阅行情等接口功能。
◇ 2.2.2.初始化
第23行到37行connect()函数里，实现了线程的初始化，步
骤为：


Step 1  创建Api实例（CThostFtdcMdApi）并为其注册对应的回
调接口类的实例（RegisterSpi）。
Step 2  注册名称服务器网络地址（RegisterNameServer）或注
册前置机网络地址（RegisterFront）。
Step 3  初始化Api（Init），使API建立与前置的连接，连接成
功后回调OnFrontConnected。
第29行创建了一个api实例，并将其赋值给m_mdApi，同时
参数".//flow_md/"指明我们api流水文件存放的目录为flow目
录，因此在运行程序前，我们需要手工创建好这个目录！
流水文件以.con文件类型方式存放流水序号和交易日信
息，这些信息对api正常工作有重要意义，不可随意修改和删
除！并且多个api实例不可共享一个流水文件，必须通过不同
文件夹或者不同流水文件名区分开。虽然行情没有私有流，
但是也建议遵循以上规则。
第31行将自己定义的继承了Spi类的CMduserHandler注册给
CThostFtdcMdApi，这样API就能将收到的各种数据通过Spi类
的接口回调给用户。
第33行注册了前置地址，如果有多个地址，可以多次调用
RegisterFront函数传入不同的地址来实现。API会择优挑选一个
地址进行连接。
第35行调用Init()函数开始正式初始化api，也就是说前面的
工作只是准备工作，到了这里api才真正开始工作。此时api会向
之前注册的地址发起与CTP前置的连接。
完成以上步骤后，客户端就已经和ctp的行情前置建立了连
接，后续我们就可以调用各种API接口完成业务需求。
◇ 2.2.3.登录


第43到51行，前置连接成功后能够开始登陆交易系统了，
先初始化登陆结构体，再赋值相应字段。对于行情的登录来说
只需要调用登录接口即可，目前CTP暂时不对行情做密码校
验。之后发送登陆（ReqUserLogin）指令。通过返回值可以判
断是否发送成功，0表示成功，其他则表示失败，具体可以参考
接口ReqUserLogin。发送不成功可以尝试等待一小会重发。
注意这里返回0不表示登录成功，而是仅仅表示api指令
发出去了。该规则同样适用于其他请求接口，建议在实际应
用中做好超时重发机制，以便在网络丢包的情况下能够及时
重发指令。
◇ 2.2.4.订阅行情
第57到67行，调用订阅行情的接口，我们这里订阅了合
约"IF1809"。若需要批量订阅多个合约，则需要循环把合约输
入到ppInstrumentID中去，同时别忘了更改合约数量（第二个参
数）。订阅发出后通过OnRspSubMarketData响应判断是否订阅
成功。
第73到81行，订阅行情后，通过OnRtnDepthMarketData回
调推送实时行情信息。可以在此时实现自身业务逻辑。
但是如果业务逻辑比较耗时，应该在另外一个线程处
理，而不应该卡在此回调里，否则会导致后续的行情堵塞在
API内部，严重情况下会导致断线。
◇ 2.2.5.程序运行流程


第89到103行，是我们的主函数，该函数是业务实现主体。
首先初始化CMduserHandler类。调用connect函数开始连接ctp前
置，然后依次执行登录和订阅行情操作。
请参阅：
 >> CThostFtdcMdApi
 >> CThostFtdcMdSpi
回到顶部
< 前页 回目录 后页 >


CThostFtdcMdApi
■ 6.7.8_API接口说明
└△ 行情接口
　└◆ CThostFtdcMdApi
CthostFtdcMdApi类提供了行情api的初始化、登录、订阅等功能。


◇ 1.接口
class MD_API_EXPORT CThostFtdcMdApi
{
public:
    ///创建MdApi
    ///@param pszFlowPath 存贮订阅信息文件的目录，默认为当前目
录
    ///@return 创建出的UserApi
    ///modify for udp marketdata
    static CThostFtdcMdApi *CreateFtdcMdApi(const char 
*pszFlowPath = "", const bool bIsUsingUdp=false, const bool 
bIsMulticast=false);
    ///获取API的版本信息
    ///@retrun 获取到的版本号
    static const char *GetApiVersion();
    ///删除接口对象本身
    ///@remark 不再使用本接口对象时,调用该函数删除接口对象
    virtual void Release() = 0;
    ///初始化
    ///@remark 初始化运行环境,只有调用后,接口才开始工作
    virtual void Init() = 0;
    ///等待接口线程结束运行
    ///@return 线程退出代码
    virtual int Join() = 0;
    ///获取当前交易日
    ///@retrun 获取到的交易日
    ///@remark 只有登录成功后,才能得到正确的交易日
    virtual const char *GetTradingDay() = 0;
    ///注册前置机网络地址
    ///@param pszFrontAddress：前置机网络地址。
    ///@remark 网络地址的格式为：“protocol://ipaddress:port”，
如：”tcp://127.0.0.1:17001”。 


    ///@remark “tcp”代表传输协议，“127.0.0.1”代表服务器地
址。”17001”代表服务器端口号。
    virtual void RegisterFront(char *pszFrontAddress) = 0;
    ///注册名字服务器网络地址
    ///@param pszNsAddress：名字服务器网络地址。
    ///@remark 网络地址的格式为：“protocol://ipaddress:port”，
如：”tcp://127.0.0.1:12001”。 
    ///@remark “tcp”代表传输协议，“127.0.0.1”代表服务器地
址。”12001”代表服务器端口号。
    ///@remark RegisterNameServer优先于RegisterFront
    virtual void RegisterNameServer(char *pszNsAddress) = 0;
    ///注册名字服务器用户信息
    ///@param pFensUserInfo：用户信息。
    virtual void RegisterFensUserInfo(CThostFtdcFensUserInfoField * 
pFensUserInfo) = 0;
    ///注册回调接口
    ///@param pSpi 派生自回调接口类的实例
    virtual void RegisterSpi(CThostFtdcMdSpi *pSpi) = 0;
    ///订阅行情。
    ///@param ppInstrumentID 合约ID  
    ///@param nCount 要订阅/退订行情的合约个数
    ///@remark 
    virtual int SubscribeMarketData(char *ppInstrumentID[], int nCount) 
= 0;
    ///退订行情。
    ///@param ppInstrumentID 合约ID  
    ///@param nCount 要订阅/退订行情的合约个数
    ///@remark 
    virtual int UnSubscr

---

## GetTradingDay

GetTradingDay
获得当前交易日。只有当登陆成功后才会取到正确的值。


◇ 1.函数原型
virtual const char *GetTradingDay() = 0;
回到顶部


◇ 2.返回
返回一个指向日期信息字符串的常量指针。
回到顶部


◇ 3.调用示例
CThostFtdcMdApi  *pUserMdApi = 
CThostFtdcMdApi::CreateFtdcMdApi();
CSimpleMdHandler ash(pUserMdApi);
pUserMdApi->RegisterSpi(&ash);
pUserMdApi->RegisterFront(“tcp://127.0.0.1:41213”);
pUserMdApi->Init();
//登录成功后
printf("获取当前交易日期:%s\n", pUserMdApi->GetTradingDay());
回到顶部


◇ 4.FAQ
登录前为什么也可以获取到交易日？
登录前调用该函数会去上一次的API流水文件里去获
