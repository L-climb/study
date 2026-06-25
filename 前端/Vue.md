如果你是**完全没有前端基础**，我建议学习顺序不要直接冲 Vue，而是：

```text
HTML
↓
CSS
↓
JavaScript
↓
Vue
↓
Vue生态（Pinia、Vue Router）
↓
Next.js / React（如果以后做AI应用）
```

因为 Vue 本质上是在 HTML、CSS、JavaScript 之上的框架。如果 JS 不会，Vue 很难真正理解。

---

# 第一阶段：前端到底是什么？

你可以把一个网页理解成：

```text
HTML = 骨架
CSS = 外观
JavaScript = 动作
```

例如一个登录页面：

```text
+----------------+
| 用户名         |
| [___________]  |
|                |
| 密码           |
| [___________]  |
|                |
| [登录按钮]     |
+----------------+
```

---

## HTML：负责结构

```html
<h1>欢迎登录</h1>

<input>

<button>登录</button>
```

效果：

```text
欢迎登录

[输入框]

[登录按钮]
```

常见标签：

| 标签     | 作用  |
| ------ | --- |
| h1     | 标题  |
| p      | 段落  |
| div    | 容器  |
| input  | 输入框 |
| button | 按钮  |
| img    | 图片  |
| table  | 表格  |

例如：

```html
<div>
    <h1>个人信息</h1>
    <p>姓名：张三</p>
</div>
```

---

## CSS：负责样式

HTML 默认很丑。

```html
<button>登录</button>
```

CSS：

```css
button {
    background: blue;
    color: white;
    width: 100px;
}
```

效果：

```text
蓝色按钮
```

---

## JavaScript：负责交互

例如点击按钮：

```html
<button onclick="hello()">
    点我
</button>

<script>
function hello() {
    alert("你好")
}
</script>
```

点击后：

```text
弹窗：
你好
```

---

# 第二阶段：Vue 是什么？

Vue 的作用：

```text
HTML
+
CSS
+
JavaScript
+
组件化
+
数据绑定
=
Vue
```

Vue 最大特点：

```text
数据变化
↓
页面自动更新
```

不需要自己操作 DOM。

---

# 第一个 Vue 页面

Vue 单文件组件：

```vue
<template>
  <h1>{{ title }}</h1>
</template>

<script setup>
const title = "Hello Vue"
</script>
```

显示：

```text
Hello Vue
```

---

# Vue 三大部分

## template

页面内容

```vue
<template>
  <h1>Hello</h1>
</template>
```

相当于：

```html
<h1>Hello</h1>
```

---

## script

写逻辑

```vue
<script setup>
const name = "张三"
</script>
```

---

## style

写样式

```vue
<style>
h1 {
  color: red;
}
</style>
```

---

完整：

```vue
<template>
  <h1>{{ name }}</h1>
</template>

<script setup>
const name = "张三"
</script>

<style>
h1 {
  color: red;
}
</style>
```

---

# Vue 最核心的概念：数据绑定

## 普通 HTML

```html
<h1>张三</h1>
```

改名字：

```html
<h1>李四</h1>
```

必须修改页面。

---

## Vue

```vue
<template>
  <h1>{{ name }}</h1>
</template>

<script setup>
const name = "张三"
</script>
```

页面显示：

```text
张三
```

修改：

```vue
const name = "李四"
```

页面自动变：

```text
李四
```

---

# Vue变量显示

使用：

```vue
{{ }}
```

例如：

```vue
<script setup>
const age = 20
</script>

<template>
  <p>{{ age }}</p>
</template>
```

结果：

```text
20
```

---

# 响应式数据（重点）

Vue 最重要的知识之一：

```vue
import { ref } from 'vue'

const count = ref(0)
```

这里：

```vue
count.value
```

是真实值。

---

示例：

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function add() {
  count.value++
}
</script>

<template>
  <h1>{{ count }}</h1>

  <button @click="add">
    +1
  </button>
</template>
```

点击：

```text
0
↓
1
↓
2
↓
3
```

页面自动更新。

---

# 事件绑定

## 点击事件

```vue
<button @click="hello">
    点我
</button>
```

```vue
function hello() {
    alert("你好")
}
```

---

## 输入事件

```vue
<input @input="change">
```

---

# 双向绑定（非常重要）

Vue 最经典功能：

```vue
v-model
```

例如：

```vue
<script setup>
import { ref } from 'vue'

const name = ref('')
</script>

<template>
  <input v-model="name">

  <p>{{ name }}</p>
</template>
```

效果：

```text
输入：张三

下面实时显示：

张三
```

---

# 条件渲染

## v-if

```vue
<p v-if="isLogin">
    欢迎回来
</p>
```

```vue
const isLogin = true
```

显示：

```text
欢迎回来
```

如果：

```vue
const isLogin = false
```

则不显示。

---

# 列表渲染

## v-for

```vue
<script setup>
const users = [
  '张三',
  '李四',
  '王五'
]
</script>

<template>
  <li v-for="user in users">
    {{ user }}
  </li>
</template>
```

结果：

```text
张三
李四
王五
```

---

# Vue组件（最重要）

组件就是可复用模块。

例如：

```text
登录框
聊天框
导航栏
用户卡片
```

都可以写成组件。

---

创建：

`UserCard.vue`

```vue
<template>
  <h2>用户信息</h2>
</template>
```

使用：

```vue
<script setup>
import UserCard from './UserCard.vue'
</script>

<template>
  <UserCard />
</template>
```

---

# Vue 项目目录

以后你看到的 Vue 项目大概长这样：

```text
src
├── assets
├── components
│   └── UserCard.vue
├── views
│   └── Home.vue
├── router
├── stores
├── App.vue
└── main.js
```

其中：

* `components`：组件
* `views`：页面
* `router`：路由
* `stores`：状态管理（Pinia）

---

# 学完这些后，你就能做什么？

已经能写：

* 登录页面
* 注册页面
* 用户管理页面
* 表格展示
* CRUD增删改查
* 调用 FastAPI 接口

例如你的技术栈：

```text
Vue
↓
Axios
↓
FastAPI
↓
Neo4j
```

完全可以做一个：

```text
人物关系风险分析系统
```

前端展示关系图，后端用 FastAPI 调 Neo4j 查询数据。

---

下一步建议学 Vue 的这几个核心内容：

1. `ref()` 和 `reactive()`
2. `computed()`（计算属性）
3. `watch()`（监听）
4. 组件通信（props、emit）
5. Vue Router（页面跳转）
6. Pinia（全局状态管理）
7. Axios（调用后端接口）

这几个学会后，基本就具备独立开发 Vue 管理系统的能力了。
