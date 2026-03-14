# 创建视频生成任务 API

`POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks`

模型会依据传入的图片及文本信息生成视频，待生成完成后，可按条件查询任务并获取生成的视频。本接口仅支持 API Key 鉴权。

## 模型能力对照

| 模型 | Model ID | 文生视频 | 首帧图生视频 | 首尾帧图生视频 | 参考图生视频 | 有声视频 | 样片模式 |
|------|----------|----------|-------------|---------------|-------------|----------|----------|
| Seedance 1.5 pro | `doubao-seedance-1-5-pro-251215` | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |
| Seedance 1.0 pro | `doubao-seedance-1-0-pro-250528` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Seedance 1.0 pro fast | `doubao-seedance-1-0-pro-fast-250528` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Seedance 1.0 lite t2v | `doubao-seedance-1-0-lite-t2v` | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Seedance 1.0 lite i2v | `doubao-seedance-1-0-lite-i2v` | ❌ | ✅ | ✅ | ✅(1-4张) | ❌ | ❌ |

## 请求参数

### model (string, 必选)

模型 ID（Model ID）。也可通过 Endpoint ID 来调用模型，获得限流、计费类型、运行状态查询、监控、安全等高级能力。

### content (object[], 必选)

输入给模型生成视频的信息，支持文本、图片和视频（样片 Draft）格式。支持以下组合：
- 文本（文生视频）
- 文本 + 图片（图生视频）
- 视频/样片（基于 Draft 生成正式视频）

#### 文本信息 (text)

```json
{
    "type": "text",
    "text": "描述期望生成的视频内容"
}
```

- `content.type`: 固定为 `text`
- `content.text`: 文本提示词，支持中英文。建议中文不超过 500 字，英文不超过 1000 词。字数过多信息容易分散，模型可能忽略细节。

#### 图片信息 (image_url)

```json
{
    "type": "image_url",
    "image_url": {"url": "图片URL或Base64编码"},
    "role": "first_frame"
}
```

- `content.type`: 固定为 `image_url`
- `content.image_url.url`: 图片 URL 或 Base64 编码
  - URL：确保可被访问
  - Base64：格式 `data:image/<图片格式>;base64,<Base64编码>`，图片格式需小写
- `content.role` (条件必填): 图片位置/用途

**图片要求：**
- 格式：jpeg、png、webp、bmp、tiff、gif（Seedance 1.5 pro 新增 heic、heif）
- 宽高比（宽/高）：(0.4, 2.5)
- 宽高长度：(300, 6000) px
- 大小：< 30 MB

**图生视频场景：**

| 场景 | 支持模型 | role 取值 | 说明 |
|------|---------|-----------|------|
| 首帧图生视频 | 所有图生视频模型 | `first_frame`（可不填） | 传入 1 个 image_url 对象 |
| 首尾帧图生视频 | 1.5pro, 1.0pro, 1.0lite-i2v | 首帧: `first_frame`，尾帧: `last_frame` | 传入 2 个 image_url 对象，role 必填。首尾帧可相同，宽高比不一致时以首帧为主 |
| 参考图生视频 | 1.0lite-i2v | `reference_image` | 传入 1~4 个 image_url 对象，role 必填 |

> **注意**: 首帧、首尾帧、参考图为 3 种互斥场景，不支持混用。

**参考图提示词技巧：**
- 可用自然语言指定多张图片组合
- 推荐使用 `[图1]xxx，[图2]xxx` 格式指定图片
- 示例：`[图1]戴着眼镜穿着蓝色T恤的男生和[图2]的柯基小狗，坐在[图3]的草坪上，3D卡通风格`

#### 样片信息 (draft_task) - 仅 Seedance 1.5 pro

```json
{
    "type": "draft_task",
    "draft_task": {"id": "样片任务ID"}
}
```

- `content.type`: 固定为 `draft_task`
- `content.draft_task.id`: 样片任务 ID。平台将自动复用 Draft 视频使用的用户输入（model、content.text、content.image_url、generate_audio、seed、ratio、duration、camera_fixed），生成正式视频。

**使用流程：**
1. 调用本接口，设置 `"draft": true` 生成 Draft 视频
2. 确认 Draft 视频符合预期后，基于返回的 Draft 视频任务 ID 调用本接口生成最终视频

### callback_url (string, 可选)

回调通知地址。当视频生成任务有状态变化时，向此地址推送 POST 请求。回调内容结构与查询任务 API 返回体一致。

回调状态：
- `queued`: 排队中
- `running`: 任务运行中
- `succeeded`: 任务成功（发送失败时 5 秒内重试，最多回调 3 次）
- `failed`: 任务失败（发送失败时 5 秒内重试，最多回调 3 次）
- `expired`: 任务超时

### return_last_frame (boolean, 默认 false)

- `true`: 返回生成视频的尾帧图像（png 格式，无水印），可通过查询接口获取。可实现多段连续视频：以上一个视频的尾帧作为下一个视频的首帧。
- `false`: 不返回尾帧图像

### service_tier (string, 默认 "default")

- `default`: 在线推理模式，RPM 和并发数配额较低，适合对时效性要求高的场景
- `flex`: 离线推理模式，TPD 配额更高，**价格为在线推理的 50%**，适合对时延要求不高的场景

> 不支持修改已提交任务的服务等级。

### execution_expires_after (integer, 默认 172800)

任务超时阈值（秒），从 created_at 时间戳开始计算。默认 172800 秒（48 小时）。取值范围：[3600, 259200]。超时后任务自动终止，标记为 expired 状态。

### generate_audio (boolean, 默认 true) - 仅 Seedance 1.5 pro

控制生成的视频是否包含与画面同步的声音。
- `true`: 视频包含同步音频。模型基于文本提示词与视觉内容，自动生成匹配的人声、音效及背景音乐。**建议将对话部分置于双引号内**，以优化音频生成效果。
- `false`: 无声视频

示例提示词：`男人叫住女人说："你记住，以后不可以用手指指月亮。"`

### draft (boolean, 默认 false) - 仅 Seedance 1.5 pro

- `true`: 开启样片模式，使用 480p 分辨率生成预览视频，快速验证场景结构、镜头调度、主体动作与 prompt 意图。消耗 token 数更少，成本更低。
  - 不支持其他分辨率
  - 不支持返回尾帧功能
  - 不支持离线推理功能
- `false`: 正常生成视频

### resolution (string)

视频分辨率。

| 模型 | 默认值 | 可选值 |
|------|--------|--------|
| Seedance 1.5 pro | 720p | 480p, 720p, 1080p |
| Seedance 1.0 lite | 720p | 480p, 720p, 1080p（参考图不支持 1080p） |
| Seedance 1.0 pro/pro fast | 1080p | 480p, 720p, 1080p |

### ratio (string)

生成视频的宽高比。

| 取值 | 说明 |
|------|------|
| `16:9` | 横屏（文生视频默认值） |
| `4:3` | 传统比例 |
| `1:1` | 正方形 |
| `3:4` | 竖屏 |
| `9:16` | 手机竖屏 |
| `21:9` | 超宽屏 |
| `adaptive` | 自动选择（图生视频默认值，Seedance 1.5 pro 文生视频默认值） |

**adaptive 规则：**
- 文生视频：根据提示词自动选择（仅 Seedance 1.5 pro）
- 首帧/首尾帧图生视频：根据首帧图片比例自动选择
- 参考图生视频：不支持 adaptive

**宽高像素值对照表 (Seedance 1.5 pro)：**

| 分辨率 | 16:9 | 4:3 | 1:1 | 3:4 | 9:16 | 21:9 |
|--------|------|-----|-----|-----|------|------|
| 480p | 864×496 | 752×560 | 640×640 | 560×752 | 496×864 | 992×432 |
| 720p | 1280×720 | 1112×834 | 960×960 | 834×1112 | 720×1280 | 1470×630 |
| 1080p | 1920×1080 | 1664×1248 | 1440×1440 | 1248×1664 | 1080×1920 | 2206×946 |

**宽高像素值对照表 (Seedance 1.0 系列)：**

| 分辨率 | 16:9 | 4:3 | 1:1 | 3:4 | 9:16 | 21:9 |
|--------|------|-----|-----|-----|------|------|
| 480p | 864×480 | 736×544 | 640×640 | 544×736 | 480×864 | 960×416 |
| 720p | 1248×704 | 1120×832 | 960×960 | 832×1120 | 704×1248 | 1504×640 |
| 1080p | 1920×1088 | 1664×1248 | 1440×1440 | 1248×1664 | 1088×1920 | 2176×928 |

> 图生视频选择的宽高比与上传图片不一致时，会居中裁剪适配。

### duration (integer, 默认 5)

视频时长（秒），支持 2~12 秒。`duration` 和 `frames` 二选一，`frames` 优先级更高。

**Seedance 1.5 pro 特殊支持：**
- 指定具体时长：[4, 12] 范围内任一整数
- 设置为 `-1`：由模型自主选择合适的视频长度（整数秒），实际时长可通过查询接口获取

### frames (integer) - Seedance 1.5 pro 暂不支持

生成视频的帧数。取值范围：[29, 289] 区间内所有满足 `25 + 4n` 格式的整数值（n 为正整数）。

计算公式：帧数 = 时长 × 帧率(24)

示例：需要 2.4 秒视频，帧数 = 2.4×24 = 57.6，最接近的合法值为 57（25+4×8），实际视频 57/24 = 2.375 秒。

### seed (integer, 默认 -1)

随机种子，取值范围 [-1, 2^32-1]。
- 不同 seed 值生成不同结果
- 相同 seed 值生成类似结果（不保证完全一致）
- `-1` 表示使用随机数

### camera_fixed (boolean, 默认 false)

- `true`: 固定摄像头（参考图场景不支持）
- `false`: 不固定摄像头

### watermark (boolean, 默认 false)

- `true`: 含水印
- `false`: 不含水印

## 响应参数

```json
{"id": "cgt-2025******-****"}
```

- `id` (string): 视频生成任务 ID，保存 7 天。创建为异步接口，需通过查询接口获取任务状态和视频 URL。

## 请求示例

### 文生视频

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-0-pro-250528",
    "content": [
        {
            "type": "text",
            "text": "写实风格，晴朗的蓝天之下，一大片白色的雏菊花田，镜头逐渐拉近，最终定格在一朵雏菊花的特写上，花瓣上有几颗晶莹的露珠"
        }
    ],
    "ratio": "16:9",
    "duration": 5,
    "watermark": false
}'
```

### 有声视频 - 首帧图生视频 (Seedance 1.5 pro)

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [
        {"type": "text", "text": "女孩微笑着对镜头说：\"今天天气真好啊\""},
        {
            "type": "image_url",
            "image_url": {"url": "https://example.com/girl.jpg"},
            "role": "first_frame"
        }
    ],
    "generate_audio": true,
    "ratio": "16:9",
    "duration": 5
}'
```

### 首尾帧图生视频

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [
        {"type": "text", "text": "人物缓缓转身"},
        {
            "type": "image_url",
            "image_url": {"url": "https://example.com/start.jpg"},
            "role": "first_frame"
        },
        {
            "type": "image_url",
            "image_url": {"url": "https://example.com/end.jpg"},
            "role": "last_frame"
        }
    ],
    "ratio": "16:9",
    "duration": 5
}'
```

### 参考图生视频 (Seedance 1.0 lite i2v)

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-0-lite-i2v",
    "content": [
        {"type": "text", "text": "[图1]戴着眼镜穿着蓝色T恤的男生和[图2]的柯基小狗，坐在[图3]的草坪上，3D卡通风格"},
        {"type": "image_url", "image_url": {"url": "https://example.com/boy.jpg"}, "role": "reference_image"},
        {"type": "image_url", "image_url": {"url": "https://example.com/dog.jpg"}, "role": "reference_image"},
        {"type": "image_url", "image_url": {"url": "https://example.com/grass.jpg"}, "role": "reference_image"}
    ],
    "ratio": "16:9",
    "duration": 5
}'
```

### 样片模式 (Seedance 1.5 pro)

```bash
# Step 1: 生成 Draft 视频
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [{"type": "text", "text": "一只猫在花园里追蝴蝶"}],
    "draft": true,
    "resolution": "480p",
    "ratio": "16:9",
    "duration": 5
}'
# 返回: {"id": "cgt-draft-xxxx"}

# Step 2: 确认 Draft 符合预期后，基于 Draft ID 生成正式视频
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [
        {"type": "draft_task", "draft_task": {"id": "cgt-draft-xxxx"}}
    ],
    "resolution": "720p"
}'
```

### Base64 编码图片

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [
        {"type": "text", "text": "人物微笑"},
        {
            "type": "image_url",
            "image_url": {"url": "data:image/png;base64,iVBORw0KGgo..."},
            "role": "first_frame"
        }
    ]
}'
```

### 新方式传参（推荐）vs 旧方式传参

**新方式（推荐）** - 在 request body 中直接传入参数（强校验，参数错误会报错）：

```json
{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [{"type": "text", "text": "小猫对着镜头打哈欠"}],
    "resolution": "720p",
    "ratio": "16:9",
    "duration": 5,
    "seed": 11,
    "camera_fixed": false,
    "watermark": true
}
```

**旧方式** - 在文本提示词后追加 `--[parameters]`（弱校验，参数错误使用默认值不报错）：

```json
{
    "model": "doubao-seedance-1-5-pro-251215",
    "content": [
        {"type": "text", "text": "小猫对着镜头打哈欠 --rs 720p --rt 16:9 --dur 5 --seed 11 --cf false --wm true"}
    ]
}
```
