````markdown
# PyTorch 必会知识速查（AI应用开发 / 机器学习工程师版）

如果你已经掌握 Python 和 Pandas，那么 PyTorch 重点关注：

1. Tensor
2. 自动求导（Autograd）
3. Dataset/DataLoader
4. 模型定义
5. 损失函数
6. 优化器
7. 训练流程
8. GPU训练
9. 模型保存加载
10. 推理部署

---

# 1 Tensor（最核心）

PyTorch 一切都是 Tensor。

## 创建Tensor

```python
import torch

x = torch.tensor([1,2,3])

x = torch.zeros(3,4)

x = torch.ones(3,4)

x = torch.rand(3,4)

x = torch.randn(3,4)

x = torch.arange(0,10)

x = torch.eye(3)
````

---

## 查看属性

```python
x.shape

x.dtype

x.device

x.ndim
```

---

## 数据类型转换

```python
x.float()

x.long()

x.int()

x.bool()
```

分类标签通常：

```python
y = y.long()
```

---

## Tensor与Numpy互转

```python
import numpy as np

arr = np.array([1,2,3])

tensor = torch.from_numpy(arr)

arr2 = tensor.numpy()
```

---

# 2 Tensor操作

## reshape

```python
x = torch.arange(12)

x = x.reshape(3,4)

x = x.view(3,4)
```

---

## 维度变换

```python
x.unsqueeze(0)

x.unsqueeze(1)

x.squeeze()

x.transpose(0,1)

x.permute(0,2,1)
```

面试高频：

```python
(B,C,H,W)

↓

(B,H,W,C)
```

```python
x.permute(0,2,3,1)
```

---

## 拼接

```python
torch.cat([x1,x2],dim=0)

torch.stack([x1,x2],dim=0)
```

区别：

```python
cat
```

维度不变

```python
stack
```

增加一个维度

---

## 数学运算

```python
x+y

x-y

x*y

x/x

x**2
```

---

## 矩阵乘法

```python
torch.matmul(a,b)

a @ b
```

深度学习中大量使用。

---

# 3 GPU训练

---

## 查看GPU

```python
torch.cuda.is_available()
```

---

## 指定设备

```python
device = torch.device(
    "cuda" if torch.cuda.is_available()
    else "cpu"
)
```

---

## 放到GPU

```python
model.to(device)

x = x.to(device)

y = y.to(device)
```

---

## 查看显存

```python
torch.cuda.memory_allocated()
```

---

# 4 自动求导 Autograd

PyTorch核心机制。

---

## 开启梯度

```python
x = torch.tensor(
    2.0,
    requires_grad=True
)
```

---

## 反向传播

```python
y = x**2

y.backward()

print(x.grad)
```

结果：

```python
4
```

因为：

```text
d(x²)/dx = 2x
```

---

## 清空梯度

```python
optimizer.zero_grad()
```

必须写。

否则梯度累加。

---

# 5 Dataset与DataLoader

训练数据标准写法。

---

## Dataset

```python
from torch.utils.data import Dataset

class MyDataset(Dataset):

    def __init__(self):
        ...

    def __len__(self):
        ...

    def __getitem__(self, idx):
        return x,y
```

必须实现：

```python
__len__()

__getitem__()
```

---

## DataLoader

```python
from torch.utils.data import DataLoader

loader = DataLoader(
    dataset,
    batch_size=32,
    shuffle=True
)
```

---

## 遍历

```python
for x,y in loader:
    ...
```

---

# 6 神经网络定义

---

## 继承nn.Module

```python
import torch.nn as nn

class Net(nn.Module):

    def __init__(self):
        super().__init__()

    def forward(self,x):
        return x
```

---

## 全连接层

```python
nn.Linear(128,64)
```

表示：

```text
128 -> 64
```

---

## 卷积层

```python
nn.Conv2d(
    in_channels=3,
    out_channels=32,
    kernel_size=3
)
```

---

## 激活函数

```python
nn.ReLU()

nn.Sigmoid()

nn.Tanh()

nn.GELU()
```

目前最常见：

```python
ReLU

GELU
```

---

## Dropout

```python
nn.Dropout(0.5)
```

防止过拟合。

---

# 7 Sequential

快速搭模型。

```python
model = nn.Sequential(
    nn.Linear(128,64),
    nn.ReLU(),
    nn.Linear(64,10)
)
```

---

# 8 损失函数（必须掌握）

---

## 分类

### 二分类

```python
nn.BCEWithLogitsLoss()
```

最常用。

---

### 多分类

```python
nn.CrossEntropyLoss()
```

最常见面试题。

注意：

```python
不要先softmax
```

因为内部已经做了。

---

## 回归

```python
nn.MSELoss()

nn.L1Loss()
```

---

# 9 优化器

---

## SGD

```python
torch.optim.SGD(
    model.parameters(),
    lr=0.01
)
```

---

## Adam

实际最常用：

```python
torch.optim.Adam(
    model.parameters(),
    lr=1e-3
)
```

---

# 10 标准训练流程（面试必背）

```python
for epoch in range(epochs):

    model.train()

    for x,y in loader:

        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad()

        pred = model(x)

        loss = criterion(pred,y)

        loss.backward()

        optimizer.step()
```

---

# 11 验证流程

关闭梯度。

```python
model.eval()

with torch.no_grad():

    for x,y in val_loader:

        pred = model(x)
```

---

## train和eval区别

```python
model.train()
```

开启：

```python
Dropout

BatchNorm
```

训练模式

---

```python
model.eval()
```

验证模式

---

# 12 保存与加载模型

---

## 保存参数（推荐）

```python
torch.save(
    model.state_dict(),
    "model.pth"
)
```

---

## 加载参数

```python
model.load_state_dict(
    torch.load("model.pth")
)
```

---

## 完整保存

```python
torch.save(model,"model.pth")
```

一般不推荐。

---

# 13 常见网络层速查

```python
nn.Linear
```

全连接

---

```python
nn.Conv2d
```

卷积

---

```python
nn.MaxPool2d
```

池化

---

```python
nn.BatchNorm2d
```

归一化

---

```python
nn.Dropout
```

随机失活

---

```python
nn.Embedding
```

词向量

NLP必备

---

```python
nn.LSTM
```

循环神经网络

---

```python
nn.MultiheadAttention
```

Transformer核心

---

# 14 维度必须牢记

## 表格数据

```python
(batch_size, features)
```

例如：

```python
(32,128)
```

---

## 图片

```python
(batch_size, channels, height, width)
```

例如：

```python
(32,3,224,224)
```

---

## NLP

```python
(batch_size, seq_len)
```

例如：

```python
(32,512)
```

---

# 15 PyTorch面试高频问题

## loss.backward()做什么

计算梯度。

---

## optimizer.step()做什么

更新参数。

---

## optimizer.zero_grad()为什么要写

防止梯度累加。

---

## model.train()和eval()区别

控制：

```text
Dropout
BatchNorm
```

行为。

---

## CrossEntropyLoss前要不要Softmax

不要。

---

## torch.no_grad()作用

关闭梯度计算。

减少显存。

加快推理。

---

# AI应用开发最常用模块

```python
torch
torch.nn
torch.optim
torch.utils.data
torchvision
transformers
```

如果你的目标是：

* AI应用开发工程师
* RAG工程师
* Agent工程师
* LLM应用工程师

那么真正需要熟练掌握的是：

Tensor → Dataset → DataLoader → nn.Module → Loss → Optimizer → Train/Eval → GPU → Save/Load

掌握这条主线，已经覆盖了 80% 以上的 PyTorch 日常开发场景。

```
```
