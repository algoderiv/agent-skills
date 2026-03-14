# 查询视频生成任务 API

`GET https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks/{id}`

查询视频生成任务的状态。仅支持查询最近 7 天的历史数据。时间计算统一采用 UTC 时间戳。

本接口支持 API Key 鉴权。

## 请求参数

### id (string, 必选)

需要查询的视频生成任务 ID。在 URL 路径中传入。

## 响应参数

### id (string)
视频生成任务 ID。

### model (string)
任务使用的模型名称和版本。

### status (string)
任务状态：

| 状态 | 说明 |
|------|------|
| `queued` | 排队中 |
| `running` | 任务运行中 |
| `cancelled` | 任务已取消（24h 自动删除，只支持排队中状态的任务被取消） |
| `succeeded` | 任务成功 |
| `failed` | 任务失败 |
| `expired` | 任务超时 |

### error (object | null)
错误提示信息。
- `error.code` (string): 错误码
- `error.message` (string): 错误提示信息

### created_at (integer)
任务创建时间的 Unix 时间戳（秒）。

### updated_at (integer)
任务当前状态更新时间的 Unix 时间戳（秒）。

### content (object)
视频生成任务的输出内容。
- `content.video_url` (string): 生成视频的 URL，格式为 mp4。**24 小时后被清理，请及时转存。**
- `content.last_frame_url` (string): 视频尾帧图像 URL（需在创建时设置 `return_last_frame: true`）。24 小时后过期。

### seed (integer)
本次请求使用的种子整数值。

### resolution (string)
生成视频的分辨率。

### ratio (string)
生成视频的宽高比。

### duration (integer)
生成视频的时长（秒）。`duration` 和 `frames` 只返回一个。

### frames (integer)
生成视频的帧数。`duration` 和 `frames` 只返回一个。

### framespersecond (integer)
生成视频的帧率（通常为 24）。

### generate_audio (boolean) - 仅 Seedance 1.5 pro
是否包含音频。

### draft (boolean) - 仅 Seedance 1.5 pro
是否为 Draft 视频。

### draft_task_id (string) - 仅 Seedance 1.5 pro
Draft 视频任务 ID。

### service_tier (string)
实际处理任务使用的服务等级。

### execution_expires_after (integer)
任务超时阈值（秒）。

### usage (object)
Token 用量。
- `usage.completion_tokens` (integer): 模型输出视频花费的 token 数量
- `usage.total_tokens` (integer): 总 token 数量（输入 token 为 0，total = completion）

## 请求示例

```bash
curl -X GET https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks/cgt-2025xxxx \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY"
```

## 响应示例

```json
{
  "id": "cgt-2025******-****",
  "model": "doubao-seedance-1-5-pro-251215",
  "status": "succeeded",
  "content": {
    "video_url": "https://ark-content-generation-cn-beijing.tos-cn-beijing.volces.com/xxx"
  },
  "usage": {
    "completion_tokens": 108900,
    "total_tokens": 108900
  },
  "created_at": 1743414619,
  "updated_at": 1743414673,
  "seed": 10,
  "resolution": "720p",
  "ratio": "16:9",
  "duration": 5,
  "framespersecond": 24,
  "service_tier": "default",
  "execution_expires_after": 172800,
  "generate_audio": true,
  "draft": false
}
```

## 轮询模式

创建任务后需轮询查询状态，推荐间隔 5-10 秒：

```python
import time
import os
from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

# 创建任务
task = client.content.generations.tasks.create(
    model="doubao-seedance-1-5-pro-251215",
    content=[{"type": "text", "text": "一只猫在花园里玩耍"}],
    duration=5,
)

# 轮询查询
while True:
    result = client.content.generations.tasks.get(task.id)
    if result.status in ("succeeded", "failed", "expired", "cancelled"):
        break
    print(f"Status: {result.status}, waiting...")
    time.sleep(10)

if result.status == "succeeded":
    print(f"Video URL: {result.content.video_url}")
    # 注意：video_url 24小时后过期，请及时下载转存
else:
    print(f"Task {result.status}: {result.error}")
```
