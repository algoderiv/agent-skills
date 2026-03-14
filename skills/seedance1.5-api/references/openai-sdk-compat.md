# 兼容 OpenAI SDK

火山方舟模型 API 兼容 OpenAI API。修改 `base_url`、`model`、`api_key` 等少量配置，即可将方舟模型服务集成到已有系统中。

> 注意：社区第三方 SDK 不由火山引擎团队维护，本文仅供参考。

## 前提条件

- Python 版本：3.7 及以上
- OpenAI SDK：1.0 版本及以上

```bash
pip install --upgrade "openai>=1.0"
```

## 快速开始

```python
from openai import OpenAI
import os

client = OpenAI(
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    api_key=os.environ.get("ARK_API_KEY"),
)

# Chat 对话
completion = client.chat.completions.create(
    model="doubao-seed-1-6-251015",  # 替换为实际 Model ID
    messages=[
        {"role": "user", "content": "Hello"},
    ],
)
print(completion.choices[0].message.content)
```

## 设置额外字段

传入 OpenAI SDK 中不支持的字段，通过 `extra_body` 字典传入：

```python
completion = client.chat.completions.create(
    model="doubao-seed-1-6-251015",
    messages=[
        {"role": "user", "content": "Hello"},
    ],
    extra_body={
        "thinking": {
            "type": "disabled",   # 不使用深度思考能力
            # "type": "enabled",  # 使用深度思考能力
        }
    },
)
```

## 设置自定义 Header

```python
completion = client.chat.completions.create(
    model="doubao-seed-1-6-251015",
    messages=[
        {"role": "user", "content": "Hello"},
    ],
    extra_headers={"X-Client-Request-Id": "202406251728190000B7EA7A9648AC08D9"},
)
```

## LangChain 集成

```bash
pip install langchain-openai
```

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
import os

llm = ChatOpenAI(
    openai_api_key=os.environ.get("ARK_API_KEY"),
    openai_api_base="https://ark.cn-beijing.volces.com/api/v3",
    model="doubao-seed-1-6-251015",  # 替换为实际 Model ID
)

template = """Question: {question}

Answer: Let's think step by step."""

prompt = PromptTemplate.from_template(template)
question = "What NFL team won the Super Bowl in the year Justin Beiber was born?"
llm_chain = prompt | llm
print(llm_chain.invoke(question))
```

## 注意事项

- 文本向量化模型已逐步下线，建议使用多模态向量化模型
- 多模态向量化能力模型不支持 OpenAI API，请使用方舟 SDK
- 视频生成 API 的接口路径与 OpenAI 不同（`/contents/generations/tasks`），对话 API 路径兼容（`/chat/completions`）
