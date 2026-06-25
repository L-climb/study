如果你的目标是 **AI 应用开发、数据处理、Neo4j 数据导入、FastAPI 后端开发**，那么 Pandas 不需要学到数据分析师那种程度，但必须做到：

> 拿到 Excel/CSV → 读取 → 筛选 → 修改 → 导出

这一套操作要熟练。

下面是我整理的 **Pandas 必会基础知识（开发版）**。

---

# Pandas 必会基础知识

## 1. 安装

```bash
pip install pandas
```

导入：

```python
import pandas as pd
```

约定俗成：

```python
import pandas as pd
```

几乎所有项目都这么写。

---

# 2. DataFrame 是什么

Pandas 最核心的数据结构：

```python
import pandas as pd

df = pd.DataFrame({
    "姓名": ["张三", "李四"],
    "年龄": [20, 25]
})

print(df)
```

输出：

```text
   姓名  年龄
0 张三  20
1 李四  25
```

你可以理解为：

```text
Excel表
数据库表
CSV文件
```

本质都是 DataFrame。

---

# 3. 读取 CSV

最常用。

假设：

```csv
name,age
Tom,20
Jack,25
Lucy,18
```

读取：

```python
df = pd.read_csv("user.csv")
```

查看：

```python
print(df)
```

---

# 4. 读取 Excel

安装：

```bash
pip install openpyxl
```

读取：

```python
df = pd.read_excel("user.xlsx")
```

---

# 5. 查看前几行

```python
df.head()
```

默认：

```python
df.head(5)
```

查看前5行。

---

查看后10行：

```python
df.tail(10)
```

---

# 6. 查看列名

```python
print(df.columns)
```

结果：

```python
Index(['name', 'age'])
```

转换列表：

```python
list(df.columns)
```

结果：

```python
['name', 'age']
```

---

# 7. 查看基本信息

```python
df.info()
```

输出：

```text
列名
数据类型
空值情况
```

非常重要。

---

# 8. 查看统计信息

```python
df.describe()
```

输出：

```text
平均值
最大值
最小值
标准差
```

---

# 9. 获取一列

## 方法1

```python
df["name"]
```

结果：

```python
0     Tom
1    Jack
2    Lucy
```

---

## 方法2

```python
df.name
```

推荐：

```python
df["name"]
```

更稳定。

---

# 10. 获取多列

```python
df[["name", "age"]]
```

结果：

```text
name age
Tom 20
Jack 25
```

---

# 11. 获取一行

## 按索引

```python
df.loc[0]
```

结果：

```text
name    Tom
age      20
```

---

## 获取多行

```python
df.loc[0:2]
```

---

# 12. iloc

按位置获取。

```python
df.iloc[0]
```

第一行。

---

```python
df.iloc[0, 1]
```

第一行第二列。

---

例如：

```python
print(df.iloc[0,1])
```

输出：

```text
20
```

---

# 13. 条件筛选

这是最重要的操作之一。

---

年龄大于20：

```python
df[df["age"] > 20]
```

结果：

```text
Jack 25
```

---

多个条件：

```python
df[
    (df["age"] > 18)
    &
    (df["age"] < 30)
]
```

注意：

```python
&
|
```

不是：

```python
and
or
```

---

# 14. 新增列

```python
df["salary"] = 10000
```

结果：

```text
name age salary
Tom 20 10000
Jack 25 10000
```

---

根据已有列计算：

```python
df["age2"] = df["age"] + 10
```

结果：

```text
20 → 30
25 → 35
```

---

# 15. 修改数据

修改某行：

```python
df.loc[0, "age"] = 30
```

结果：

```text
Tom 30
```

---

# 16. 删除列

```python
df.drop(columns=["salary"])
```

---

永久删除：

```python
df = df.drop(columns=["salary"])
```

---

# 17. 删除行

删除索引0：

```python
df = df.drop(index=0)
```

---

# 18. 排序

年龄升序：

```python
df.sort_values("age")
```

---

年龄降序：

```python
df.sort_values(
    "age",
    ascending=False
)
```

---

# 19. 去重

```python
df.drop_duplicates()
```

---

按某列去重：

```python
df.drop_duplicates(
    subset=["name"]
)
```

---

# 20. 缺失值处理

查看：

```python
df.isnull()
```

---

统计：

```python
df.isnull().sum()
```

---

填充：

```python
df.fillna(0)
```

---

删除空值：

```python
df.dropna()
```

---

# 21. 遍历数据

开发里非常常见。

---

遍历行：

```python
for index, row in df.iterrows():
    print(row["name"])
```

例如：

```python
for index, row in df.iterrows():
    print(row["name"], row["age"])
```

输出：

```text
Tom 20
Jack 25
Lucy 18
```

---

# 22. 转换成字典

非常重要。

FastAPI接口经常这样返回。

```python
records = df.to_dict(
    orient="records"
)
```

结果：

```python
[
    {
        "name":"Tom",
        "age":20
    },
    {
        "name":"Jack",
        "age":25
    }
]
```

---

# 23. 保存 CSV

```python
df.to_csv(
    "result.csv",
    index=False
)
```

---

# 24. 保存 Excel

```python
df.to_excel(
    "result.xlsx",
    index=False
)
```

---

# 25. AI开发必须掌握的操作

假设：

```csv
name,age
Tom,20
Jack,25
Lucy,18
```

读取：

```python
df = pd.read_csv("user.csv")
```

筛选：

```python
adult = df[df["age"] >= 18]
```

转换：

```python
data = adult.to_dict(
    orient="records"
)
```

结果：

```python
[
    {"name":"Tom","age":20},
    {"name":"Jack","age":25},
    {"name":"Lucy","age":18}
]
```

返回 FastAPI：

```python
return data
```

这就是很多实际项目里的完整流程。

---

# Pandas 基础阶段掌握目标

必须会：

✅ `read_csv()`

✅ `read_excel()`

✅ `head()`

✅ `info()`

✅ `columns`

✅ `loc`

✅ `iloc`

✅ 条件筛选

✅ 新增列

✅ 修改数据

✅ 删除行列

✅ 排序

✅ 去重

✅ 缺失值处理

✅ `iterrows()`

✅ `to_dict()`

✅ `to_csv()`

✅ `to_excel()`

如果这些都会了，基本已经能处理：

* Excel 数据清洗
* Neo4j 导入数据预处理
* FastAPI 接口数据处理
* Dify 插件中的 CSV/Excel 文件处理

下一步建议学习 **NumPy（开发常用部分）** 或直接进入 **FastAPI 核心知识复习**，因为对于 AI 应用开发来说，FastAPI 的优先级通常比深入学习 NumPy 更高。
