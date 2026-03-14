# 查询视频生成任务列表

`GET https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks`

传入条件筛选参数，返回符合要求的任务列表。仅支持查询最近 7 天的历史数据。

本接口支持 API Key 鉴权。

## 请求参数 (Query String Parameters)

### page_num (integer, 可选)
返回结果的页码。取值范围：[1, 500]。

### page_size (integer, 可选)
每页结果数量。取值范围：[1, 500]。

### filter.status (string, 可选)
按任务状态过滤：`queued` / `running` / `cancelled` / `succeeded` / `failed`

### filter.task_ids (string[], 可选)
按视频生成任务 ID 精确搜索，支持多个。多个 ID 通过 `&` 连接：
```
filter.task_ids=id1&filter.task_ids=id2
```

### filter.model (string, 可选)
按推理接入点 ID 精确搜索。

### filter.service_tier (string, 可选, 默认 "default")
按服务等级过滤：
- `default`: 在线推理模式
- `flex`: 离线推理模式

## 响应参数

### total (integer)
符合筛选条件的任务总数。

### items (object[])
任务列表，每个任务对象包含与查询单个任务相同的字段：

- `items.id` (string): 任务 ID
- `items.model` (string): 模型名称和版本
- `items.status` (string): 任务状态
- `items.error` (object | null): 错误信息
- `items.created_at` (integer): 创建时间 Unix 时间戳
- `items.updated_at` (integer): 状态更新时间 Unix 时间戳
- `items.content` (object): 输出内容
  - `content.video_url` (string): 视频 URL（24 小时后清理）
  - `content.last_frame_url` (string): 尾帧图像 URL（24 小时有效）
- `items.seed` (integer): 种子值
- `items.resolution` (string): 分辨率
- `items.ratio` (string): 宽高比
- `items.duration` (integer): 时长（秒）
- `items.frames` (integer): 帧数
- `items.framespersecond` (integer): 帧率
- `items.generate_audio` (boolean): 是否包含音频（仅 Seedance 1.5 pro）
- `items.draft` (boolean): 是否为 Draft 视频（仅 Seedance 1.5 pro）
- `items.draft_task_id` (string): Draft 视频任务 ID
- `items.service_tier` (string): 服务等级
- `items.execution_expires_after` (integer): 超时阈值（秒）
- `items.usage` (object): Token 用量

## 请求示例

### 按状态筛选

```bash
curl -X GET "https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks?page_size=3&filter.status=succeeded" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY"
```

### 按多个任务 ID 搜索

```bash
curl -X GET "https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks?filter.task_ids=cgt-2025xxxx1&filter.task_ids=cgt-2025xxxx2" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY"
```

## 响应示例

```json
{
  "total": 3,
  "items": [
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
  ]
}
```
