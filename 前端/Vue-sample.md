很好，既然你是**前端零基础**，那不要一上来就学复杂的组件通信、Pinia这些。

先把 Vue 当成一个：

```text
HTML + JavaScript 的增强版
```

来学习。

下面我给你几个最基础的 Vue 3 示例，每个都带详细注释。

---

# 1. Hello Vue（第一个Vue程序）

```vue
<template>
  <!-- 页面显示内容 -->
  <h1>{{ message }}</h1>
</template>

<script setup>
// 定义一个变量
const message = "Hello Vue"
</script>
```

运行效果：

```text
Hello Vue
```

---

## 代码解释

### template

```vue
<template>
</template>
```

这里写页面内容。

类似HTML：

```html
<h1>Hello Vue</h1>
```

---

### script setup

```vue
<script setup>
</script>
```

这里写JavaScript逻辑。

---

### {{ }}

```vue
{{ message }}
```

表示：

```text
把message变量显示出来
```

---

# 2. 显示多个变量

```vue
<template>
  <h2>姓名：{{ name }}</h2>
  <h2>年龄：{{ age }}</h2>
</template>

<script setup>
const name = "张三"

const age = 18
</script>
```

效果：

```text
姓名：张三

年龄：18
```

---

# 3. 点击按钮

这是Vue最经典的例子。

```vue
<template>
  <button @click="hello">
    点我
  </button>
</template>

<script setup>

// 点击按钮执行的函数
function hello() {
  alert("你好")
}

</script>
```

---

## @click是什么意思？

```vue
@click="hello"
```

等于：

```html
onclick="hello()"
```

意思：

```text
点击按钮
↓
执行hello函数
```

---

# 4. 计数器（Vue最经典案例）

```vue
<template>

  <h1>{{ count }}</h1>

  <button @click="add">
    +1
  </button>

</template>

<script setup>

import { ref } from 'vue'

// 响应式变量
const count = ref(0)

// 点击按钮执行
function add() {
  count.value++
}

</script>
```

---

## 运行过程

初始：

```text
0
```

点击：

```text
1
```

再点击：

```text
2
```

再点击：

```text
3
```

---

## 为什么用 ref？

普通变量：

```js
let count = 0
```

Vue不知道它变了。

---

Vue变量：

```js
const count = ref(0)
```

Vue会监听变化。

页面自动更新。

---

# 5. 输入框获取内容

```vue
<template>

  <input v-model="name">

  <p>
    你输入的是：{{ name }}
  </p>

</template>

<script setup>

import { ref } from 'vue'

const name = ref('')

</script>
```

---

效果：

输入：

```text
张三
```

页面实时显示：

```text
你输入的是：张三
```

---

# 6. 条件显示

```vue
<template>

  <p v-if="isLogin">
    欢迎回来
  </p>

</template>

<script setup>

const isLogin = true

</script>
```

显示：

```text
欢迎回来
```

---

改成：

```js
const isLogin = false
```

结果：

```text
什么都不显示
```

---

# 7. if else

```vue
<template>

  <p v-if="score >= 60">
    及格
  </p>

  <p v-else>
    不及格
  </p>

</template>

<script setup>

const score = 75

</script>
```

结果：

```text
及格
```

---

# 8. 列表循环

```vue
<template>

  <ul>

    <li v-for="item in users">

      {{ item }}

    </li>

  </ul>

</template>

<script setup>

const users = [
  "张三",
  "李四",
  "王五"
]

</script>
```

结果：

```text
张三
李四
王五
```

---

# 9. 显示对象数据

```vue
<template>

  <h2>姓名：{{ user.name }}</h2>

  <h2>年龄：{{ user.age }}</h2>

</template>

<script setup>

const user = {
  name: "张三",
  age: 18
}

</script>
```

效果：

```text
姓名：张三

年龄：18
```

---

# 10. 综合练习（推荐自己敲一遍）

用户信息卡片：

```vue
<template>

  <h1>用户信息</h1>

  <p>姓名：{{ name }}</p>

  <p>年龄：{{ age }}</p>

  <button @click="addAge">
    年龄+1
  </button>

</template>

<script setup>

import { ref } from 'vue'

// 姓名
const name = ref("张三")

// 年龄
const age = ref(18)

// 点击按钮年龄加1
function addAge() {
  age.value++
}

</script>
```

运行：

```text
用户信息

姓名：张三

年龄：18

[年龄+1]
```

点击：

```text
年龄：19
```

再点击：

```text
年龄：20
```

---

# 现阶段必须掌握的知识

如果你是0基础，先把下面5个搞懂：

```vue
{{ }}
```

数据显示

---

```vue
ref()
```

响应式变量

---

```vue
@click
```

点击事件

---

```vue
v-model
```

双向绑定

---

```vue
v-for
```

列表循环

---

这几个掌握后，你就已经能写一个简单的“用户管理页面”了。下一步我可以带你从 **创建 Vue 项目（Vue CLI / Vite）开始，手把手做一个“学生管理系统”项目**，边做边学 Vue。
