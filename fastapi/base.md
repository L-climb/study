# FastAPI 基础知识教程

## 1. FastAPI 简介

FastAPI 是一个基于 Python 类型提示（Type Hints）构建的现代 Web 框架，具有以下特点：

* 开发速度快
* 自动生成 API 文档
* 支持异步编程
* 性能接近 Node.js 和 Go
* 非常适合 AI 应用开发

安装 FastAPI：

```bash
pip install fastapi uvicorn
```

启动项目：

```bash
uvicorn main:app --reload
```

---

# 2. 创建第一个接口

## 代码示例

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def hello():
    return {"msg": "hello"}
```

访问：

```text
http://127.0.0.1:8000
```

返回：

```json
{
    "msg": "hello"
}
```

---

# 3. 自动生成 API 文档

FastAPI 内置 Swagger。

启动项目后访问：

```text
http://127.0.0.1:8000/docs
```

ReDoc 文档：

```text
http://127.0.0.1:8000/redoc
```

无需手动编写接口文档。

---

# 4. 路径参数

## 代码示例

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/user/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

请求：

```text
GET /user/100
```

返回：

```json
{
    "user_id": 100
}
```

---

# 5. 查询参数

## 代码示例

```python
@app.get("/search")
def search(keyword: str):
    return {"keyword": keyword}
```

请求：

```text
GET /search?keyword=fastapi
```

返回：

```json
{
    "keyword": "fastapi"
}
```

---

# 6. POST 请求

POST 常用于提交数据。

## 代码示例

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

@app.post("/user")
def create_user(user: User):
    return user
```

请求：

```json
{
    "name": "Tom",
    "age": 20
}
```

返回：

```json
{
    "name": "Tom",
    "age": 20
}
```

---

# 7. Pydantic 数据模型

FastAPI 使用 Pydantic 进行数据验证。

## 定义模型

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
```

## 自动验证

错误请求：

```json
{
    "name": "Tom",
    "age": "abc"
}
```

返回：

```json
{
    "detail": [...]
}
```

FastAPI 自动校验数据类型。

---

# 8. 请求体(Request Body)

## 代码示例

```python
class Item(BaseModel):
    name: str
    price: float

@app.post("/item")
def create_item(item: Item):
    return item
```

请求：

```json
{
    "name": "Keyboard",
    "price": 199.9
}
```

---

# 9. 返回响应(Response)

## 返回字典

```python
@app.get("/")
def root():
    return {"message": "success"}
```

---

## 返回状态码

```python
from fastapi import status

@app.post("/create", status_code=status.HTTP_201_CREATED)
def create():
    return {"msg": "created"}
```

---

# 10. 请求头(Header)

## 获取 Header

```python
from fastapi import Header

@app.get("/info")
def info(user_agent: str = Header()):
    return {"user_agent": user_agent}
```

---

# 11. 文件上传

## 单文件上传

```python
from fastapi import UploadFile

@app.post("/upload")
async def upload(file: UploadFile):
    return {"filename": file.filename}
```

---

## 保存文件

```python
@app.post("/upload")
async def upload(file: UploadFile):

    with open(file.filename, "wb") as f:
        f.write(await file.read())

    return {"msg": "success"}
```

---

# 12. 异步接口

FastAPI 推荐使用异步。

## 示例

```python
@app.get("/")
async def hello():
    return {"msg": "hello"}
```

---

## 调用异步函数

```python
import asyncio

@app.get("/wait")
async def wait():

    await asyncio.sleep(3)

    return {"msg": "done"}
```

---

# 13. 数据库连接

安装：

```bash
pip install sqlalchemy
```

创建数据库连接：

```python
from sqlalchemy import create_engine

DATABASE_URL = "sqlite:///test.db"

engine = create_engine(DATABASE_URL)
```

---

# 14. 依赖注入(Dependency Injection)

FastAPI 核心特性。

## 示例

```python
from fastapi import Depends

def get_token():
    return "token"

@app.get("/")
def hello(token=Depends(get_token)):
    return {"token": token}
```

---

# 15. JWT 用户认证

安装：

```bash
pip install python-jose passlib
```

常见流程：

```text
用户登录
    ↓
验证账号密码
    ↓
生成JWT
    ↓
返回Token
    ↓
客户端保存Token
    ↓
后续请求携带Token
```

请求头：

```text
Authorization: Bearer xxxxxx
```

---

# 16. 流式输出（AI应用重点）

适用于：

* ChatGPT
* Claude
* DeepSeek
* Ollama

## 示例

```python
from fastapi.responses import StreamingResponse

async def generate():

    for i in range(5):
        yield f"token-{i}\n"

@app.get("/stream")
async def stream():
    return StreamingResponse(generate())
```

---

# 17. 项目目录结构

推荐结构：

```text
project/

├── app
│   ├── api
│   ├── models
│   ├── schemas
│   ├── services
│   └── database
│
├── tests
│
├── docs
│
├── main.py
│
└── requirements.txt
```

---

# 18. AI应用接口示例

调用 Ollama：

```python
import requests

@app.post("/chat")
def chat(question: str):

    response = requests.post(
        "http://localhost:11434/api/generate",
        json={
            "model": "qwen3",
            "prompt": question
        }
    )

    return response.json()
```

---

# 19. 常用 HTTP 状态码

| 状态码 | 含义    |
| --- | ----- |
| 200 | 成功    |
| 201 | 创建成功  |
| 400 | 请求错误  |
| 401 | 未授权   |
| 403 | 禁止访问  |
| 404 | 资源不存在 |
| 500 | 服务器错误 |

---

# 20. FastAPI 学习路线

```text
FastAPI基础
    ↓
Pydantic
    ↓
HTTP协议
    ↓
异步编程
    ↓
SQLAlchemy
    ↓
JWT认证
    ↓
文件上传
    ↓
流式输出
    ↓
Docker部署
    ↓
OpenAI/Ollama集成
    ↓
RAG系统开发
    ↓
LangChain/LangGraph
```

掌握以上内容后，即可开发：

* AI 聊天助手
* 知识库问答系统
* Agent 系统
* 工作流平台
* Dify 插件
* 企业级 AI 应用
