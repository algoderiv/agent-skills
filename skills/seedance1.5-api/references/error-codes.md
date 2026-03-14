# 错误码参考

调用火山方舟 API 可能涉及的错误码，包含推理错误码和精调错误码。

## 推理错误码

### HTTP 400 BadRequest

| 错误码 | 说明 |
|--------|------|
| `MissingParameter` | 请求缺少必要参数 |
| `InvalidParameter` | 请求包含非法参数 |
| `InvalidEndpoint.ClosedEndpoint` | 推理接入点已关闭或暂时不可用 |
| `SensitiveContentDetected` | 输入文本可能包含敏感信息 |
| `SensitiveContentDetected.SevereViolation` | 输入文本可能包含严重违规信息 |
| `SensitiveContentDetected.Violence` | 输入文本可能包含激进行为信息 |
| `InputTextSensitiveContentDetected` | 输入文本可能包含敏感信息 |
| `InputImageSensitiveContentDetected` | 输入图像可能包含敏感信息 |
| `InputVideoSensitiveContentDetected` | 输入视频可能包含敏感信息 |
| `InputAudioSensitiveContentDetected` | 输入音频可能包含敏感信息 |
| `OutputTextSensitiveContentDetected` | 生成的文字可能包含敏感信息 |
| `OutputImageSensitiveContentDetected` | 生成的图像可能包含敏感信息 |
| `OutputVideoSensitiveContentDetected` | 生成的视频可能包含敏感信息 |
| `OutputAudioSensitiveContentDetected` | 生成的音频可能包含敏感信息 |
| `InputTextSensitiveContentDetected.PolicyViolation` | 输入文本可能违反平台规定 |
| `InputImageSensitiveContentDetected.PolicyViolation` | 输入图片可能违反平台规定 |
| `InputVideoSensitiveContentDetected.PolicyViolation` | 输入视频可能违反平台规定 |
| `InputAudioSensitiveContentDetected.PolicyViolation` | 输入音频可能违反平台规定 |
| `InputImageSensitiveContentDetected.PrivacyInformation` | 输入图片可能包含真人 |
| `InputVideoSensitiveContentDetected.PrivacyInformation` | 输入视频可能包含真人 |
| `InputTextRiskDetection` | 风险识别检测到输入文本敏感 |
| `InputImageRiskDetection` | 风险识别检测到输入图片敏感 |
| `OutputTextRiskDetection` | 风险识别检测到输出文本敏感 |
| `OutputImageRiskDetection` | 风险识别检测到输出图片敏感 |
| `ContentSecurityDetectionError` | 风险识别产品请求失败 |
| `InvalidParameter.{{Parameter}}` | 请求参数值不合法 |
| `MissingParameter.{{Parameter}}` | 缺少必要的请求参数 |
| `InvalidArgumentError` | messages 列表中消息体缺少 role 字段 |
| `InvalidArgumentError.UnknownRole` | role 值不被支持 |
| `InvalidArgumentError.InvalidImageDetail` | image_url 中 detail 参数值无效（只接受 auto/high/low） |
| `InvalidImageURL.EmptyURL` | 传入的图片 URL 为空 |
| `InvalidImageURL.InvalidFormat` | 无法解析或处理图片 |
| `OutofContextError` | 文本和图片总 token 数超过模型上下文长度限制 |

### HTTP 400 Forbidden

| 错误码 | 说明 |
|--------|------|
| `InvalidSubscription` | Coding Plan 套餐未订阅或已过期 |

### HTTP 401 Unauthorized

| 错误码 | 说明 |
|--------|------|
| `AuthenticationError` | API Key 或 AK/SK 校验未通过 |

### HTTP 401 Forbidden

| 错误码 | 说明 |
|--------|------|
| `InvalidAccountStatus` | 当前使用的账号异常 |

### HTTP 403 Forbidden

| 错误码 | 说明 |
|--------|------|
| `OperationDenied.InvalidState` | 请求关联的资源处于非空闲状态 |
| `OperationDenied.PermissionDenied` | 没有权限访问基础模型配置 |
| `OperationDenied.ServiceNotOpen` | 模型服务不可用 |
| `OperationDenied.ServiceOverdue` | 账单已逾期 |
| `AccountOverdueError` | 当前账号欠费（余额 < 0） |
| `AccessDenied` | 没有访问该资源的权限 |
| `OperationDenied.FileQuotaExceeded` | 文件存储额度耗尽 |

### HTTP 404 NotFound

| 错误码 | 说明 |
|--------|------|
| `InvalidEndpointOrModel.NotFound` | 模型或推理接入点不存在或无权访问 |
| `ModelNotOpen` | 账号暂未开通该模型服务 |
| `NotFound.{{Parameter}}` | 指定资源找不到 |
| `InvalidEndpointOrModel.ModelIDAccessDisabled` | 不允许使用模型 ID 来调用模型 |
| `UnsupportedModel` | 当前模型不支持 Coding Plan |

### HTTP 429 TooManyRequests

| 错误码 | 说明 |
|--------|------|
| `RateLimitExceeded.EndpointRPMExceeded` | 推理接入点已超过 RPM 限制 |
| `RateLimitExceeded.EndpointTPMExceeded` | 推理接入点已超过 TPM 限制 |
| `ModelAccountRpmRateLimitExceeded` | 超过帐户模型 RPM 限制 |
| `ModelAccountTpmRateLimitExceeded` | 超过帐户模型 TPM 限制 |
| `APIAccountRpmRateLimitExceeded` | 当前账号该接口的 RPM 限制已超出 |
| `ModelAccountIpmRateLimitExceeded` | 超过账户模型 IPM 限制 |
| `QuotaExceeded` | 免费额度用完 / 排队任务数超限 / 超出时间段额度 |
| `ServerOverloaded` | 服务资源紧张（流量突增或长时间未使用） |
| `RequestBurstTooFast` | 请求量激增触发系统保护 |
| `SetLimitExceeded` | 达到设置的推理限额值 |
| `InflightBatchsizeExceeded` | 达到最大并发数限制 |
| `AccountRateLimitExceeded` | 超出 RPM/TPM 限制 |

### HTTP 500 InternalServerError

| 错误码 | 说明 |
|--------|------|
| `InternalServiceError` | 内部系统异常 |

## 视频生成常见错误处理

### 内容安全相关
当遇到 `SensitiveContentDetected` 或 `PolicyViolation` 类错误时：
- 检查输入文本是否包含敏感内容
- 检查输入图片/视频是否包含真人面部（`PrivacyInformation`）
- 调整提示词避免敏感内容

### 限流相关
当遇到 `429 TooManyRequests` 时：
- 使用 `service_tier: "flex"` 离线模式获取更高 TPD 配额
- 降低请求频率，实现请求排队
- 检查账户配额是否充足

### 参数相关
当遇到 `InvalidParameter` 时：
- 检查 `resolution`、`ratio`、`duration` 等参数取值是否在允许范围内
- 确认所用模型是否支持该参数（如 `generate_audio` 仅 Seedance 1.5 pro 支持）
- 检查图片格式、尺寸是否符合要求
