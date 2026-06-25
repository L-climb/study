如果你是做 **AI 应用开发工程师**，学习 LangChain 不需要研究所有高级特性，但以下内容属于 **面试必问 + 项目必用 + LangGraph/Dify/Agent 必备基础**。

我按照 **必须掌握（★★★★★）→ 了解即可（★★★）** 来整理。

---

# LangChain 必会知识

## 1. LangChain是什么 ★★★★★

LangChain 本质上是：

> 用来开发 LLM 应用的框架

帮助开发者完成：

* Prompt管理
* LLM调用
* RAG
* Agent
* Tool调用
* Memory管理
* Workflow编排

典型流程：

```text
用户问题
    ↓
Prompt
    ↓
LLM
    ↓
工具调用
    ↓
结果返回
```

---

# 2. LangChain核心组件 ★★★★★

LangChain面试最爱问：

```text
Prompt
LLM
Output Parser
Chain
Memory
Tool
Agent
Retriever
VectorStore
```

---

# 3. PromptTemplate ★★★★★

作用：

```python
from langchain_core.prompts import PromptTemplate

prompt = PromptTemplate.from_template(
    "请介绍一下{city}"
)

prompt.invoke({"city":"北京"})
```

结果：

```text
请介绍一下北京
```

---

## 多变量

```python
prompt = PromptTemplate.from_template(
    """
    你是一名{role}
    请介绍{topic}
    """
)
```

调用：

```python
prompt.invoke({
    "role":"老师",
    "topic":"机器学习"
})
```

---

# 4. ChatPromptTemplate ★★★★★

最常用。

```python
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages(
    [
        ("system","你是一名Python专家"),
        ("human","解释一下{topic}")
    ]
)
```

调用：

```python
prompt.invoke({
    "topic":"装饰器"
})
```

---

# 5. ChatModel ★★★★★

以前：

```python
OpenAI()
```

现在：

```python
ChatOpenAI()
```

例如：

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="gpt-4o"
)
```

调用：

```python
llm.invoke("你好")
```

---

# 6. Output Parser ★★★★★

LLM输出通常是字符串。

有时候希望得到结构化数据。

---

## StrOutputParser

```python
from langchain_core.output_parsers import StrOutputParser

parser = StrOutputParser()
```

---

## JsonOutputParser

```python
from langchain_core.output_parsers import JsonOutputParser
```

输出：

```json
{
  "name":"Tom",
  "age":20
}
```

---

# 7. LCEL表达式 ★★★★★

LangChain 0.1之后最重要。

通过：

```python
|
```

连接组件。

---

传统写法：

```python
chain = LLMChain(...)
```

已经基本淘汰。

---

现代写法：

```python
chain = prompt | llm | parser
```

执行：

```python
chain.invoke({
    "topic":"机器学习"
})
```

---

这是LangChain最核心知识点。

必须熟练。

---

# 8. Runnable ★★★★★

LCEL底层全部是Runnable。

常见方法：

```python
invoke()
```

单次执行

---

```python
batch()
```

批量执行

---

```python
stream()
```

流式输出

---

```python
ainvoke()
```

异步执行

---

示例：

```python
for chunk in chain.stream(
    {"topic":"Python"}
):
    print(chunk)
```

---

# 9. Chain思想 ★★★★★

Chain：

```text
多个步骤串起来
```

例如：

```text
用户问题
 ↓
Prompt
 ↓
LLM
 ↓
解析结果
 ↓
返回
```

代码：

```python
chain = prompt | llm | parser
```

---

# 10. Memory ★★★★

让模型记住历史对话。

---

早期：

```python
ConversationBufferMemory
```

---

现在：

```python
ChatMessageHistory
```

更常用。

---

存储：

```python
history.add_user_message("你好")

history.add_ai_message("你好")
```

---

# 11. Tool工具 ★★★★★

Agent核心。

工具就是：

```python
函数
```

---

定义工具：

```python
from langchain.tools import tool

@tool
def add(a:int,b:int):
    return a+b
```

---

工具描述非常重要。

Agent靠描述判断是否调用。

```python
@tool
def weather(city:str):
    """
    查询城市天气
    """
```

---

# 12. Agent ★★★★★

Agent = 大模型 + 工具

---

普通LLM：

```text
问题
 ↓
回答
```

---

Agent：

```text
问题
 ↓
判断是否需要工具
 ↓
调用工具
 ↓
获得结果
 ↓
总结回答
```

---

例如：

用户：

```text
北京天气怎么样
```

Agent：

```text
调用天气工具
```

得到：

```text
28℃
```

然后回复：

```text
北京当前28℃
```

---

# 13. Agent执行流程(ReAct) ★★★★★

经典流程：

```text
Thought
Action
Observation
```

---

例如：

```text
Question:
北京天气

Thought:
需要查天气

Action:
weather_tool

Observation:
28℃
```

---

最后：

```text
Answer:
北京当前28℃
```

---

# 14. RAG ★★★★★

面试出现频率最高。

---

RAG：

```text
Retrieval
Augmented
Generation
```

即：

```text
检索增强生成
```

---

流程：

```text
用户问题
 ↓
向量检索
 ↓
找到相关文档
 ↓
拼接Prompt
 ↓
LLM回答
```

---

# 15. Document ★★★★★

LangChain统一文档格式。

```python
from langchain_core.documents import Document
```

---

创建：

```python
doc = Document(
    page_content="LangChain教程",
    metadata={"source":"pdf"}
)
```

---

结构：

```python
doc.page_content

doc.metadata
```

---

# 16. Text Splitter ★★★★★

切分文档。

---

为什么切？

```text
上下文长度有限
```

---

常见：

```python
RecursiveCharacterTextSplitter
```

---

```python
splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,
    chunk_overlap=100
)
```

---

# 17. Embedding ★★★★★

向量化模型。

作用：

```text
文本
 ↓
向量
```

例如：

```python
OpenAIEmbeddings()
```

---

输出：

```python
[0.12,0.55,...]
```

---

# 18. Vector Store ★★★★★

向量数据库。

常见：

```text
FAISS
Chroma
Milvus
Qdrant
Weaviate
PGVector
```

---

最常用：

```python
FAISS
```

```python
vectorstore = FAISS.from_documents(
    docs,
    embeddings
)
```

---

# 19. Retriever ★★★★★

检索器。

作用：

```text
问题
 ↓
Embedding
 ↓
向量搜索
 ↓
返回相关文档
```

---

创建：

```python
retriever = vectorstore.as_retriever()
```

查询：

```python
docs = retriever.invoke(
    "什么是LangChain"
)
```

---

# 20. LangChain标准RAG流程 ★★★★★

```text
PDF
 ↓
Loader
 ↓
Splitter
 ↓
Embedding
 ↓
VectorStore
 ↓
Retriever
 ↓
Prompt
 ↓
LLM
 ↓
Answer
```

面试高频题：

> RAG完整流程是什么？

直接背这个。

---

# 21. LangGraph（新重点）★★★★★

2025以后越来越重要。

很多Agent项目已经：

```text
LangGraph
+
LangChain
```

---

本质：

```text
状态机
+
工作流
```

---

节点：

```text
Node
```

边：

```text
Edge
```

---

流程：

```text
Start
 ↓
Agent
 ↓
Tool
 ↓
Agent
 ↓
End
```

---

解决：

```text
循环
分支
状态管理
多Agent
```

问题。

---

# 面试必背知识总结

```text
PromptTemplate
ChatPromptTemplate

ChatOpenAI

OutputParser

LCEL(|)

Runnable
invoke
stream
batch

Tool

Agent

ReAct

Document

TextSplitter

Embedding

VectorStore

Retriever

RAG完整流程

LangGraph
```

如果你是准备 **AI应用开发 / Agent开发 / RAG开发岗位**，下一步建议重点复习：

```text
1. LangChain LCEL
2. Tool调用
3. Agent
4. RAG
5. LangGraph
```

这五部分基本覆盖了现在 80% 以上的 LangChain 实际项目场景。
