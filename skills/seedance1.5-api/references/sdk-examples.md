# SDK 使用示例

火山方舟提供 Python、Go、Java 三种官方 SDK。

## SDK 安装

| 语言 | 包名 | 安装 |
|------|------|------|
| Python | `volcenginesdkarkruntime` | `pip install volcenginesdkarkruntime` |
| Go | `volcengine-go-sdk` | `go get github.com/volcengine/volcengine-go-sdk` |
| Java | `volcengine-java-sdk-ark-runtime` | Maven/Gradle 依赖 |

## 初始化客户端

### Python

```python
import os
from volcenginesdkarkruntime import Ark

# API Key 鉴权
client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    timeout=1800,     # 推荐 30 分钟
    max_retries=2,    # 默认重试 2 次
)
```

### Go

```go
import (
    "os"
    "time"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
)

client := arkruntime.NewClientWithApiKey(
    os.Getenv("ARK_API_KEY"),
    arkruntime.WithTimeout(30*time.Minute),
    arkruntime.WithRetryTimes(2),
    arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
)
```

### Java

```java
import com.volcengine.ark.runtime.service.ArkService;

ArkService service = ArkService.builder()
    .baseUrl("https://ark.cn-beijing.volces.com/api/v3")
    .apiKey(apiKey)
    .timeout(Duration.ofSeconds(1800))
    .connectTimeout(Duration.ofSeconds(20))
    .retryTimes(2)
    .build();
```

## Access Key 鉴权（替代 API Key）

务必使用单例模式进行请求。

### Python

```python
from volcenginesdkarkruntime import Ark

client = Ark(
    ak=os.environ.get("VOLC_ACCESSKEY"),
    sk=os.environ.get("VOLC_SECRETKEY"),
    base_url="https://ark.cn-beijing.volces.com/api/v3",
)
```

### Go

```go
client := arkruntime.NewClientWithAkSk(
    os.Getenv("VOLC_ACCESSKEY"),
    os.Getenv("VOLC_SECRETKEY"),
    arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
)
```

### Java

```java
ArkService service = ArkService.builder()
    .ak(ak)
    .sk(sk)
    .baseUrl("https://ark.cn-beijing.volces.com/api/v3")
    .build();
```

## 超时与重试设置

推荐值：
- 超时（timeout）：30 分钟（1800 秒）
- 重试次数（max_retries）：2 次

| SDK | 默认连接超时 | 默认 socket 超时 |
|-----|------------|-----------------|
| Python | 60 秒 | 600 秒 |
| Java | 60 秒 | 600 秒 |
| Go | 600 秒（端到端） | - |

## 视频生成完整示例 (Python)

```python
import os
import time
from volcenginesdkarkruntime import Ark

client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    timeout=1800,
)

# 文生视频
task = client.content.generations.tasks.create(
    model="doubao-seedance-1-5-pro-251215",
    content=[{"type": "text", "text": "一只小猫在花园里追蝴蝶，阳光明媚"}],
    ratio="16:9",
    duration=5,
    resolution="720p",
    generate_audio=True,
)
print(f"Task created: {task.id}")

# 轮询等待完成
while True:
    result = client.content.generations.tasks.get(task.id)
    if result.status in ("succeeded", "failed", "expired", "cancelled"):
        break
    print(f"Status: {result.status}")
    time.sleep(10)

if result.status == "succeeded":
    print(f"Video URL: {result.content.video_url}")
    print(f"Tokens used: {result.usage.total_tokens}")
else:
    print(f"Failed: {result.error}")
```

## 图生视频示例 (Python)

```python
import os
import base64
from volcenginesdkarkruntime import Ark

client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    timeout=1800,
)

# 方式1：使用图片 URL
task = client.content.generations.tasks.create(
    model="doubao-seedance-1-5-pro-251215",
    content=[
        {"type": "text", "text": "人物缓缓转身，微笑"},
        {
            "type": "image_url",
            "image_url": {"url": "https://example.com/photo.jpg"},
            "role": "first_frame",
        },
    ],
    ratio="16:9",
    duration=5,
)

# 方式2：使用 Base64 编码
with open("image.png", "rb") as f:
    b64 = base64.b64encode(f.read()).decode()

task = client.content.generations.tasks.create(
    model="doubao-seedance-1-5-pro-251215",
    content=[
        {"type": "text", "text": "人物缓缓转身"},
        {
            "type": "image_url",
            "image_url": {"url": f"data:image/png;base64,{b64}"},
            "role": "first_frame",
        },
    ],
)
```

## 连续视频生成示例 (Python)

使用 `return_last_frame` 实现多段连续视频：

```python
# 第一段视频，返回尾帧
task1 = client.content.generations.tasks.create(
    model="doubao-seedance-1-5-pro-251215",
    content=[{"type": "text", "text": "女孩走进咖啡馆"}],
    return_last_frame=True,
    duration=5,
)

# 等待完成...
result1 = client.content.generations.tasks.get(task1.id)
last_frame_url = result1.content.last_frame_url

# 第二段视频，使用上一段的尾帧作为首帧
task2 = client.content.generations.tasks.create(
    model="doubao-seedance-1-5-pro-251215",
    content=[
        {"type": "text", "text": "女孩坐下来点了一杯咖啡"},
        {
            "type": "image_url",
            "image_url": {"url": last_frame_url},
            "role": "first_frame",
        },
    ],
    return_last_frame=True,
    duration=5,
)
```

## 自定义 Header

### 问题定位 - 设置请求 ID

```python
# Python
completion = client.chat.completions.create(
    model="doubao-seed-1-6-251015",
    messages=[{"role": "user", "content": "Hello"}],
    extra_headers={"X-Client-Request-Id": "My-Request-Id"},
)
```

```go
// Go
resp, err := client.CreateChatCompletion(
    ctx, req,
    arkruntime.WithCustomHeader(model.ClientRequestHeader, "my-request-id"),
)
```

## 单例模式

请使用单例方式请求模型服务，勿重复创建实例导致额外资源消耗：

```python
# 正确：创建一个 client 实例，多次使用
client = Ark(api_key=os.environ.get("ARK_API_KEY"))
result1 = client.content.generations.tasks.create(...)
result2 = client.content.generations.tasks.create(...)

# 错误：每次请求都创建新实例
# client1 = Ark(api_key=os.environ.get("ARK_API_KEY"))
# client2 = Ark(api_key=os.environ.get("ARK_API_KEY"))
```
