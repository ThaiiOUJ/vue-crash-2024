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

## 插入数据

有一个定义好的数据json文件

```javascript
import JobListing from './JobListing.vue';
import jobData from '@/jobs.json';
import { ref } from 'vue';

const jobs = ref(jobData);
```

> 这里的意思是 导入一个jobData,数据位置是在本Src目录下的jobs.json
>
> 第二个命令是导入`ref`是用于响应数据
>
> `const jobs = ref(jobData)`这个的意思是用`ref`把joData包装成一个响应式的数据,当数据改变时,视图会自动更新

### `template`部分

```html
<template>
    <section class="bg-blue-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
                Browse jobs
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <JobListing v-for="job in jobs" :key="job.id" :job="job"/>
            </div>
        </div>
    </section>
</template>
```

> 这里是创建了一个名为`JobListing`文件夹,通过组件方式传参,把job作为job传递给另一个组件JobListing,让子组件来显示每个方面的内容这样的优势是简洁,可以重复调用

### `JobListing`组件

```javascript
<script setup>
import {defineProps} from 'vue';

defineProps({
    job: Object,
})
</script>
```

> <script setup> 部分中的 job: Object 就是指 <JobListing v-for="job in j   obs" :key="job.id" :job="job"/> 中的 :job="job"
>
> **在父组件中，通过 `:job="job"` 把每个 `job` 对象传递给 `JobListing` 组件。**
>
> **在 `JobListing` 组件的 `<script setup>` 部分，用 `defineProps` 定义了 `job` 这个 `props`，类型是 `Object`，用来接收从父组件传入的 `job` 对象。**
>
> **这样，`JobListing` 组件就能访问到 `job` 对象的内容，例如 `job.type`、`job.title`、`job.salary` 和 `job.location`，并在模板中显示这些信息。**

### 定义界面

> 定义界面只显示三个内容,避免一次性显示,简洁页面

`JobListing`父级组件

```vue
<!-- 父级 -->
<script setup>
import JobListing from './JobListing.vue';
import jobData from '@/jobs.json';
import {ref ,defineProps} from 'vue';

// 现在页面显示
defineProps({
    // 用于接收为数字类型
    limit:Number,
})
const jobs = ref(jobData);

</script>

<template>
    <section class="bg-blue-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
                Browse jobs
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <JobListing v-for="job in jobs.slice(0, limit || jobs.length)" :key="job.id" :job="job"/>
            </div>
        </div>
    </section>
</template> 
```

`<JobListing v-for="job in jobs.slice(0, limit || jobs.length)" :key="job.id" :job="job"/>`

> <span style="color:#6600CC;">**`slice(0, limit || jobs.length)`**：</span>
>
> - `slice` 是 JavaScript 中的一个数组方法，用于提取数组中的一部分元素，并返回一个新数组，不改变原始数组。
> - `slice(0, limit)` 表示从 `jobs` 数组的第一个元素（索引 `0`）开始，提取 `limit` 个元素。
> - 如果 `limit` 未定义或为 `0`，则 `limit || jobs.length` 会取 `jobs.length` 的值，相当于提取整个数组。
>
> `limit`单独定义,
>
> `defineProps({
>     // 用于接收为数字类型
>     limit:Number,
> })`
>
> `limit` 是一个 `prop`，由父组件传入，类型是 `Number`。
>
> `limit` 控制在页面上显示多少个职位信息。例如，当在App件中使用 `<JobListings :limit="3"/>` 时，就会显示前 3 个职位信息。
>
> `:limit`为动态绑定v-bind
>
> `slice`是js里的`const numbers = [1, 2, 3, 4, 5];`,提取`numbers.slice(1, 3); // 返回 [2, 3]`

### 缩减显示

**导入依赖**：

```javascript
import { defineProps, ref, computed } from 'vue';
```

- `defineProps`: 用于定义组件接收的属性。
- `ref`: 用于创建响应式变量。
- `computed`: 用于定义计算属性。

**定义属性**：

```javascript
 const props = defineProps({
    job: Object,
});
```

- `job`: 一个对象，包含职位的相关信息（如描述、类型、标题等）。

**定义响应式变量**：

```javascript
const showFullDescription = ref(false);
```

- `showFullDescription` 初始为 `false`，表示默认不显示完整描述。

**切换描述的函数**：

```javascript
 const toggleFullDescription = () => {
    showFullDescription.value = !showFullDescription.value;
};
```

- 这个函数用于切换 `showFullDescription` 的布尔值，实现描述的展开和收起。

**计算属性**：

```javascript
 const truncatedDescription = computed(() => {
    let description = props.job.description;
    if (!showFullDescription.value) {
        description = description.substring(0, 90) + '...';
    }
    return description;
});
```

- `truncatedDescription` 用于根据 `showFullDescription` 的值返回描述。
- 如果 `showFullDescription` 为 `false`，则截取描述的前 90 个字符，并加上省略号；否则返回完整描述。

## 导入图标库PRIMEVUE

> 使用命令在运行文件中下载
>
> ```css
> cnpm install primeicons
> 
> <!---在项目文件内找到main.js,粘贴下面代码-->
> 
> import 'primeicons/primeicons.css'
> ```

### 添加图标例子

`pi pi-map-marker` 是 PrimeIcons 的类名，用于显示定位（地图标记）图标。

`text-orange-500` 是 Tailwind CSS 的类名，用于将图标颜色设置为橙色。

```css
 <i class="pi pi-map-marker text-orange-500"></i>
```

## 添加路由

由于刚开始创建项目时是有提供创建路由的,但是我们拒绝了,现在我们要重新创建路由

`cnpm i vue-router`  这个的命令是创建vue路由, i是代表install缩写,cnpm是国内镜像下载,下面图片代表成功

![image-20241103144900421](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241103144900421.png)

由于我们拒绝了现在要在文件中创建一个新的文件`router`用来存放路由配置,这个文件夹下有一个`index.js`文件

### `index.js`文件内

在这个文件内我们需要导入路由和路由网络配置

```js
import { createRouter, createWebHistory } from "vue-router";
```

### `views`文件

> 在src下新建立一个名为views文件,用于存放视图

### 迁移视图显示

`HomeViews`配置

> 之前没有路由时是把视图文件直接显示在app.vue里面的,现在需要除固定格式外的视图迁移到views文件里
>
> ![image-20241103150846718](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241103150846718.png)
>
> 把圈起来的页面文件重新写在views文件下的HomeViews.vue里

`index.js`配置

```js
// 用于添加路由
import { createRouter, createWebHistory } from "vue-router";
// 引入页面组件
import HomeView from "@/views/HomeView.vue";
//定义路由规则
const router = createRouter({
    history:createWebHistory(import.meta.env.BASE_URL),
    routes:[
        {
            path:'/',
            name:'home',
            component:HomeView
        },
    ],
});

export default router;
```

1. `createWebHistory` 是干嘛的？
   - `createWebHistory` 是 Vue Router 里的一个东西，用来控制 URL 显示方式的。它让你的地址栏里不会出现 `#` 符号。
   - 比如说:
     - 使用 `createWebHistory` 后：`http://localhost:8080/home`
     - 如果用 `createWebHashHistory`，会有个 `#`：`http://localhost:8080/#/home`
2. `import.meta.env.BASE_URL` 是啥？
   - 这个 `import.meta.env.BASE_URL` 是 Vite 给的一个**环境变量**，它会告诉你的项目：项目应该从哪个“基础路径”开始运行。
   - 举例子说:
     - **开发时**，你本地跑代码，通常地址是根路径 `/`，也就是 `http://localhost:8080/`。
     - **上线时**，你的项目可能会放到一个子目录下，比如 `https://example.com/app/`。在这种情况下，`BASE_URL` 就会变成 `/app/`。
   - 这个 `BASE_URL` 可以让你的项目在不同的地方运行时都适配地址，不用手动改路径。
3. **总结**
   - `createWebHistory`是去掉网址链接里的#显得美观
   - `import.meta.env.BASE_URL`是动态的调整路径让项目在不同的地方正常访问

最后，通过 `export default router` 导出路由实例，使得它可以在 `main.js` 中导入并注册到 Vue 应用中。

`main.js`配置

```js
import "./assets/main.css";
import "primeicons/primeicons.css";
import router from "./router";

import { createApp } from "vue";
import App from "./App.vue";

const app = createApp(App);

app.use(router);

app.mount("#app");
```

1. `import "./assets/main.css";`和`import "primeicons/primeicons.css";`是引入全局样式文件,和图标文件

2. `import router from "./router";` 这里是路由配置文件,通过`./router`引入了`index.js`中的路由定义

3. ```js
   import { createApp } from "vue";
   import App from "./App.vue";
   ```

   这里的`createApp`是vue的核心函数,用来创建整个应用实例,相当于"启动引擎"

   `App`是根组件,也就是src下的`App.vue`文件,所有的内容和页面都是从这个文件开始,称之为入口

4. `const app = createApp(App);`这里创建了一个实例,并且把`App.vue`方进去告诉vue这个组件作为页面的起点

5. `app.use(router)`这里是给页面加上路由功能,这样就能通过不一样的路径来展示不一样的内容界面

6. `app.mount("#app");`这部是启动应用并挂载到页面上,也就是`#app` 是 `index.html` 文件里的一个 `div` 的 `id`。这里就是把 Vue 应用的内容挂载到 `#app` 这个位置上，页面内容就会显示出来了。

做完上面的页面还是显示空白,原因是因为,App.vue这个根组件还没有进行导入路由

`App.vue`配置

```vue
<!-- 1级 -->
<script setup>
import Navbar from './components/Navbar.vue';
// 把配置好的路由导入到这里
import { RouterView } from 'vue-router';
</script>


<template>
    <Navbar />
    <RouterView />
</template>

```

这样就能显示到根组件了,因为在路由暂时只有配置了根组件,所以根组件才能显示,如果访问`http://localhost:8000/test/`会发现只有一个导航栏没有别的东西,因为导航栏是直接显示在App.vue里的没有通过路由进行配置

## 添加别的页面

  主界面已经写完,接下来写的是jobs界面和add jobs界面,后面的文件全写在views文件夹内

前面我已经写了JOBS页面但是显示的太多了,然后通过了limit显示了只显示三个jobs,

`JobsView.vue`

```vue
<script setup>
import JobListings from '@/components/JobListings.vue';
</script>

<template>
    <JobListings />
</template>
```

这里直接导入了之前我们写好的`JobListings.vue`

`index.js`配置

```js
// 用于添加路由
import { createRouter, createWebHistory } from "vue-router";
// 引入页面组件
import HomeView from "@/views/HomeView.vue";
import JobsView from "@/views/JobsView.vue";
//定义路由规则
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: "/",
      name: "home",
      component: HomeView,
    },
    {
      path: "/jobs",
      name: "jobs",
      component: JobsView,
    },
  ],
});

export default router;
```

在home视图后面加了一个/jobs路由配置,`path:"/xxx"`这里必须要加`/`不然的话会进行报错

![image-20241103170628391](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241103170628391.png)

## 把链接方式更改为RouterLink

```vue
<script setup>
// 更改a标签用路由去链接
import { RouterLink } from 'vue-router';
// 导入图标
import logo from '@/assets/img/logo.png'
</script>

<RouterLink class="flex flex-shrink-0 items-center mr-4" to="/">
  <img class="h-10 w-auto" :src="logo" alt="Vue Jobs" />
  <span class="hidden md:block text-white text-2xl font-bold ml-2">Vue Jobs</span>
</RouterLink>
```

`<RouterLink to="/">...</RouterLink>`用来替代`<a href="/">...</a>`

使用`to`属性代替`href`属性,这样做直接避免刷新网页,实现无缝的路由导航

## 实现导航栏常量

> 之前的导航栏点到jobs或者home时不会一直亮,下面通过`useRoute`来实现一直亮的效果

```vue
<script setup>
    // 更改a标签用路由去链接
    import { RouterLink, useRoute } from 'vue-router';
    // 导入图标
    import logo from '@/assets/img/logo.png';

    const isActiveLink = (routPath) => {
        const route = useRoute();
        return route.path === routPath;
    }
</script>

<div class="flex space-x-2">
    <RouterLink to="/"
                :class="[isActiveLink('/') ? 'bg-green-900' : 'hover:bg-gray-900 hover:text-white', 'text-white', 'px-3', 'py-2', 'rounded-md']">
        Home
    </RouterLink>
    <RouterLink to="/jobs"
                :class="[isActiveLink('/jobs') ? 'bg-green-900' : 'hover:bg-gray-900 hover:text-white', 'text-white', 'px-3', 'py-2', 'rounded-md']">
        jobs
    </RouterLink>
    <RouterLink to="/jobs/add"
                :class="[isActiveLink('/jobs/add') ? 'bg-green-900' : 'hover:bg-gray-900 hover:text-white', 'text-white', 'px-3', 'py-2', 'rounded-md']">
        Add Job
    </RouterLink>
</div>
```

> ```js
> const isActiveLink = (routPath) => {
>     const route = useRoute();
>     return route.path === routPath;
> }
> ```
>
> **定义了一个函数 `isActiveLink`**：这个函数接受一个参数 `routPath`，这个参数代表我们想要检查的路径（也就是我们想知道的某个菜单项的路径，比如 `"/"` 或 `"/jobs"`）。
>
> **获取当前路由的路径**：在 `isActiveLink` 函数中，通过 `const route = useRoute();` 获取当前页面的路由信息。`useRoute()` 返回一个对象，其中包含当前页面的所有路由信息，比如当前的路径 `route.path`。
>
> **判断是否匹配**：函数最后通过 `return route.path === routPath;` 进行判断：
>
> - 如果当前页面路径 `route.path` 和 `routPath` 相等（匹配），就返回 `true`。
> - 如果不相等，就返回 `false`。
>
> 然后`div`部分`:class="[isActiveLink('/jobs/add') ? 'bg-green-900' : 'hover:bg-gray-900 hover:text-white', 'text-white', 'px-3', 'py-2', 'rounded-md']">`,这里的意思是给a链接设立了一个数组,如果`isActiveLink里的值对应了的话背景就变成green-900`,如果不是的话就鼠标放置按钮上显示背景为`gray-900,且文本内容为白色`

## 配置没有内容的界面显示

> 前面配置完了,现在假设有个页面没有了,但是单独显示空白又不好,所以设置一个404界面

创建一个`Not FoundView.vue`界面内容如下:

```vue
<script setup>
// 先设置路由链接
import { RouterLink } from 'vue-router';

</script>

<template>
    <section class="text-center flex flex-col justify-center items-center h-96">
        <i class="pi pi-exclamation-triangle text-yellow-500 text-7xl mb-5"></i>
        <h1 class="text-6xl font-bold mb-4">404 Not Found</h1>
        <p class="text-xl mb-5">This page does not exist</p>
        <RouterLink to="/" class="text-white bg-green-700 hover:bg-green-900 rounded-md px-3 py-2 mt-4">Go Back
        </RouterLink>
    </section>

</template>
```

> 利用路由快速跳转避免刷新页面,`to="/"`可以直接回到根组件

`index.js`配置,

```js
// 用于添加路由
import { createRouter, createWebHistory } from "vue-router";
// 引入页面组件
import HomeView from "@/views/HomeView.vue";
import JobsView from "@/views/JobsView.vue";
import NotFoundView from "@/views/NotFoundView.vue";
//定义路由规则
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: "/",
      name: "home",
      component: HomeView,
    },
    {
      path: "/jobs",
      name: "jobs",
      component: JobsView,
    },
    {
      path: "/:catchAll(.*)",
      name: "not-found",
      component: NotFoundView,
    },
  ],
});

export default router;

```

> 上面第三个路由配置为全局的配置`:catchAll(.*)`里的`catchAll`是一个动态参数,`(.*)`为正则表达式,匹配任意字符并且包含了`/`,如果我们输入了不对的路由界面,也可以理解为没写没创的一个界面,就会直接匹配到这个`NotFoundView`界面
>
> ![image-20241103200639649](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241103200639649.png)

## 使用json-server

> 为什么要写 `{"jobs":[...]}`而不是直接写`[...]`？
>
> 这就像给数据贴标签一样。"jobs" 就是一个标签，告诉 json-server："这些数据是关于工作的"
>
> 有了这个标签，json-server 就知道：
>
> - 要用什么网址来访问这些数据（比如 /jobs）
> - 怎么增加新工作
> - 怎么删除或修改工作
>
> 举个生活中的例子：
>
> 这就像你整理文件时会用文件夹一样,不会把所有文件都直接扔在桌面上,而是会建一个叫"工作"的文件夹，把相关的文件都放进去,这样以后要找东西就方便多了
>
> 所以 {"jobs":[...]} 就是在给数据分类整理，让后面使用的时候更方便。

`jobs2.json`文件

```json
{"jobs":[...]}
//这样在json数据写了之后还没有完成
```

`package.json`文件

需要在json文件里找到下面的方块,添加一个
`"server": "json-server --watch src/jobs.json --port 5000"`

```js
 "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "server": "json-server --watch src/jobs.json --port 5000"
  },
```

再这一块添加`"server": "json-server --watch src/jobs.json --port 5000"`的意思是

- `json-server`-启动json-server工具
- `--watch src/jobs.json`-是在告诉server要监视哪个文件
- `--port 5000`-设置服务器运行在5000端口

> 简单的来说:是告诉`json-server`这个服务员,去盯着`src/jobs.json`站在`5000`号窗口接待客人处理的请求.如果内容有更新你要即使变化
>
> 可以通过 `http://localhost:5000/jobs` 来查看 `jobs` 数据，这就像是把数据挂载到了一个虚拟的服务上，方便访问和操作。

上面发现jobs下没有东西是因为`jobs2.json`文件里的监视文件搞错了,这个项目有两个文件,一个是jobs.json一个是jobs2.json,这里想看到虚拟服务器上的内容数据要,监视正确的文件名把`jobs.json`改为`jobs2.json`就可以了。

## Axios

> 上面的方法过于繁琐,下面使用axios会方便简洁很多

先进行安装`Axions`

```bash
cnpm i axios
```

原本`JobListings.vue`文件使用了本地导入的json文件,

缺点:

- 文件数据是静态的,直接写再前端代码里
- 一旦要更新数据,需要修改代码,并重新部署
- 所有数据都会一次性加载到内存中
- 没有网络请求,加载更快但是不太灵活

现在更新使用`axios`来进行冲服务器获取

优点:

- 数据是动态的,从服务器获取
- 可以随时更新服务器上的数据,前端会自动获取最细的数据
- 支持分页
- 更接近真实开发

更改

先把`JobListings.vue`的文件里`import jobData from '@/jobs.json'`这个导入本地的删掉,换成

`import axios from 'axios'`,后在`import {ref,defineProps,} from 'vue',`添加个`onMounted`挂载生命周期钩子

在下面方法写一个异步判断

```js
onMounted(async() => {
    try{
        const response = await axios.get('http://localhost:5000/jobs');
        jobs.value = response.data;
    }
    catch(error){
        console.error('Error ftching jobs',error)
    }
})
```

解释:

这个用到生命周期的意思是,当页面挂载好了后,马上执行`onMounted(() => { ... })`里的事情

- `const response = await axios.get('http://localhost:5000/jobs');`  等待axios 去这个url地址拿数据
- `jobs.value = response.data;`  拿到数据后,把数据放进jobs变量里
- `console.error('Error ftching jobs',error)`如果中间出现了仍和问题,在控制台答应错误信息

生活例子

> 1. 就像你开了一家店
> 2. 当店门一打开（onMounted）
> 3. 你马上打电话给仓库要货（axios.get）
> 4. 等待仓库送货（await）
> 5. 货到了就把商品摆上架子（jobs.value = response.data）
> 6. 如果送货过程中出了问题（比如车坏了），就记录下来（catch error）
> 7. async/await 的作用就是：
>    1. 告诉程序"要耐心等待"数据回来
>    2. 不要着急往下执行
>    3. 就像你打电话订外卖，要等外卖送到才能吃

## 使用reactive

在`JobListings.vue`导入`reactive`去代替`ref` ,`import {ref, reactive, defineProps , onMounted } from 'vue';`

这样子下面就不需要.value了

```js
const state = reactive({
    jobs:[],
    isLoading:true
});

onMounted(async() => {
    try {
        const response = await axios.get('http://localhost:5000/jobs');
        // state.jobs 写法reactive
        state.jobs = response.data;
    } catch (error) {
        console.error('Error fetching jobs', error)
    } 
    // 写法reactive
    finally{
        state.isLoading = false;
    }
});

//这里因为没有.value,所以需要用到state.jobs
 <JobListing v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job" />
```

### **`reactive` 和 `ref` 区别：**

- **`reactive`**：用来处理 **对象**（包括数组和普通对象）。它会把对象变成响应式的，也就是说，当你修改对象的属性时，Vue 会自动追踪这些变化并更新视图。**直接使用对象的属性，不需要 `.value`**。
- **`ref`**：用来处理 **基本数据类型**（如数字、字符串、布尔值等），当你使用 `ref` 时，它会返回一个包含 `.value` 的对象，所以你需要通过 `.value` 来访问或修改值。

## 添加加载动画`spinner`

![image-20241109222335573](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241109222335573.png)

这个加载动画在数据没有拉去到时会显示,如果拉去到了则会马上显示数据内容,

这样就向用户传达「数据正在加载中」的状态

先下载这个加载模块

 终端输入: `cnpm i spinner`

`JobListings.vue`部分

导入`import PulseLoader from 'vue-spinner/src/PulseLoader';`

然后再下面部分添加一个加载动画

```vue
<!-- 加载动画（spinner） -->
<div v-if="state.isLoading" class="text-center text-gray-500 py-6">
    <PulseLoader/>
</div>


<!--加载完成显示--->
<!--这里加个else来判断,如果state.isLoading等于真的话就显示加载组件,如果为假的话就显示内容--->
<div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
    <JobListing v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job" />
</div>
```

这里还配合了之前写的`state`状态

- `state`使用 `reactive` 定义为一个响应式对象，包含两个属性：
  - `jobs`：存储职位列表数据，初始为空数组。
  - `isLoading`：控制加载动画的显示状态，初始值为 `true` 表示加载中。

### `JobListing.vue`部分

> 在这个文件新加入了
>
> ```js
> import PulseLoader from 'vue-spinner/src/PulseLoader';
> import {reactive ,onMounted} from 'vue';
> import {useRoute,RouterLink} from 'vue-router';
> import axios from 'axios';
> 
> const route = useRoute();
> 
> const jobId = route.params.id;
> 
> const state = reactive({
>  job:{},
>  isLoading: true
> });
> 
> onMounted(async() => {
>  try {
>      const response = await axios.get(`http://localhost:5000/jobs/${jobId}`);
>      // state.jobs 写法reactive
>      state.job = response.data;
>  } catch (error) {
>      console.error('Error fetching job', error)
>  } 
>  // 写法reactive
>  finally{
>      state.isLoading = false;
>  }
> });
> ```
>
> - 这里用了  `const route = useRoute();` 用于获取当前路由的一些信息,
>   - 比如
>   - `params`（路径参数）、`query`（查询参数）、`name`（路由名称）等属性通过这些对象可以在组件中访问当前路由的详细信息就不用依赖于全局的`this.$route`
>   
> - `const jobId = route.params.id;`
>
>   - 这里定义了 `jobId`，通过 `route.params.id` 获取路由路径中的 `id` 参数，并赋值给 `jobId`。`http://localhost:8000/jobs/1` 中的 `1` 就是 `jobId
>
> - 使用 `axios.get` 发送异步请求，访问 `http://localhost:5000/jobs/${jobId}`，其中 `${jobId}` 会被替换为当前路由中的 `id`。
>
>   请求成功时，将返回的数据赋值给 `state.job`，存储在响应式状态中，以便组件可以自动更新显示职位信息。
>
> `<section v-if="!state.isLoading" class="bg-green-50">`这里就判断isLoading是否为真如果为`**假**`就显示信息,如果为`**真**`就显示加载组件
>
> - 这里这样子是因为上面设置了state.Loding 默认值为false

#### `template`部分

```html
<div class="text-gray-500 mb-4">{{ state.job.type }}</div>
```

这个部分的{{}}里面的意思是,上面做一个异步请求接收,赋值给了state,然后这个部分就是在调用这个state里的job下的type数据,
这些数据是部署在了`https://localhost:5000/jobs/${jobId}`下的

> :to="`/jobs/edit/${state.job.id}`"

：这里的 `state.job.id` 动态插入到路径中，用于传递职位的 ID，使得跳转到 `/jobs/edit/:id` 时能够知道要编辑的职位是哪一个

## 创建BackButton.vue

创建这个是为了有一个返回按钮

```vue
<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
        <section>
      <div class="container m-auto py-6 px-6">
        <RouterLink
          to="/jobs"
          class="text-green-500 hover:text-green-600 flex items-center"
        >
          <i class="pi pi-arrow-circle-left"></i> Back to Job Listings
        </RouterLink>
      </div>
    </section>
</template>
```

## 设置代理

在开发中，前端和后端通常运行在不同的端口上（比如前端在 `8000` 端口，后端在 `5000` 端口），这会导致 **跨域问题**，让前端无法直接请求后端的数据。代理的作用就是解决这个问题。

在`vite.config.js`文件`export default defineConfig({....})`中`server`下添加新的内容

```js
proxy: {
  '/api': { // 规则：如果前端请求路径以 `/api` 开头
    target: 'http://localhost:5000', // 就把请求转发到 http://localhost:5000 这个地址
    changeOrigin: true, // 改变请求的源头，防止被拦截
    rewrite: (path) => path.replace(/^\/api/, '') // 去掉 `/api` 前缀，这样后端可以识别正确的路径
  }
}
```

> 这个配置的意思是，如果前端请求的路径是 `/api/jobs`，代理会把这个请求改成 `http://localhost:5000/jobs`，然后再发送给后端服务器。最终后端服务器接收到的路径是 `/jobs`，而不是 `/api/jobs`

在`JobListings.vue`组件中更改`axios`链接改为`axios.get('/api/jobs')`这里向路径/api/jobs发送请求,为上面设置了代理,vite会把这个请求拦截下来,

- ​	然后根据代理规则.vite会把请求的路径/api/jobs改为`http://localhost:5000/jobs`再发送给后端
- **`state.jobs = response.data`**：如果请求成功，返回的数据会赋值给 `state.jobs`，页面就可以使用这些数据。
- **加载状态**：最后把 `isLoading` 设置为 `false`，让加载动画消失.

## 添加新工作

```js
const form = reactive({
    type:'Full-Time',
    title:'',
    description:'',
    salary:'',
    location:'',
    company:{
        name:'',
        description:'',
        contactEmail:'',
        contactPhone:'',
    },
});
```

- 这里使用`reactive`创建了一个响应式的`form`对象,里面包含了职位信息（如 `type`、`title`、`description` 等）以及公司信息（`company`）。
- 在初始状态下,`form`的各属性都默认为空的字符串,这样子可以在表单中输入数据

### 使用v-model绑定表单数据

```html
<div class="mb-4">
    <label class="block text-gray-700 font-bold mb-2"
           >Job Listing Name</label
        >
    <input
           type="text"
           v-model="form.title"
           id="title"
           name="name"
           class="border rounded w-full py-2 px-3 mb-2"
           placeholder="eg. Beautiful Apartment In Miami"
           required
           />
</div>
```

- 例如上面,在每个表单输入数据里绑定一个`v-model`用于在用户输入时,跟后端数据实时更新,这个方法为双向数据绑定,避免用户在输入时,和后端输入的数据不一致的问题

### 提交表单的handleSubmit函数

```html
<form @submit.prevent="handleSubmit">
```

在表单这里绑定一个提交事件`(vue语法@或v-click)`, 上面提交事件后面还设置了拦截,`prevent`阻止了表单默认提交,让表单提交时以定义的`handleSubmit`函数来实现

### handleSubmit函数

```js
const handleSubmit = async () => {
    const newJob = {
        title: form.title,
        type: form.type,
        location: form.location,
        description: form.description,
        salary: form.salary,
        company: {
            name: form.company.name,
            description: form.company.description,
            contactEmail: form.company.contactEmail,
            contactPhone: form.company.contactPhone,
        },
    };
    try {
        const response = await axios.post('/api/jobs', newJob);
        router.push(`/jobs/${response.data.id}`);
    } catch (error) {
        console.error('Error fetching job', error);
    }
};
```

- 这里做了异步写法,在`handleSubmit`里创建了一个新的工作对象`newJob`这个工作对象包括了职位和公司信息
- 发送请求使用`axios.post`方法将`newJob`发送代理路径到`/api/jobs`由这个代理转发给后端的`http://localhost:5000/jobs`,后端会接收完整的职位信息,从而保存到服务器上
- 路由跳转:如果发送请求成功的话,会跳转到新添加的信息详情界面
  - `import router from '@/router'`因为这里导入了路由配置界面所以`handleSubmit`能够进行跳转

## 添加删除按钮

用到了`cnpm i vue-toastification@next`这是一个vue好看的弹窗显示

`main.js`文件

```js
import "./assets/main.css";
import "primeicons/primeicons.css";
import Toast from "vue-toastification";
import "vue-toastification/dist/index.css";
import router from "./router";

import { createApp } from "vue";
import App from "./App.vue";

const app = createApp(App);

app.use(router);
app.use(Toast);

app.mount("#app");
```

**`import Toast from "vue-toastification";`**：这行代码将 `vue-toastification` 插件导入到应用中

**`import "vue-toastification/dist/index.css";`**：这行代码导入了插件的 **默认样式**，它定义了 Toast 消息的外观和动画效果。

**`app.use(Toast);`**这里将Toast这个插件注入到了整个vue项目里

### 弹窗通知`AddJob`视图

```js
const handleSubmit = async () => {
    const newJob = {
        title:form.title,
        type:form.type,
        location:form.location,
        description:form.description,
        salary:form.salary,
        company:{
            name:form.company.name,
            description:form.company.description,
            contactEmail:form.company.contactEmail,
            contactPhone:form.company.contactPhone
        },
    };
    try {
        const response = await axios.post('/api/jobs',newJob)
        toast.success('JOB添加成功')
        router.push(`/jobs/${response.data.id}`);
    } catch (error) {
        console.error('Error fetching job', error)
        toast.error('JOB 未添加')
    } 
};

const toast = useToast();
```

> 这里是导入了Toast插件,然后定义了toast实例函数,又在异步里设置了toast调用,发送请求.如果发送请求成功就显示新的页面,并同时提示Job添加成功
>
> 如果又错误,则显示报错信息,和弹窗提示Job未添加
>
> 如图显示:弹窗效果

![image-20241110152817327](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241110152817327.png)

### 删除按钮完善

`JobView.vue`组件

```js
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import {reactive ,onMounted} from 'vue';
import {useRoute,RouterLink,useRouter} from 'vue-router';
import BackButton from '@/components/BackButton.vue';
import { useToast } from 'vue-toastification';
import axios from 'axios';
```

1. 在这里先导入了`toast`,侧弹窗插件
2. 再导入`useR outer`,这个**`useRouter`** 就是用来 **控制页面之间的跳转** 的

```js
const router = useRouter();
const toast = useToast();
```

- 定义这两个新导入的方法

- 在删除按钮绑定deletJob方法`@click="deletJob`

  - ```html
    <button @click="deletJob"
            class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
            >
        Delete Job
    </button>
    ```

- `deletJob`方法函数

  - ```js
    const deletJob = async () => {
      try{
        const confirm = window.confirm('你真的要删除这个工作职位吗?');
        if (confirm){
          await axios.delete(`/api/jobs/${jobId}`);
          toast.success('Job 删除成功');
          router.push('/jobs');
        }
      }
      catch(error){
        console.error('Error deleting job',error);
        toast.error('Job 删除失败')
      }
    };
    ```

  - 用异步进行判断,并用了js原始的弹窗警告,`window.confirm`

    1. 当用户点击删除按钮是进行异步拦截判断,
    2. 弹出警告如果用户选择了yes,就继续执行`axios.delete`向后端代理数据发送请求删除当前jobs的id
    3. 如果删除成功就执行路由跳转,跳转到jobs下并同时显示`job删除成功`
    4. 如果请求失败则`F12`终端弹出提示显示`Error deleting job`并同时toast插件弹出删除失败

## 编辑工作按钮

```vue
<script setup>
    import router from '@/router';
    import { reactive,onMounted  } from 'vue';
    import {useRoute} from 'vue-router';
    import { useToast } from 'vue-toastification';
    import axios from 'axios';

    const route = useRoute();

    const jobId = route.params.id;

    const form = reactive({
        type:'Full-Time',
        title:'',
        description:'',
        salary:'',
        location:'',
        company:{
            name:'',
            description:'',
            contactEmail:'',
            contactPhone:'',
        },
    });

    const state = reactive({
        job:{},
        isLoading:true,
    });

    const handleSubmit = async () => {
        const UpdatedJob = {
            title:form.title,
            type:form.type,
            location:form.location,
            description:form.description,
            salary:form.salary,
            company:{
                name:form.company.name,
                description:form.company.description,
                contactEmail:form.company.contactEmail,
                contactPhone:form.company.contactPhone
            },
        };
        try {
            const response = await axios.put(`/api/jobs/${jobId}`,UpdatedJob)
            toast.success('JOB修改成功')
            router.push(`/jobs/${response.data.id}`);
        } catch (error) {
            console.error('Error fetching job', error)
            toast.error('JOB 修改错误')
        } 
    };

    onMounted (async() =>{
        try{
            const response = await axios.get(`/api/jobs/${jobId}`);
            state.job = response.data;
            form.title = state.job.title;
            form.type = state.job.type;
            form.description = state.job.description;
            form.location= state.job.location;
            form.salary = state.job.salary;
            form.company.name = state.job.company.name;
            form.company.contactEmail = state.job.company.contactEmail;
            form.company.contactPhone = state.job.company.contactPhone;
            form.company.description = state.job.company.description;
        }
        catch(error){
            console.error('Error fetching job',error);
        }finally{
            state.isLoading = false;
        }
    })

    const toast = useToast();
</script>

<template>
<section class="bg-green-50">
    <div class="container m-auto max-w-2xl py-24">
        <div
             class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0"
             >
            <form @submit.prevent="handleSubmit">
                <h2 class="text-3xl text-center font-semibold mb-6">Edit Job</h2>

                <div class="mb-4">
                    <label for="type" class="block text-gray-700 font-bold mb-2"
                           >Job Type</label
                        >
                    <select
                            v-model="form.type"
                            id="type"
                            name="type"
                            class="border rounded w-full py-2 px-3"
                            required
                            >
                        <option value="Full-Time">Full-Time</option>
                        <option value="Part-Time">Part-Time</option>
                        <option value="Remote">Remote</option>
                        <option value="Internship">Internship</option>
    </select>
    </div>

                <div class="mb-4">
                    <label class="block text-gray-700 font-bold mb-2"
                           >Job Listing Name</label
                        >
                    <input
                           type="text"
                           v-model="form.title"
                           id="title"
                           name="name"
                           class="border rounded w-full py-2 px-3 mb-2"
                           placeholder="eg. Beautiful Apartment In Miami"
                           required
                           />
    </div>
                <div class="mb-4">
                    <label
                           for="description"
                           class="block text-gray-700 font-bold mb-2"
                           >Description</label
                        >
                    <textarea
                              id="description"
                              v-model="form.description"
                              name="description"
                              class="border rounded w-full py-2 px-3"
                              rows="4"
                              placeholder="Add any job duties, expectations, requirements, etc"
                              ></textarea>
    </div>

                <div class="mb-4">
                    <label for="type" class="block text-gray-700 font-bold mb-2"
                           >Salary</label
                        >
                    <select
                            id="salary"
                            v-model="form.salary"
                            name="salary"
                            class="border rounded w-full py-2 px-3"
                            required
                            >
                        <option value="Under $50K">under $50K</option>
                        <option value="$50K - $60K">$50 - $60K</option>
                        <option value="$60K - $70K">$60 - $70K</option>
                        <option value="$70K - $80K">$70 - $80K</option>
                        <option value="$80K - $90K">$80 - $90K</option>
                        <option value="$90K - $100K">$90 - $100K</option>
                        <option value="$100K - $125K">$100 - $125K</option>
                        <option value="$125K - $150K">$125 - $150K</option>
                        <option value="$150K - $175K">$150 - $175K</option>
                        <option value="$175K - $200K">$175 - $200K</option>
                        <option value="Over $200K">Over $200K</option>
    </select>
    </div>

                <div class="mb-4">
                    <label class="block text-gray-700 font-bold mb-2">
                        Location
    </label>
                    <input
                           type="text"
                           v-model="form.location"
                           id="location"
                           name="location"
                           class="border rounded w-full py-2 px-3 mb-2"
                           placeholder="Company Location"
                           required
                           />
    </div>

                <h3 class="text-2xl mb-5">Company Info</h3>

                <div class="mb-4">
                    <label for="company" class="block text-gray-700 font-bold mb-2"
                           >Company Name</label
                        >
                    <input
                           type="text"
                           v-model="form.company.name"
                           id="company"
                           name="company"
                           class="border rounded w-full py-2 px-3"
                           placeholder="Company Name"
                           />
    </div>

                <div class="mb-4">
                    <label
                           for="company_description"
                           class="block text-gray-700 font-bold mb-2"
                           >Company Description</label
                        >
                    <textarea
                              id="company_description"
                              v-model="form.company.description"
                              name="company_description"
                              class="border rounded w-full py-2 px-3"
                              rows="4"
                              placeholder="What does your company do?"
                              ></textarea>
    </div>

                <div class="mb-4">
                    <label
                           for="contact_email"
                           class="block text-gray-700 font-bold mb-2"
                           >Contact Email</label
                        >
                    <input
                           type="email"
                           v-model="form.company.contactEmail"
                           id="contact_email"
                           name="contact_email"
                           class="border rounded w-full py-2 px-3"
                           placeholder="Email address for applicants"
                           required
                           />
    </div>
                <div class="mb-4">
                    <label
                           for="contact_phone"
                           class="block text-gray-700 font-bold mb-2"
                           >Contact Phone</label
                        >
                    <input
                           type="tel"
                           v-model="form.company.contactPhone"
                           id="contact_phone"
                           name="contact_phone"
                           class="border rounded w-full py-2 px-3"
                           placeholder="Optional phone for applicants"
                           />
    </div>

                <div>
                    <button
                            class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
                            type="submit"
                            >
                        Update Job
    </button>
    </div>
    </form>
    </div>
    </div>
    </section>

</template>
```

### 代码解释

这里是由`AddJobView.vue`复制过来的,新添加内容如下:

- 添加了`onMounted`挂载生命周期钩子

  - ```js
    onMounted (async() =>{
        try{
            const response = await axios.get(`/api/jobs/${jobId}`);
            state.job = response.data;
            form.title = state.job.title;
            form.type = state.job.type;
            form.description = state.job.description;
            form.location= state.job.location;
            form.salary = state.job.salary;
            form.company.name = state.job.company.name;
            form.company.contactEmail = state.job.company.contactEmail;
            form.company.contactPhone = state.job.company.contactPhone;
            form.company.description = state.job.company.description;
        }
        catch(error){
            console.error('Error fetching job',error);
        }finally{
            state.isLoading = false;
        }
    })
    ```

  - 这里的意思是axios使用`get`向代理后端发送了请求用来获取当前界面的jobid,获取到的数据返回给response并赋值给job,

  - `form.title = state.job.title;`这些类似的意思是用户点击修改时的页面,后端返回的数据,这样子就能看到原来的数据,然后进行更改

- 创建了state实例

  - ```js
    const state = reactive({
        job:{},
        isLoading:true,
    });
    ```

  - **`job: {}`**：`job` 是一个空对象，用来存放从后端获取到的职位数据。等请求到数据后，就会把职位的详细信息（如标题、类型、描述等）保存到这个 `job` 里。

  - **`isLoading: true`**：`isLoading` 是一个布尔值（true 或 false），用来表示数据是否在加载中。初始值是 `true`，意思是“数据还没加载好”。当数据加载完成后，`isLoading` 会被设置为 `false`，表示“加载结束，可以显示内容了”。

- 路由跳转`import {useRoute} from 'vue-router';`  创建了  `const route = useRoute();`实例

- `const jobId = route.params.id;` 可理解为调用路由实例来进行获取当前页面

- `const handleSubmit = async () => {const UpdatedJob = {......}}`这里把原先的newjob修改为`updatedjob`

- 异步判断使用了`put`用于axios进行更新服务器的内容`const response = await axios.put(`/api/jobs/${jobId}`,UpdatedJob)`
