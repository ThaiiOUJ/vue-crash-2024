# vue-crash-2024

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

# Vue项目

## 更改locahost:8000

1. 找到`vite.config.js`文件,在`plugins`后面加入以下代码

   ```vue
     server:{
       port:8000, //这里可以更改为想要的地址比如5173等等
     },
   ```

## 组合api

组合式api需要导入一个`ref`才行不然会点击没有反应

`import {ref} from 'vue';`

```vue
<script>

import {ref} from 'vue';

export default{
  setup(){
    const name = ref('John Doe');
    const status = ref('active');
    const tasks = ref(['Task One', 'Task Two', 'Task Three']);


    const toggleStatus = () => {
      if (this.status === 'active'){
        this.status = 'pending';
      }else if (this.status === 'pending'){
        this.status = 'inactive';
      }else {
        this.status = 'active';
      }
    };

    return{
      name,
      status,
      tasks,
      toggleStatus,
    };
  }
}

</script>

<template>
<h1>{{ name }}</h1>
<p v-if="status === 'active'">用户处于活动状态</p>
<p v-else-if="status==='pending'">用户正在等待</p>
<p v-else>用户处于非活动状态</p>
<h3>tasks:</h3>
<ul>
  <li v-for="task in tasks" :key="task">{{ task }}</li>
</ul>
<br>
<button @click="toggleStatus">改变状态</button>
</template>



```

> 然后用ref后下面的逻辑不使用this.xxxx,直接使用值+value

如下:

```vue
<script>

import {ref} from 'vue';

export default{
  setup(){
    const name = ref('John Doe');
    const status = ref('active');
    const tasks = ref(['Task One', 'Task Two', 'Task Three']);


    const toggleStatus = () => {
      if (status.value === 'active'){
          //这里不在用this.status改为值+value
        status.value = 'pending';
      }else if (status.value === 'pending'){
        status.value = 'inactive';
      }else {
        status.value = 'active';
      }
    };

    return{
      name,
      status,
      tasks,
      toggleStatus,
    };
  }
}

</script>

<template>
<h1>{{ name }}</h1>
<p v-if="status === 'active'">用户处于活动状态</p>
<p v-else-if="status==='pending'">用户正在等待</p>
<p v-else>用户处于非活动状态</p>
<h3>tasks:</h3>
<ul>
  <li v-for="task in tasks" :key="task">{{ task }}</li>
</ul>
<br>
<button @click="toggleStatus">改变状态</button>
</template>
```

### 组合缩减写法

> 缩减写法直接定义在sctipt里
>
> ```vue
> <script setup>
> 
> </script>
> 
> 这个写法连下面的内容都不需要
> export default{
> setup(){
> 
> }
> }
> ```

### 正确如下

```vue
<script setup>
import {ref} from 'vue';

    const name = ref('John Doe');
    const status = ref('active');
    const tasks = ref(['Task One', 'Task Two', 'Task Three']);
    const newTask = ref('');


    const toggleStatus = () => {
      if (status.value === 'active'){
        status.value = 'pending';
      }else if (status.value === 'pending'){
        status.value = 'inactive';
      }else {
        status.value = 'active';
      }
    };

    // 添加新任务
    // push 方法用于向数组的末尾添加一个或多个元素，并返回新数组的长度
    const addTask = () =>{
      if(newTask.value.trim() !== ''){
        tasks.value.push(newTask.value);
        newTask.value = '';
      }
    };
    
    // 删除任务
    // splice 方法用于从数组中添加或删除元素。它可以修改数组的内容
    const deleteTask = (index) =>{
      tasks.value.splice(index,1);
    } ;
</script>

<template>
<h1>{{ name }}</h1>
<p v-if="status === 'active'">用户处于活动状态</p>
<p v-else-if="status==='pending'">用户正在等待</p>
<p v-else>用户处于非活动状态</p>

<form @submit.prevent="addTask">
  <label for="newTask">Add Task</label>
  <input type="text" name="newTesk" id="newTask" v-model="newTask">
  <button type="submit">提交</button>
</form>

<h3>tasks:</h3>
<ul>
  <li v-for="(task,index) in tasks" :key="task">
    <span>
      {{ task }}
    </span>
    <button @click="deleteTask">删除</button>
  </li>
</ul>
<br>
<button @click="toggleStatus(index)">改变状态</button>
</template>
```

### 代码解释

> **`tasks.value.splice(index, 1);`**：
>
> - `tasks` 是一个响应式数据，通常在 Vue 中使用（比如 `ref` 或 `reactive`）。
>
> - `tasks.value` 是访问其内部值的方式（假设 `tasks` 是一个响应式引用）。
>
> - ```bash
>   splice
>   ```
>
>    方法在这里用于从 
>
>   ```bash
>   tasks
>   ```
>
>    数组中删除元素：
>
>   - 第一个参数 `index` 指定要删除的元素的位置。
>   - 第二个参数 `1` 表示要删除的元素数量，这里是删除 1 个元素。

### 数组添加方法

> `push` 方法用于向数组的末尾添加一个或多个元素，并返回新数组的长度
>
> `splice` 方法用于从数组中添加或删除元素。它可以修改数组的内容。
>
> `shift`从数组开头移除第一个元素，并返回该元素。
> `unshift`向数组开头添加一个或多个元素，并返回新数组的长度。
>
> `slice`返回一个新的数组，包含从原数组中提取的部分元素，不改变原数组。
>
> `concat`合并两个或多个数组，并返回一个新数组。
>
> `forEach`对数组的每个元素执行一个提供函数,不返回任何结果
>
> `map`创建一个新数组,其中包含调用提供的函数处理每个元素的后结果
>
> `filter`创建一个新的数组,包含所有通过提供的测试的元素
>
> `pop`从数组末尾移除最后一个元素并返回该元素

## **选项式 API 与组合式 API 的对比**

| **特性**         | **选项式 API**                       | **组合式 API（Composition API）** |
| ---------------- | ------------------------------------ | --------------------------------- |
| **逻辑分布**     | 数据、方法、计算属性分散在不同选项里 | 数据和逻辑集中在 `setup` 函数里   |
| **访问方式**     | 使用 `this` 访问数据和方法           | 直接通过变量访问                  |
| **学习曲线**     | 更容易上手，适合初学者               | 灵活性更强，但需要熟悉新的语法    |
| **代码重用**     | 重用逻辑较困难                       | 可以通过自定义 hooks 重用逻辑     |
| **大型项目维护** | 逻辑可能分散，不易维护               | 逻辑集中，适合大型项目            |

## 生命周期

> 生命周期函数 
>
> - <span style="color:#FF3399;"> 创建期: `beforeCreate` `created` </span>
> - <span style="color:#FF3399;"> 挂载期: `beforeMounte` `mounted` </span>
> - <span style="color:#FF3399;"> 更新期: `beforeUpdate`   `updated` </span>
> - <span style="color:#FF3399;"> 销毁期: `beforeUnmount`  ` unmounted `</span>

### 配合异步

```vue
<script setup>
import {ref,onMounted} from 'vue';

    const name = ref('John Doe');
    const status = ref('active');
    const tasks = ref(['Task One', 'Task Two', 'Task Three']);
    const newTask = ref('');


    const toggleStatus = () => {
      if (status.value === 'active'){
        status.value = 'pending';
      }else if (status.value === 'pending'){
        status.value = 'inactive';
      }else {
        status.value = 'active';
      }
    };

    // 添加新任务
    // push 方法用于向数组的末尾添加一个或多个元素，并返回新数组的长度
    const addTask = () =>{
      if(newTask.value.trim() !== ''){
        tasks.value.push(newTask.value);
        newTask.value = '';
      }
    };
    
    // 删除任务
    // splice 方法用于从数组中添加或删除元素。它可以修改数组的内容
    const deleteTask = (index) =>{
      tasks.value.splice(index,1 );
    } ;

    // 生命周期
    onMounted(async () =>{
      try{
        const response = await fetch('https://jsonplaceholder.typicode.com/todos');
        // // await fetch(...) 会暂停代码执行，直到 fetch 完成并返回结果
        const data = await response.json();
        tasks.value = data.map(task => task.title);
      }catch(error){
        console.log('Error fetching tasks');
      }
    })
</script>

<template>
<h1>{{ name }}</h1>
<p v-if="status === 'active'">用户处于活动状态</p>
<p v-else-if="status==='pending'">用户正在等待</p>
<p v-else>用户处于非活动状态</p>

<form @submit.prevent="addTask">
  <label for="newTask">Add Task</label>
  <input type="text" name="newTesk" id="newTask" v-model="newTask">
  <button type="submit">提交</button>
</form>

<h3>tasks:</h3>
<ul>
  <li v-for="(task,index) in tasks" :key="task">
    <span>
      {{ task }}
    </span>
    <button @click="deleteTask">删除</button>
  </li>
</ul>
<br>
<button @click="toggleStatus(index)">改变状态</button>
</template>
```



```bash
    // 生命周期
    onMounted(async () =>{
      try{
        const response = await fetch('https://invalid-url.example.com/todos');
        // await fetch(...) 会暂停代码执行，直到 fetch 完成并返回结果
        const data = await response.json();
        tasks.value = data.map(task => task.title);
      }catch(error){
        console.log('Error fetching tasks');
      }
    })
```

> 异步函数,搭配,`async`,`await`
>
> **`onMounted(async () => { ... })`**：
>
> - 这段代码是一个生命周期钩子，意思是在组件加载到页面上后执行的内容。`async` 表示这个函数里面会有异步操作。
>
> 
>
> **`try { ... }`**：
>
> - 这里用 `try` 来包裹代码块，意思是尝试执行里面的代码。如果出现错误，不会让程序崩溃，而是会转到 `catch` 处理错误。
>
> 
>
> **`const response = await fetch('https://jsonplaceholder.typicode.com/todos');`**：
>
> - 这行代码的意思是去网络上请求一份任务列表（数据）。`fetch` 函数负责这个请求，`await` 让代码在这里暂停，等请求完成后再继续执行下一步。
>
> 
>
> **`const data = await response.json();`**：
>
> - 当请求成功后，得到一个响应（`response`）。这行代码将这个响应转换成 JSON 格式的数据。`await` 也是在这里用来等待这个转换完成。
>
> 
>
> **`tasks.value = data.map(task => task.title);`**：
>
> - 这行代码的意思是，从获取的数据中提取每个任务的标题，并把它们存入 `tasks` 数组。`data.map(task => task.title)` 是一个简单的循环，用来获取所有任务的标题。
>
> 
>
> **`catch (error) { console.log('Error fetching tasks'); }`**：
>
> - 如果在 `try` 块中的任意一步出现了错误，比如网络问题，程序就会跳转到这里，打印出错误信息 `'Error fetching tasks'`，告诉你获取任务的时候出错了。

### 总结

该代码使用了生命周期钩子,`onMounted`并在后面使用了异步处理`async`,`await`

如果`await fetch('............')`这个括号里识别的url有错误直接就跳转到`try`里的`catch (error){.......}`报错里

如图:

![image-20241101114418303](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241101114418303.png)

url没有问题的话就会赋值给`response`去响应,后进行转换后赋值给`data`, 源码:`const data = await response.json();`

`data`有数据后使用 `map` 方法从 `data` 数组中提取每个任务对象的 `title` 属性，并生成一个新的数组，包含所有任务的标题。这个新的数组被赋值给 `tasks.value`，从而更新 Vue 组件中 `tasks` 的值。源码:`tasks.value = data.map(task => task.title);`

## Tailwind CSS配置

```css
/** @type {import('tailwindcss').Config} */
export default {
  content: [ './index.html','./src/**/*.{vue,js,ts,jsx,tsx}'],
  theme: {
    extend: {
      fontFamily:{
        sans:['Poppins','sans-serif'],
      },
      gridTemplateColumns:{
        '70/30':'70% 28%',
      }
    },
  },
  variants:{
    extends:{}
  },
  plugins: [],
}
```

> **content**:
>
> ```css
> content: [ './index.html','./src/**/*.{vue,js,ts,jsx,tsx}']
> ```
>
> - 这里定义了可以使用 Tailwind CSS 的文件范围。指定了 `index.html` 和 `src` 文件夹下所有 `.vue`、`.js`、`.ts`、`.jsx`、`.tsx` 文件。这意味着 Tailwind CSS 只会扫描并生成在这些文件中用到的样式类，从而减少生成的 CSS 文件大小。
>
> **extend**:
>
> - ```css
>   extend: { ... }
>   ```
>
>    用于在默认主题的基础上添加或覆盖自定义配置。例如：
>
>   - fontFamily:
>
>     ```css
>     fontFamily: { sans: ['Poppins', 'sans-serif'] }
>     ```
>
>     - 在 `extend` 下添加了字体配置。它优先使用 `Poppins` 字体，如果该字体不可用，则使用浏览器默认的无衬线字体（`sans-serif`）。
>
>   - gridTemplateColumns:
>
>     ```css
>     gridTemplateColumns: { '70/30': '70% 30%' }
>     ```
>
>     - 定义了新的网格模板列布局，名称为 `70/30`。它表示创建两列，第一列占 70%，第二列占 30%。这里你写的是 `70% 28%`，可能是笔误，应该是 `70% 30%`。
>
> **variants**:
>
> ```css
> variants: { extend: {} }
> ```
>
> - `variants` 用于定义 Tailwind 的变体配置。变体控制某些状态下的样式（如 `hover`、`focus`、`active` 等）。这里使用空的 `extend` 对象，表示没有额外的变体配置，只使用默认变体。
>
> **plugins**:
>
> ```css
> plugins: []
> ```
>
> - `plugins` 用于添加 Tailwind CSS 插件，扩展功能或增加额外的样式。在这里没有添加任何插件，表示仅使用 Tailwind CSS 的默认功能。

## 插槽实用

> 有一个卡片样式版,里面是一些文本内容,但是卡片样式版有一个固定的版如果写在html里会重复写
>
> 下面使用插槽写法可以是代码简洁,并可多个地方用到这个卡片样式
>
> 下面代码我们可以看到重复了一些样式`class="bg-gray-100 p-6 rounded-lg shadow-md"`如果每次都这么干会很麻烦
>
> ```vue
> <div class="bg-gray-100 p-6 rounded-lg shadow-md">
>     <h2 class="text-2xl font-bold">For Developers</h2>
>     <p class="mt-2 mb-4">
>         Browse our Vue jobs and start your career today
>     </p>
>     <a
>        href="jobs.html"
>        class="inline-block bg-black text-white rounded-lg px-4 py-2 hover:bg-gray-700"
>        >
>         Browse Jobs
>     </a>
> </div>
> <div class="bg-green-100 p-6 rounded-lg shadow-md">
>     <h2 class="text-2xl font-bold">For Employers</h2>
>     <p class="mt-2 mb-4">
>         List your job to find the perfect developer for the role
>     </p>
>     <a
>        href="add-job.html"
>        class="inline-block bg-green-500 text-white rounded-lg px-4 py-2 hover:bg-green-600"
>        >
>         Add Job
>     </a>
> </div>
> ```
>
> 优化后:
>
> ```vue
> <!--新建一个名为卡的组件Card.vue-->
> <div class="bg-gray-100 p-6 rounded-lg shadow-md">
>     <slot>
>     </slot>
> </div>
> ```
>
> 通过在 `Card.vue` 组件中定义样式并使用插槽（`<slot>`），你可以在任何需要样式一致的地方复用 `Card` 组件，而不用每次都重复写相同的样式代码。
>
> ```vue
> <Card>
>     <h2 class="text-2xl font-bold">For Developers</h2>
>     <p class="mt-2 mb-4">
>         Browse our Vue jobs and start your career today
>     </p>
>     <a
>        href="jobs.html"
>        class="inline-block bg-black text-white rounded-lg px-4 py-2 hover:bg-gray-700"
>        >
>         Browse Jobs
>     </a>
> </Card>
> ```

## 组件传参

> 上面是插槽,但是这样定义会让全部卡片都是为背景灰色,如果我们需要一个一样的样式但是背景颜色要不一样的,这样需求需要组件传参来进行实现如下:
>
> 使用`defineProps`来实现传参
>
> ```vue
> <template>
>      <div :class="`${bg} p-6 rounded-lg shadow-md`">
>          <!--这里需要给class加一个动态的,然后使用${bg}动态绑定这个背景颜色-->
>         <slot>
>         </slot>
>      </div>
> </template>
> 
> <script setup>
> import {defineProps} from'vue';
> 
> defineProps({
>     // 用bg来充当背景缩写
>     bg:{
>         type:String,
>         default:'bg-gray-100'
>         //使用default来设置默认卡片背景颜色为bg-gray-100,灰色
>     }
> })
> </script>
> ```
>
> 定义好卡片样式组件后，目前默认的背景颜色是灰色的。如果我们需要更改卡片的背景颜色，可以通过向组件传递 `bg` 参数来实现。
>
> ```css
> <Card bg = "bg-green-100">
> 这时卡片就会变成绿色会跟别的卡片不一样背景颜色
> </Card>
> ```
>
> ![image-20241101151921403](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241101151921403.png)

