---
name: seedance1.5-api
description: Seedance 1.5/1.0 系列视频生成 API 文档（火山方舟平台）。用于通过 API 调用 Seedance 模型实现文生视频、图生视频（首帧/首尾帧/参考图）、有声视频生成等功能，包含完整的请求参数、SDK 示例、提示词技巧和错误码参考。
---

# Seedance 1.5 API Skill

Seedance 是字节跳动豆包大模型团队推出的视频生成基础模型系列，通过火山方舟（Volcano Ark）平台提供 API 服务。本技能涵盖 Seedance 1.5 pro、1.0 pro、1.0 pro fast、1.0 lite 全系列模型的视频生成 API 使用方法。

> **注意**: Seedance 2.0 目前仅支持控制台体验中心体验，暂不支持 API 调用。

## When to Use This Skill

This skill should be triggered when:
- 调用 Seedance API 生成视频（文生视频、图生视频）
- 使用火山方舟平台的视频生成 API
- 需要了解 Seedance 模型的参数配置（分辨率、宽高比、时长等）
- 编写 Seedance 视频生成的提示词（prompt）
- 使用 Python/Go/Java SDK 调用 Seedance
- 处理视频生成任务的创建、查询、取消
- 调试 Seedance API 的错误码

## Quick Reference

### API Base URL

```
https://ark.cn-beijing.volces.com/api/v3
```

### 鉴权方式

```bash
Authorization: Bearer $ARK_API_KEY
```

### 模型 ID 列表

| 模型 | Model ID | 能力 |
|------|----------|------|
| Seedance 1.5 pro | `doubao-seedance-1-5-pro-251215` | 文生视频、首帧图生视频、首尾帧图生视频、有声视频、样片模式 |
| Seedance 1.0 pro | `doubao-seedance-1-0-pro-250528` | 文生视频、首帧图生视频、首尾帧图生视频 |
| Seedance 1.0 pro fast | `doubao-seedance-1-0-pro-fast-250528` | 文生视频、首帧图生视频 |
| Seedance 1.0 lite t2v | `doubao-seedance-1-0-lite-t2v` | 文生视频 |
| Seedance 1.0 lite i2v | `doubao-seedance-1-0-lite-i2v` | 首帧/首尾帧/参考图(1-4张)图生视频 |

### 核心 API 端点

| 操作 | 方法 | 端点 |
|------|------|------|
| 创建视频生成任务 | `POST` | `/contents/generations/tasks` |
| 查询视频生成任务 | `GET` | `/contents/generations/tasks/{id}` |
| 查询任务列表 | `GET` | `/contents/generations/tasks?page_num=&page_size=&filter.status=` |
| 取消/删除任务 | `DELETE` | `/contents/generations/tasks/{id}` |

### 快速开始 - 文生视频（curl）

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [
        {
            "type": "text",
            "text": "写实风格，晴朗的蓝天之下，一大片白色的雏菊花田，镜头逐渐拉近，最终定格在一朵雏菊花的特写上，花瓣上有几颗晶莹的露珠"
        }
    ],
    "ratio": "16:9",
    "duration": 5,
    "resolution": "720p",
    "watermark": false
}'
```

### 快速开始 - 图生视频（首帧）

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [
        {"type": "text", "text": "老人缓缓戴上眼镜，微笑"},
        {
            "type": "image_url",
            "image_url": {"url": "https://example.com/image.jpg"},
            "role": "first_frame"
        }
    ],
    "ratio": "16:9",
    "duration": 5
}'
```

### 快速开始 - 有声视频（Seedance 1.5 pro）

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [
        {
            "type": "text",
            "text": "男人叫住女人说：\"你记住，以后不可以用手指指月亮。\""
        }
    ],
    "generate_audio": true,
    "ratio": "16:9",
    "duration": 5
}'
```

### 查询任务状态

```bash
curl -X GET https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks/cgt-2025xxxx \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY"
```

### Python SDK 示例

```python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    timeout=1800,
)

# 创建文生视频任务
task = client.content.generations.tasks.create(
    model="doubao-seedance-1-5-pro-251215",
    content=[{"type": "text", "text": "一只小猫对着镜头打哈欠"}],
    ratio="16:9",
    duration=5,
)
print(f"Task ID: {task.id}")

# 查询任务状态
result = client.content.generations.tasks.get(task.id)
print(f"Status: {result.status}")
if result.status == "succeeded":
    print(f"Video URL: {result.content.video_url}")
```

### 视频参数速查

| 参数 | 取值 | 默认值 | 说明 |
|------|------|--------|------|
| `resolution` | `480p` / `720p` / `1080p` | 1.5pro/lite: `720p`, 1.0pro: `1080p` | 分辨率 |
| `ratio` | `16:9` / `4:3` / `1:1` / `3:4` / `9:16` / `21:9` / `adaptive` | 文生: `16:9`, 图生: `adaptive` | 宽高比 |
| `duration` | 2~12 秒（1.5pro 支持 -1 自动） | `5` | 视频时长 |
| `seed` | -1 ~ 2^32-1 | `-1` (随机) | 随机种子 |
| `camera_fixed` | `true` / `false` | `false` | 固定摄像头 |
| `watermark` | `true` / `false` | `false` | 水印 |
| `generate_audio` | `true` / `false` | `true` | 有声视频(仅1.5pro) |
| `draft` | `true` / `false` | `false` | 样片模式(仅1.5pro) |
| `service_tier` | `default` / `flex` | `default` | flex为离线模式,半价 |

## References

- `api-create-task.md` - 创建视频生成任务 API 完整参数说明
- `api-query-task.md` - 查询视频生成任务 API
- `api-query-task-list.md` - 查询视频生成任务列表 API
- `api-cancel-delete-task.md` - 取消或删除视频生成任务 API
- `authentication.md` - Base URL、鉴权方式和 API Key 配置
- `sdk-examples.md` - Python/Go/Java SDK 使用示例
- `openai-sdk-compat.md` - 兼容 OpenAI SDK 调用方式
- `prompt-guide.md` - Seedance 提示词使用技巧
- `error-codes.md` - 错误码参考
