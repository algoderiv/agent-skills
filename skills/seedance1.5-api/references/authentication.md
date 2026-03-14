# Base URL、鉴权与 API Key 配置

## Base URL

| 类型 | Base URL |
|------|----------|
| 数据面 API（模型调用） | `https://ark.cn-beijing.volces.com/api/v3` |
| 管控面 API（资源管理） | `https://ark.cn-beijing.volcengineapi.com/` |

- **数据面 API**：直接面向业务数据传输、实时交互、用户请求处理的接口。视频生成 API 属于数据面 API。
- **管控面 API**：用于系统资源管理、配置控制和状态监控的接口。

## 鉴权方式

### 方式一：API Key 签名鉴权（推荐，简单方便）

在 HTTP 请求 header 中添加 Authorization：

```
Authorization: Bearer $ARK_API_KEY
```

**curl 示例：**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{...}'
```

### 方式二：Access Key 签名鉴权（企业精细化管理）

面向企业使用传统云上资源权限管控。需获取 Access Key（参见 API 访问密钥管理）。建议创建 IAM 用户并授予方舟权限。

通过 Access Key 鉴权时，`model` 字段需配置为 Endpoint ID。

**签名字段信息：**
- Service: `ark`
- Region: `cn-beijing`
- 签名算法: HMAC-SHA256

```bash
curl -X POST \
  'https://ark.cn-beijing.volcengineapi.com/?Action=ListEndpoints&Version=2024-01-01' \
  -H 'Authorization: HMAC-SHA256 Credential=AKL**/20240710/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=a7a****' \
  -H 'Content-Type: application/json' \
  -H 'Host: ark.cn-beijing.volcengineapi.com' \
  -H 'X-Content-Sha256: 44***' \
  -H 'X-Date: 20240710T042925Z' \
  -d '{}'
```

## 获取 API Key

1. 打开并登录 [API Key 管理](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey) 页面
2. （可选）切换项目空间
3. 单击「创建 API Key」按钮
4. 确认/更改 API Key 名称，单击创建
5. 在 API Key 列表中查看刚创建的 API Key

## 配置 API Key

**推荐将 API Key 配置在环境变量中**，避免硬编码泄露：

```bash
# Linux/Mac
export ARK_API_KEY="your-api-key-here"

# Windows
set ARK_API_KEY=your-api-key-here
```

## API Key 使用说明

- **配额**: 一个主账号下支持创建 50 个 API Key，如需更多请提交工单
- **权限控制**: API Key 创建于当前项目，用于访问当前项目下的资源
  - 可额外限制可鉴权的 Model ID / 自定义推理接入点
  - 可限制可调用该 API Key 的 IP
  - 可通过切换项目空间创建 API Key 实现权限隔离
- **注意**: API Key 仅支持访问指定项目下的接入点，不支持跨项目访问
