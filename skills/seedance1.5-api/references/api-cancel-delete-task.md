# 取消或删除视频生成任务

`DELETE https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks/{id}`

取消排队中的视频生成任务，或删除视频生成任务记录。

本接口支持 API Key 鉴权。

## 请求参数

### id (string, 必选)
需要取消或删除的视频生成任务 ID。在 URL 路径中传入。

## 任务状态与 DELETE 操作对照

| 当前状态 | 是否支持 DELETE | 操作结果 |
|---------|----------------|----------|
| `queued` | ✅ 支持 | 取消排队，状态变为 `cancelled` |
| `running` | ❌ 不支持 | - |
| `succeeded` | ✅ 支持 | 删除任务记录，后续不支持查询 |
| `failed` | ✅ 支持 | 删除任务记录，后续不支持查询 |
| `cancelled` | ❌ 不支持 | - |
| `expired` | ✅ 支持 | 删除任务记录，后续不支持查询 |

## 响应参数

本接口无返回参数，返回空对象 `{}`。

## 请求示例

```bash
curl -X DELETE https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks/cgt-2025xxxx \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY"
```

## 响应示例

```json
{}
```
