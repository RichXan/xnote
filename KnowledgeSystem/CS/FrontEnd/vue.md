
# 基础知识
## 1. Proxy代理
- Proxy : 代理，可以理解为在对象之前设置的一个”拦截“。

### 基本使用
`const p = new Proxy(target, handler);`
-   `target`： 所要拦截的目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）
-   `handler`：一个对象，定义要拦截的行为
```js
const p = new Proxy({}, {
    get(target, propKey) {
        return '哈哈，你被我拦截了';
    }
});
console.log(p.name);
// 哈哈，你被我拦截了
```




```js
const app = Vue.createApp({
	data(){
		return {
			count: 4
		}
	},
	methods: {  
		increment() {  
	        // `this` 指向该组件实例  
		    this.count++  
    }
})
document.write(app.count)  // 4
app.count = 6
document.write(vm.$data.count)   // 6
app.increment()

```
>1. `v-model` 指令用来在 input、select、textarea、checkbox、radio 等表单控件元素上创建双向数据绑定，根据表单上的值，自动更新绑定的元素的值。
>2. 按钮的事件我们可以使用` v-on `监听事件，并对用户的输入进行响应。
>3. `v-show`根据条件展示元素
>4. `v-for= "site in sites"`      
  `sites:[text:'Google',text:'Runoob',text:'Taobao']
  5.`// 定义一个名为 runoob 的新全局组件 app.component('runoob', { template: '<h1>自定义组件!</h1>' })`

```js
<div id="app"> 
	<runoob></runoob> 
</div> 

<script> 
// 创建一个Vue 应用 
const app = Vue.createApp({}) 

// 定义一个名为 runoob 的新全局组件 
app.component('runoob', {
	template: '<h1>自定义组件!</h1>' 
})

app.mount('#app') 
</script>
```




`v-bind`缩写     :
`v-on`缩写        @


1. 使用子组件的时候通过属性传递值。
2. 定义的时候通过props接收值。
3. 在普通的HTML中，传值的时候如果使用的是短横线，接收(props)的时候要使用驼峰。
4. 在普通的HTML中，传值的时候（不区分大小写）如果使用的是驼峰，接收的时候要全小写。
5. 在模板里，传值的时候区分大小写，如果说使用的是驼峰，那么接收的时候也要使用驼峰

### 父子组件传值
子组件接收父组件值的有三种方式
```js
// 第一种
defineProps(["page"]);

//第二种，可以设置传来值的类型
defineProps({
	page:Number
});

//第三种，可以设置传来值的类型和默认值
defineProps({
	page:{
		type:Number,
		default:2
	}
});

//第四种，可以设置传来值的多种类型
defineProps({
	page:[String,Number]
});


```

### 父子组件传方法
子组件中定义接收父组件传过来的方法
```js
<script setup>
import { ref, reactive, toRefs, defineProps, defineEmits } from "vue";
const emit = defineEmits(["pageFn"]);  //pageFn父组件传来的方法
//传给父组件的值的方法
const btnFn=()=>{
    emit('pageFn',1111)
}
</script>
```

### 子组件获取父组件传来的值与方法
```ts
// 父组件
<Child
  :id="updateId"
  @fetchData="fetchData"

/>

<script setup>
  const updateId = ref(0)
  const fuFetchData = () => {
	  console.log("ok")
  }

</script>

// 子组件
  // 接收从父组件传过来的值
  const props = defineProps({
    id: Number,
    visible: Boolean
  });
  // 使用从父子间传过来的值
  console.log(props.id)

  // 接收从父组件传过来的方法
  const emits = defineEmits(['fetchData']);
  // 使用从父组件传过来的方法
  emits('disvisible');
```

### provide&inject 注入
```js
//父组件
  // 查看抽屉可视化操作相关代码
  const getId = ref(0);
  const viewVisible = ref(false);
  // 参数注入到子组件中
  provide('getId', getId);
  provide('viewVisible', viewVisible);

//子组件
  // 注入响应式的值
  const getId = inject('getId');
  const visibleIn = inject('viewVisible');
```

### 1. 如何调用子组件内setup内的方法?

1.  子组件在setup写好方法`method`,并通过`return`暴露出去
2.  父组件调用子组件时为其添加`ref`属性
3.  父组件setup内拿到子组件添加的`ref`属性`property`,再通过`property.value.method()`调用

子组件
```vue
<script lang="ts">
import { defineComponent, ref } from 'vue'
export default defineComponent({
  name: 'Son',
  setup() {
    const valueRef = ref('')    
    // 该函数可以接受父级传递一个参数，并修改valueRef的值
    const acceptValue = (value: string) => (valueRef.value = value)
    return {
      acceptValue,
      valueRef
    }
  }
})
</script>
```
<template>
  // 渲染从父级接受到的值
  <div>Son: {{ valueRef }}</div>
</template>

父组件

```vue
<template>
  <div>sonRef</div>
  <button @click="sendValue">send</button>
  // 这里ref接受的字符串，要setup返回的ref类型的变量同名
  <Son ref="sonRef" />
</template>
<script lang="ts">
import { defineComponent, ref } from 'vue'
import Son from '@/components/Son.vue'
export default defineComponent({
  name: 'Demo',
  components: {
    Son
  },
  setup() {
    // 如果ref初始值是一个空，可以用于接受一个实例
    // vue3中获取实例的方式和vue2略有不同
    const sonRef = ref()
    const sendValue = () => {
      // 可以拿到son组件实例，并调用其setup返回的所有信息
      console.log(sonRef.value)      
      // 通过调用son组件实例的方法，向其传递数据
      sonRef.value.acceptValue('123456')
    }
    return {
      sonRef,
      sendValue
    }
  }
})
</script>
```



# 1、基础
## 1.1 基础概念
### 1.1.1 单文件组件
用一种类似HTML格式的文件写Vue组件，被称为的单文件组件（SFC）。
也就是说Vue的单文件组件里包含逻辑（js），模板（html）和样式（CSS）都封装在同一个文件里。
```vue
<script>
export default {
  data() {
    return {
      count: 0
    }
  }
}
</script>

<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
```


### 1.1.2 API风格
两者的区分在于如何描述组件的逻辑（js）

#### 选项式API
用包含多个选项的对象来描述组件的逻辑，例如 `data`、`methods` 和 `mounted`。选项所定义的属性都会暴露在函数内部的 `this` 上，它会指向当前的组件实例。
```vue
<script>
export default {
  // data() 返回的属性将会成为响应式的状态
  // 并且暴露在 `this` 上
  data() {
    return {
      count: 0
    }
  },

  // methods 是一些用来更改状态与触发更新的函数
  // 它们可以在模板中作为事件监听器绑定
  methods: {
    increment() {
      this.count++
    }
  },

  // 生命周期钩子会在组件生命周期的各个不同阶段被调用
  // 例如这个函数就会在组件挂载完成后被调用
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

#### 组合式API
通过组合式 API，我们可以使用导入的 API 函数来描述组件逻辑。在单文件组件中，组合式 API 通常会与 [`<script setup>`](https://cn.vuejs.org/api/sfc-script-setup.html) 搭配使用。这个 `setup` attribute 是一个标识，告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API。比如，`<script setup>` 中的导入和顶层变量/函数都能够在模板中直接使用。

下面是使用了组合式 API 与 `<script setup>` 改造后和上面的模板完全一样的组件：
```vue
<script setup>
import { ref, onMounted } from 'vue'

// 响应式状态
const count = ref(0)

// 用来修改状态、触发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```


## 1.2 知识点
### 1.2.1 挂载应用
应用实例必须调用了`.mount()`方法后才会渲染出来
```html
<div id="app"></div>
```
```js
app.mount('#app')
```

### 1.2.2 文本插值
最基本的数据绑定形式是文本插值，它使用的是“Mustache”语法 (即双大括号)：
```html
<span>Message: {{ msg }}</span>
```

双大括号标签会被替换为相应组件实例中 `msg` 属性的值。同时每次 `msg` 属性更改时它也会同步更新。


### 1.2.3 attribute绑定
双大括号不能在 HTML attributes 中使用。想要响应式地绑定一个 attribute，应该使用 [`v-bind` 指令](https://cn.vuejs.org/api/built-in-directives.html#v-bind)：

```html
<div v-bind:id="dynamicId"></div>
```
`v-bind` 指令指示 Vue 将元素的 `id` attribute 与组件的 `dynamicId` 属性保持一致。如果绑定的值是 `null` 或者 `undefined`，那么该 attribute 将会从渲染的元素上移除。

#### 简写
因为 `v-bind` 非常常用，我们提供了特定的简写语法：
```html
<div :id="dynamicId"></div>
```


### 1.2.4 指令directives
#### 参数 Arguments[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#arguments)

某些指令会需要一个“参数”，在指令名后通过一个冒号隔开做标识。例如用 `v-bind` 指令来响应式地更新一个 HTML attribute：

```html
<a v-bind:href="url"> ... </a>

<!-- 简写 -->
<a :href="url"> ... </a>
```

这里 `href` 就是一个参数，它告诉 `v-bind` 指令将表达式 `url` 的值绑定到元素的 `href` attribute 上。在简写中，参数前的一切 (例如 `v-bind:`) 都会被缩略为一个 `:` 字符。

另一个例子是 `v-on` 指令，它将监听 DOM 事件：

```html
<a v-on:click="doSomething"> ... </a>

<!-- 简写 -->
<a @click="doSomething"> ... </a>
```





vue： 
	v-bind :    v-on @  v-slot #
	父子组件之间的通信（父组件传值给子组件，子组件传值给父组件）
	useRouter 用于重定向跳转页面
	ref、reactive 数据响应式的区别
	computed、setLoading
	主要使用的vue3，vue3的组合式与选项式的区别，arco框架主要使用组合式api风格。
	一个vue文件分为template script style三个组成
		template 不懂的可以去pro.arco.design上查（首先要了解html的基本知识：标签以及标签属性attribute） 
		script语法不懂的在vue3官网上查
		style是css的语法（一般没怎么改）

ES6：
	类型声明  : 
	箭头函数 () => (){}
	泛型<>  （涉及一点）
	async await （异步函数操作）
	interface
	try catch finally
	模块化 export  /  import
### setup
- setup 使用方法
	- https://blog.csdn.net/qq_25506089/article/details/114743592
	- 在添加了setup的script标签中，**我们不必声明和方法，这种写法会自动将所有顶级变量、函数，均会自动暴露给模板（template）使用**  
		这里强调一句 “**暴露给模板，跟暴露给外部不是一回事**”
	- https://juejin.cn/post/7009282373476941831


### loading
只是用于展示是否是在加载中


### ?:
有`?:`时不能赋值为`null`
typeScript中的?:问号冒号表示此参数或属性可选