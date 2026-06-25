下面这份是我比较推荐的 **Python 基础知识速查文档（基础版）**，适合已经会一点编程、准备快速复习 Python 的人。内容覆盖面试、日常开发、FastAPI、爬虫、AI 应用开发所必须掌握的基础知识。

````markdown
# Python 基础知识速查（基础版）

# 1. 变量与数据类型

## 变量定义

```python
name = "Tom"
age = 20
height = 1.75
is_student = True
````

Python 是动态类型语言，不需要声明类型。

---

## 常见数据类型

| 类型    | 示例             |
| ----- | -------------- |
| int   | 10             |
| float | 3.14           |
| str   | "hello"        |
| bool  | True           |
| list  | [1,2,3]        |
| tuple | (1,2,3)        |
| dict  | {"name":"Tom"} |
| set   | {1,2,3}        |

查看类型：

```python
print(type(age))
```

---

# 2. 输入与输出

## 输出

```python
name = "Tom"

print(name)

print("姓名:", name)

print(f"姓名:{name}")
```

推荐使用 f-string：

```python
age = 20

print(f"年龄:{age}")
```

---

## 输入

```python
name = input("请输入姓名：")
print(name)
```

input 返回值永远是字符串。

```python
age = int(input("请输入年龄："))
```

---

# 3. 运算符

## 算术运算

```python
a = 10
b = 3

a + b
a - b
a * b
a / b
a // b
a % b
a ** b
```

---

## 比较运算

```python
==
!=
>
<
>=
<=
```

---

## 逻辑运算

```python
and
or
not
```

示例：

```python
age = 18

if age >= 18 and age < 60:
    print("成年人")
```

---

# 4. 条件判断

## if

```python
age = 20

if age >= 18:
    print("成年")
```

---

## if-else

```python
if age >= 18:
    print("成年")
else:
    print("未成年")
```

---

## if-elif-else

```python
score = 85

if score >= 90:
    print("A")
elif score >= 80:
    print("B")
else:
    print("C")
```

---

# 5. 循环

## while

```python
i = 1

while i <= 5:
    print(i)
    i += 1
```

---

## for

```python
for i in range(5):
    print(i)
```

输出：

```text
0
1
2
3
4
```

---

## range

```python
range(5)

range(1, 5)

range(1, 10, 2)
```

---

# 6. 字符串

## 创建字符串

```python
s = "hello"
```

---

## 常用操作

```python
len(s)

s.upper()

s.lower()

s.replace("h", "H")

s.split(",")

",".join(["a","b","c"])
```

---

## 切片

```python
s = "abcdef"

s[0]

s[-1]

s[1:4]

s[:3]

s[::2]
```

结果：

```text
abc
ace
```

---

# 7. 列表 List

## 创建列表

```python
nums = [1,2,3]
```

---

## 增删改查

```python
nums.append(4)

nums.insert(0,100)

nums.remove(2)

nums.pop()

nums[0] = 999

print(nums[0])
```

---

## 遍历

```python
for n in nums:
    print(n)
```

---

## 排序

```python
nums.sort()

nums.sort(reverse=True)
```

---

# 8. 元组 Tuple

## 创建

```python
t = (1,2,3)
```

特点：

```text
不可修改
速度快
可作为字典键
```

访问：

```python
print(t[0])
```

---

# 9. 字典 Dict

## 创建

```python
user = {
    "name":"Tom",
    "age":20
}
```

---

## 获取值

```python
print(user["name"])

print(user.get("name"))
```

---

## 修改

```python
user["age"] = 25
```

---

## 新增

```python
user["gender"] = "male"
```

---

## 删除

```python
del user["age"]
```

---

## 遍历

```python
for k,v in user.items():
    print(k,v)
```

---

# 10. 集合 Set

## 创建

```python
s = {1,2,3}
```

特点：

```text
无序
元素唯一
自动去重
```

---

## 操作

```python
s.add(4)

s.remove(1)
```

---

# 11. 函数

## 定义函数

```python
def add(a,b):
    return a+b
```

调用：

```python
result = add(1,2)
```

---

## 默认参数

```python
def hello(name="Tom"):
    print(name)
```

---

## 关键字参数

```python
hello(name="Jack")
```

---

# 12. 异常处理

## try-except

```python
try:
    num = int(input())
except:
    print("输入错误")
```

---

## 捕获指定异常

```python
try:
    1 / 0
except ZeroDivisionError:
    print("除零错误")
```

---

# 13. 文件操作

## 写文件

```python
with open("a.txt","w") as f:
    f.write("hello")
```

---

## 读文件

```python
with open("a.txt","r") as f:
    content = f.read()
```

---

## 读取每行

```python
with open("a.txt") as f:
    for line in f:
        print(line)
```

---

# 14. 模块导入

## 导入整个模块

```python
import math

print(math.sqrt(16))
```

---

## 导入指定内容

```python
from math import sqrt

print(sqrt(16))
```

---

# 15. 面向对象基础

## 定义类

```python
class Student:

    def __init__(self,name,age):
        self.name = name
        self.age = age

    def study(self):
        print("学习")
```

---

## 创建对象

```python
s = Student("Tom",20)

print(s.name)

s.study()
```

---

# 16. 常用内置函数

```python
len()

type()

print()

input()

range()

sum()

max()

min()

sorted()

enumerate()

zip()
```

示例：

```python
nums = [1,2,3]

print(sum(nums))
print(max(nums))
```

---

# 17. 列表推导式

传统写法：

```python
result = []

for i in range(5):
    result.append(i)
```

简写：

```python
result = [i for i in range(5)]
```

条件：

```python
result = [i for i in range(10) if i % 2 == 0]
```

---

# 18. Python 常见真值

为 False：

```python
False
None
0
0.0
''
[]
{}
set()
```

其余大多数对象都为 True。

---

# 19. 必须掌握的关键字

```python
if
else
elif

for
while

break
continue

def
return

class

try
except
finally

import
from

and
or
not

in
is

with
```

---

# 20. 新手必须理解的概念

## 变量引用

```python
a = [1,2,3]

b = a
```

此时：

```python
b.append(4)
```

a 也会变化。

---

## == 与 is

```python
==
```

比较值。

```python
is
```

比较对象地址。

```python
a = [1]
b = [1]

a == b   # True
a is b   # False
```

---

# 基础阶段掌握目标

能够独立完成：

✓ 条件判断

✓ 循环

✓ 函数封装

✓ 文件读写

✓ 字典与列表操作

✓ 简单面向对象

✓ 异常处理

✓ 使用第三方库

达到这个水平后即可开始学习：

* FastAPI
* Flask
* 爬虫
* 数据分析(Pandas)
* AI应用开发
* LangChain
* Dify插件开发

```

对于你后面想做的 **AI 应用开发工程师（FastAPI + Dify + LangChain）**，下一阶段最重要的并不是更复杂的语法，而是：
1. 函数高级用法
2. 面向对象（OOP）
3. 装饰器
4. 迭代器与生成器
5. typing 类型注解
6. 异步编程（async/await）
7. 模块与包管理
8. 虚拟环境与 pip

这些:contentReference[oaicite:0]{index=0}，内容会更贴近 FastAPI、Dify 插件开发和大模型应用开发。
```
