# Vue Study Notes(All)
Read: https://liuhongwei3.github.io/Vue-Study-Notes

<div style="display: inline-flex;">
<img alt="" src="https://img.shields.io/badge/Author-Tadm-pink.svg?style=flat-square"/>
<img alt="" src="https://img.shields.io/apm/l/vim-mode?style=flat-square"/>
<img alt="" src="https://img.shields.io/badge/Program-Vue-orange"/>
</div>

# Acquaint Vue

> 本文教程视频源：https://www.bilibili.com/video/av89760569

## Vue是一个渐进式的框架

渐进式意味着可以将Vue作为你**应用的一部分嵌入**其中，带来更丰富的交互体验。

## Features & Web开发中常见的高级功能

解耦视图和数据
可复用的组件
前端路由
状态管理
虚拟DOM

# MVVM

## View层(视图层)：

DOM------主要的作用是给用户展示各种信息。

## Model层(数据层)：

Data

## VueModel层(视图模型层)：

View 和 Model 沟通的桥梁：
一方面它实现了数据绑定，将 Model 的改变实时的反应到 View 中;

另一方面它实现了 DOM监听，当DOM发生一些事件(点击、滚动、touch等)时，可以监听到，并在需要的情况下改变对应的Data。

- m   model  
  - 数据层   Vue  中 数据层 都放在 data 里面
- v   view     视图   
  - Vue  中  view      即 我们的HTML页面  
- vm   （view-model）     控制器     将数据和视图层建立联系      
  - vm 即  Vue 的实例  就是 vm  

# Vue CDN

## 生产环境：

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11/dist/vue.min.js"></script>
```

## 开发环境：

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11/dist/vue.js"></script>
```

# First Simple Demo

```html
<div id="app">
  {{ message }}  //Hello Vue!
</div>
```

```js
var app = new Vue({
  
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

# Vue 生命周期

## 解释

- Vue实例从创建到销毁的过程，这些过程中会伴随着一些函数的自调用，这些函数为钩子函数

## 官方图示

![lifecycle](https://cn.vuejs.org/images/lifecycle.png)

- new Vue------>创建一个vue实例
- init()------>初始化空的vue对象
- beforecreate------>data & methods 还未初始化



- created------>data & methods 已经初始化好，此时可调用methods方法、使用data中的数据(最早)
- compile template------>编译模板在内存中生成DOM树，但此时未加载到页面上



- beforeMount------>模板尚未挂载至页面，页面仍未更新
- create vm.$el & replace el with it------>将内存中编译好的模板替换至页面上,此时可操作页面Dom(最早)
- mounted------>实例初始化加载完毕，进入运行阶段



- beforeUpdate------>data changed,data中数据已更新，页面上的数据仍未更新
- virtual DOM re-render and patch------>根据data的数据在内存中重新渲染新DOM树，更新完毕再将其渲染到页面(model--->view)
- updated------>页面中的data更新了



- beforeDestroy------>运行--->销毁，此时data/methods等仍可用，并未真正销毁
- destroyed------>everything is null


## 常用的 钩子函数

| beforeCreate  | 在实例初始化之后，数据观测和事件配置之前被调用 此时data 和 methods 以及页面的DOM结构都没有初始化   什么都做不了 |
| ------------- | ------------------------------------------------------------ |
| created       | 在实例创建完成后被立即调用此时data 和 methods已经可以使用  但是页面还没有渲染出来 |
| beforeMount   | 在挂载开始之前被调用   此时页面上还看不到真实数据 只是一个模板页面而已 |
| mounted       | el被新创建的vm.$el替换，并挂载到实例上去之后调用该钩子。  数据已经真实渲染到页面上  在这个钩子函数里面我们可以使用一些第三方的插件 |
| beforeUpdate  | 数据更新时调用，发生在虚拟DOM打补丁之前。   页面上数据还是旧的 |
| updated       | 由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。 页面上数据已经替换成最新的 |
| beforeDestroy | 实例销毁之前调用                                             |
| destroyed     | 实例销毁后调用                                               |

## 图解(详细)

![lifecycle.png](https://i.loli.net/2020/01/14/ef6QMCSc29gFAKW.png)

# Vue 实例参数

el---data---computed---methods---watch---props等等

## el

### Explanation

类型：String | HTMLElement
作用：确定Vue实例会管理的DOM

## data

### Explanation

类型：Object | Function
限制：组件的定义只接受 function
作用：Vue实例对应的数据对象

More：实例创建之后，可以通过 vm.$data 访问原始数据对象。Vue 实例也代理了 data 对象上所有的属性，因此访问 vm.a 等价于访问 vm.$data.a。

以 _ 或 $ 开头的属性 不会 被 Vue 实例代理，因为它们可能和 Vue 内置的属性、API 方法冲突。你可以使用例如 vm.$data._property 的方式访问这些属性。

当一个组件被定义，data 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 data 仍然是一个纯粹的对象，则所有的实例将共享引用同一个数据对象！通过提供 data 函数，每次创建一个新实例后，我们能够调用 data 函数，从而返回初始数据的一个全新副本数据对象。

### Example

```js
var data = { a: 1 }

// 直接创建一个实例
var vm = new Vue({
  data: data
})
vm.a // => 1
vm.$data === data // => true

// Vue.extend() 中 data 必须是函数
var Component = Vue.extend({
  data: function () {
    return { a: 1 }
  }
})
```

## methods

### Explanation

类型：{ [key: string]: Function }
详细：methods 将被混入到 Vue 实例中。
可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例
注意: 不应该使用箭头函数来定义 method 函数 (例如 plus: () => this.a++)。
理由是箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向 Vue 实例，this.a 将是 undefined

### Example

```js
var vm = new Vue({
  data: { 
  	a: 1 
  },
  methods: {
    plus: function () {
      this.a++
    }
    // ES6
    plus() {
      this.a++
    }
  }
})
vm.plus()
vm.a // 2
```

## props

### Explanation

类型：Array<string> | Object
详细：可以是数组或对象，用于接收来自父组件的数据。
可以是简单的数组，或者使用对象作为替代，对象允许配置高级选项，如类型检测、自定义验证和设置默认值。

## watch

### Explanation

类型：{ [key: string]: string | Function | Object | Array }
详细：一个对象，键是需要观察的表达式，值是对应回调函数。
值也可以是方法名，或者包含选项的对象。
Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。
注意：也不可以使用箭头函数，同methods

- 一般用于异步或者开销较大的操作
- watch 中的属性 一定是data 中 已经存在的数据 
- **当需要监听一个对象的改变时，普通的watch方法无法监听到对象内部属性的改变，只有data中的数据才能够监听到变化，此时就需要deep属性对对象进行深度监听**

## computed

### Explanation

类型：{ [key: string]: Function | { get: Function, set: Function } }
详细：计算属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算

计算属性将被混入到 Vue 实例中。
所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。
注意：如果你为一个计算属性使用了箭头函数，则 this 不会指向这个组件的实例，不过你仍然可以将其实例作为函数的第一个参数来访问。
如果某个依赖 (比如非响应式属性) 在该实例范畴之外，则计算属性是不会被更新的。

- computed比较适合对多个变量或者对象进行处理后返回一个结果值，也就是数多个变量中的某一个值发生了变化则我们监控的这个值也就会发生变化

```js
//	该计算属性将不再更新，因为 Date.now() 不是响应式依赖
computed: {
  now: function () {
    return Date.now()
  }
}
```

More: https://cn.vuejs.org/v2/guide/computed.html

### Example

```html
<p>{{ fullName }}</p>
<h2>{{totalPrice}}</h2>
```

```js
computed: {
	fullName: function () {
	  return this.firstName + ' ' + this.lastName
	},
	totalPrice: function () {
    let result = 0
    for (let i=0; i < this.books.length; i++) {
      result += this.books[i].price
    }
    return result
  },
  //	getter setter 
  //	一般set不常用,默认get
  fullName: {
    set: function(newValue) {
      // console.log('-----', newValue);
      const names = newValue.split(' ');
      this.firstName = names[0];
      this.lastName = names[1];
    },
    get: function () {
      return this.firstName + ' ' + this.lastName
    }
  },
  reversedMessage: function () {
    // `this` 指向 vm 实例
    return this.message.split('').reverse().join('')
  }
}
```

当firstName or lastName 发生变化时，会触发进行更新fullName

# Vue 基本语法

## Mustache

可以通过{{ argument }}访问获取data中的属性

```html
<h2>{{message}}</h2>
  <h2>{{message}}</h2>
  <!--mustache语法中,不仅仅可以直接写变量,也可以写简单的表达式-->
  <h2>{{firstName + lastName}}</h2>
  <h2>{{firstName + ' ' + lastName}}</h2>
  <h2>{{firstName}} {{lastName}}</h2>
  <h2>{{counter * 2}}</h2>
```

## v-model

双向数据绑定，限制在 `<input>、<select>、<textarea>、components`中使用

```html
<input type="text" v-model="message">
```

等同于(原理)

```html
<input type="text" :value="message" @input="message = $event.target.value">
```

input标签默认会有input方法监听输入信息

## v-on

### Explanation

作用：绑定事件监听器
缩写：@
预期：Function | Inline Statement | Object
参数：event

### Example

```html
<button @click="increment">+</button>
<button @click="decrement">-</button>
```

```js
methods: {
	increment() {
	  this.counter++
	},
	decrement() {
	  this.counter--
	}
}
```

情况一：如果该方法不需要额外参数，那么方法后的()可以不添加。
但是注意：如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去
情况二：如果需要同时传入某个参数，同时需要event时，可以通过$event传入事件。

### Example

```html
<div id="app">
  <!--1.事件调用的方法没有参数-->
  <button @click="btn1Click()">按钮1</button>
  <button @click="btn1Click">按钮1</button>
  //	btn1Click

  <!--2.在事件定义时, 写方法时省略了小括号, 但是方法本身是需要一个参数的, 这个时候, Vue会默认将浏览器生产的event事件对象作为参数传入到方法-->
  <button @click="btn2Click(123)">按钮2</button>
  //	-------- 123
  <button @click="btn2Click()">按钮2</button>
  //	-------- undefined
  <button @click="btn2Click">按钮2</button>
  //	-------- MouseEvent {isTrusted: true, screenX: 140, screenY: 138, clientX: 139, clientY: 22, …}

  <!--3.方法定义时, 我们需要event对象, 同时又需要其他参数-->
  <!-- 在调用方式, 如何手动的获取到浏览器参数的event对象: $event-->
  <button @click="btn3Click(abc, $event)">按钮3</button>
  //	++++++++ 123 MouseEvent {isTrusted: true, screenX: 320, screenY: 142, clientX: 320, clientY: 26, …}
</div>
```

```js
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊',
      abc: 123
    },
    methods: {
      btn1Click() {
        console.log("btn1Click");
      },
      btn2Click(event) {
        console.log('--------', event);
      },
      btn3Click(abc, event) {
        console.log('++++++++', abc, event);
      }
    }
  })

  // 如果函数需要参数,但是没有传入, 那么函数的形参为undefined
  // function abc(name) {
  //   console.log(name);
  // }
  //
  // abc()
```

### v-on 修饰符

.stop - 调用 event.stopPropagation() - 阻止冒泡
.prevent - 调用 event.preventDefault() - 阻止默认事件
.{keyCode | keyAlias} - 只当事件是从特定键触发时才触发回调
.native - 监听组件根元素的原生事件
.once - 只触发一次回调 

#### Example

```html
<div @click="divClick">
  aaaaaaa
  <button @click.stop="btnClick">按钮</button>
</div>

<!--2. .prevent修饰符的使用-->
<br>
<form action="baidu">
  <input type="submit" value="提交" @click.prevent="submitClick">
</form>

<!--3. .监听某个键盘的键帽-->
<input type="text" @keyup.enter="keyUp">

<!--4. .once修饰符的使用-->
<button @click.once="btn2Click">按钮2</button>
```

## v-bind

作用：动态绑定属性
缩写：:
预期：any (with argument) | Object (without argument)
参数：attrOrProp (optional)

```html
<a v-bind:href="url">...</a>
<img :src="url" alt="">
```

More: 绑定class/style

### 对象语法

传给 v-bind:class 一个对象，以动态地切换 class

```html
<div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
```

```js
data: {
  isActive: true,
  hasError: false
}
```

Result:

```html
<div class="static active"></div>
```

### 数组语法

把一个数组传给 v-bind:class，以应用一个 class 列表

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Result:

```html
<div class="active text-danger"></div>
```

Demo: https://cn.vuejs.org/v2/guide/class-and-style.html

## v-text

直接将其作为text输出到页面，不进行任何解析

## v-html

利用解析html再输出到页面

```html
<p v-html="url"></p>
```

```js
data: {
  url: '<a href="http://www.baidu.com">百度一下</a>'
}
```

## v-pre

跳过这个元素和它子元素的编译过程，用于显示原本的Mustache语法
{{ message }}

## v-cloak

解决闪烁问题

```css
<style>
	[v-cloak] {
	  display: none;
	}
</style>
```

```html
<div id="app" v-cloak>
  <h2>{{message}}</h2>
</div>
```

## v-once

能执行一次性地插值，当数据改变时，插值处的内容不会更新

## v-for

语法格式：
v-for="item in items"
v-for="(item, index) in items"
v-for="(item, key, index) in items"

其中的index就代表了取出的item在原数组的索引值。

```html
<div v-for="item in items">{{ item }}</div>
<div v-for="(item, index) in items">{{ index+1 }} - {{ item }}</div>

<!-- 遍历对象操作 -->
<!--1.在遍历对象的过程中, 如果只是获取一个值, 那么获取到的是value-->
<ul>
  <li v-for="item in info">{{item}}</li>
</ul>
<!--2.获取key和value 格式: (value, key) -->
<ul>
  <li v-for="(value, key) in info">{{value}}-{{key}}</li>
</ul>
<!--3.获取key和value和index 格式: (value, key, index) -->
<ul>
  <li v-for="(value, key, index) in info">{{value}}-{{key}}-{{index}}</li>
</ul>
```

```js
info: {
  name: 'tadm',
  age: 19,
  height: 1.75
}
```

点击指定列表更改样式

```html
<ul>
<li v-for="(item, index) in movies" :class="{active: currentIndex === index}" @click="liClick(index)">
  {{index}}.{{item}}</li>
</ul>
```

```js
data: {
  movies: ['速度与激情', '美国队长', '加勒比海盗', '终结者'],
  currentIndex: 0
},
methods: {
  liClick(index) {
    this.currentIndex = index
  }
}
```

注意：使用v-for时，给对应的元素或组件添加上一个:key属性

```html
<tr :key='item.id' v-for='item in books'>
	<td>{{ item.id }}</td>
	<td>{{ item.name }}</td>
	<td>{{ item.date }}</td>
	<td>
		<a href="#" @click.prevent>修改</a>
		<span>|</span>
		<a href="#" @click.prevent>删除</a>
	</td>
</tr>
```

- key 的作用(虚拟DOM Diff算法)

  - **key来给每个节点做一个唯一标识**(不能使用index)
  - **key的作用主要是为了高效的更新虚拟DOM**（增强复用性）

  - - key与内容一一对应，以后数据更新时只需要更新改变的部分到指定位置

## v-if && v-else-if && v-else

根据条件进行渲染

```html
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```

注意：当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级

在使用时可能会出现一些切换时部分内容未切换，比如input输入框中的内容，点击切换时不会改变Input的文字(Vue出于性能考虑尽可能复用已存在的元素)
因此可以给对应的元素添加key，若识别到key不相同则不会复用，若相同仍复用

```html
<span v-if="isUser">
  <label for="username">用户账号</label>
  <input type="text" id="username" placeholder="用户账号" key="username">
</span>
<span v-else>
  <label for="email">用户邮箱</label>
  <input type="text" id="email" placeholder="用户邮箱" key="email">
</span>
<button @click="isUser = !isUser">切换类型</button>
```

## v-show

```html
<h1 v-show="ok">Hello!</h1>
```

### More

v-show 的元素始终会被渲染并保留在 DOM 中。
v-show 只是简单地切换元素的 CSS 属性 display。

### v-show VS v-if

- v-if 是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
- 相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
- v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。
- 因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

- v-show本质就是标签display设置为none，控制隐藏
  - v-show只编译一次，后面其实就是控制css，而v-if不停的销毁和创建，故v-show性能更好一点。
- v-if是动态的向DOM树内添加或者删除DOM元素
  - v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件

# 自定义指令

## Vue.directive  注册全局指令

```html
<!-- 
  使用自定义的指令，只需在对用的元素中，加上'v-'的前缀形成类似于内部指令'v-if'，'v-text'的形式。 
-->
<input type="text" v-focus>
<script>
// 注意点： 
//   1、 在自定义指令中  如果以驼峰命名的方式定义 如  Vue.directive('focusA',function(){}) 
//   2、 在HTML中使用的时候 只能通过 v-focus-a 来使用 
    
// 注册一个全局自定义指令 v-focus
Vue.directive('focus', {
	// 当绑定元素插入到 DOM 中。 其中 el为dom元素
	inserted: function (el) {
		// 聚焦元素
		el.focus();
	}
});
new Vue({
　el:'#app'
});
</script>
```

## Vue.directive  注册全局指令---带参数

```html
<input type="text" v-color='msg'>
<script type="text/javascript">
  /*
    自定义指令-带参数
    bind - 只调用一次，在指令第一次绑定到元素上时候调用
  */
  Vue.directive('color', {
    // bind声明周期, 只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
    // el 为当前自定义指令的DOM元素  
    // binding 为自定义的函数形参   通过自定义属性传递过来的值 存在 binding.value 里面
    bind: function(el, binding){
      // 根据指令的参数设置背景色
      // console.log(binding.value.color)
      el.style.backgroundColor = binding.value.color;
    }
  });
  var vm = new Vue({
    el: '#app',
    data: {
      msg: {
        color: 'aqua'
      }
    }
  });
</script>
```

## 自定义指令局部指令

- 局部指令，需要定义在  directives 的选项   用法和全局用法一样 
- 局部指令只能在当前组件里面使用
- 当全局指令和局部指令同名时以局部指令优先

```html
<input type="text" v-color='msg'>
<input type="text" v-focus>
<script type="text/javascript">
  /*
    自定义指令-局部指令
  */
  var vm = new Vue({
    el: '#app',
    data: {
      msg: {
        color: 'aqua'
      }
    },
 	  //局部指令，需要定义在  directives 的选项
    directives: {
      color: {
        bind: function(el, binding){
          el.style.backgroundColor = binding.value.color;
        }
      },
      focus: {
        inserted: function(el) {
          el.focus();
        }
      }
    }
  });
</script>
```

# 表单操作

## 获取单选框中的值

```html
<!-- 
1、 两个单选框需要同时通过v-model 双向绑定 一个值 
2、 每一个单选框必须要有value属性  且value 值不能一样 
3、 当某一个单选框选中的时候 v-model  会将当前的 value值 改变 data 中的 数据
-->

<input type="radio" id="male" value="1" v-model='gender'>
<label for="male">boy</label>

<input type="radio" id="female" value="2" v-model='gender'>
<label for="female">girl</label>

<label for="agree">
  <input type="checkbox" id="agree" v-model="isAgree">同意协议
</label>
<h2>您选择的是: {{isAgree}}</h2>
<button :disabled="!isAgree">下一步</button>
```

```js
new Vue({
 	data: {
 		isAgree: false,
    // 默认会让当前的 value 值为 2 的单选框选中
    gender: 2
  },
})
```

## label for 介绍(容易忽略)

### 一、使用介绍

定义：for属性规定label与哪个表单元素绑定。

<label>是专门为<input>元素服务的，为其定义标记。

label 和表单控件绑定方式有两种：

#### 方法一：将表单控件作为label的内容，这种就是隐式绑定。

此时不需要for属性，绑定的控件也不需要id属性。

隐式绑定：

```html
<label>Date of Birth: <input type="text" name="DofB" /></label>
```

#### 方法二：为label标签下的for属性命名一个目标表单的id，这种就是显示绑定。

显式绑定：

```html
<label for="SSN">Social Security Number:</label>
<input type="text" name="SocSecNum" id="SSN" />
```

### 二、为什么要给label上面加上for属性

给 label 加了 for 属性绑定了input控件后，可以提高鼠标用户的用户体检。

如果在label 元素内点击文本，就会触发此控件，也就是说，当用户渲染该标签时，浏览器就会自动将焦点转到和标签相关的表单控件上。

## 获取复选框中的值

- 通过v-model
- 和获取单选框中的值一样 
- 复选框 `checkbox` 这种的组合时   data 中的 hobby 我们要定义成数组 否则无法实现多选

```html
<!-- 
	1、 复选框需要同时通过v-model 双向绑定 一个值 
  2、 每一个复选框必须要有value属性  且value 值不能一样 
	3、 当某一个单选框选中的时候 v-model  会将当前的 value值 改变 data 中的 数据
-->

<div>
	<span>Hobbies: </span>
	<input type="checkbox" id="run" value="1" v-model='hobbies'>
	<label for="ball">run</label>
	<input type="checkbox" id="sing" value="2" v-model='hobbies'>
	<label for="sing">sing</label>
	<input type="checkbox" id="code" value="3" v-model='hobbies'>
	<label for="code">code</label>
</div>

<label v-for="item in originHobbies" :for="item">
  <input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
</label>
```

```js
new Vue({
 data: {
    // 默认会让当前的 value 值为 2 和 3 的复选框选中
    hobbies: ['2', '3'],
    originHobbies: ['篮球', '足球', '乒乓球', '羽毛球', '台球', '高尔夫球']
  },
})
```

## 获取下拉框和文本框中的值

```html
<div>
  <span>职业：</span>
   <!--
		1、 需要给select  通过v-model 双向绑定 一个值 
    2、 每一个option  必须要有value属性  且value 值不能一样 
    3、 当某一个option选中的时候 v-model  会将当前的 value值 改变 data 中的 数据
	-->
	<!-- 去掉multiple 即单选取值且将occupation设置为值而不是数组 -->
  <select v-model='occupation' multiple>
    <option value="0">请选择职业...</option>
    <option value="1">教师</option>
    <option value="2">软件工程师</option>
    <option value="3">律师</option>
  </select>
  <!-- textarea 是 一个双标签   不需要绑定value 属性的  -->
  <textarea v-model='desc'></textarea>
</div>
```

```js
new Vue({
 data: {
    // 默认会让当前的 value 值为 2 和 3 的下拉框选中
   	occupation: ['2', '3'],
 	 	desc: 'nihao'
  },
})
```

## 表单修饰符

- .number  转换为数值

  - 注意点：	
  - 当开始输入非数字的字符串时，因为Vue无法将字符串转换成数值
  - 所以属性值将实时更新成相同的字符串。即使后面输入数字，也将被视作字符串。

- .trim  自动过滤用户输入的首尾空白字符

  - 只能去掉首尾的 不能去除中间的空格

- .lazy   将input事件切换成change事件

  - .lazy 修饰符延迟了同步更新属性值的时机。即将原本绑定在 input 事件的同步逻辑转变为绑定在 change 事件上
  - 在失去焦点 或者 按下回车键时才更新

```html
<!-- 自动将用户的输入值转为数值类型 -->
<input v-model.number="age" type="number">

<!--自动过滤用户输入的首尾空白字符   -->
<input type="text" v-model.trim="msg">

<!-- 在“change”时而非“input”时更新 -->
<input type="text" v-model.lazy="msg" >
```

# 过滤器

## Explanation

- Vue.js允许自定义过滤器，可被用于一些常见的文本格式化。
- 过滤器可以用在两个地方：双花括号插值和v-bind表达式。
- 过滤器应该被添加在JavaScript表达式的尾部，由“管道”符号指示
- 支持级联操作
- 过滤器不改变真正的`data`，而只是改变渲染的结果，并返回过滤后的版本
- 全局注册时是filter，没有s的。而局部过滤器是filters，是有s的

### Example

```html
<div id="app">
  <input type="text" v-model='msg'>
    <!-- upper 被定义为接收单个参数的过滤器函数，表达式  msg  的值将作为参数传入到函数中 -->
  <div>{{msg | upper}}</div>
  <!--  
    支持级联操作
    upper  被定义为接收单个参数的过滤器函数，表达式msg 的值将作为参数传入到函数中。
  	然后继续调用同样被定义为接收单个参数的过滤器 lower ，将upper 的结果传递到lower中
	-->
  <div>{{msg | upper | lower}}</div>
  <div :abc='msg | upper'>测试数据</div>
</div>
```

```js
<script type="text/javascript">
	//  lower  为全局过滤器     
	Vue.filter('lower', function(val) {
	  return val.charAt(0).toLowerCase() + val.slice(1);
	});
	var vm = new Vue({
	  el: '#app',
	  data: {
	    msg: ''
	  },
	  //  定义filters 中的过滤器为局部过滤器 
	  filters: {
	    //   upper  自定义的过滤器名字 
	    //    upper 被定义为接收单个参数的过滤器函数，表达式  msg  的值将作为参数传入到函数中
	    upper: function(val) {
	     //  过滤器中一定要有返回值 这样外界使用过滤器的时候才能拿到结果
	      return val.charAt(0).toUpperCase() + val.slice(1);
	    }
	  }
	});
</script>
```

##  过滤器中传递参数

### example

```html
<div id="box">
  <!--
		filterA 被定义为接收三个参数的过滤器函数。
		其中 message 的值作为第一个参数，
		普通字符串 'arg1' 作为第二个参数，表达式 arg2 的值作为第三个参数。
	-->
    {{ message | filterA('arg1', 'arg2') }}
</div>
```

```js
<script>
  // 在过滤器中 第一个参数 对应的是  管道符前面的数据   n  此时对应 message
  // 第2个参数  a 对应 实参  arg1 字符串
  // 第3个参数  b 对应 实参  arg2 字符串
  Vue.filter('filterA',function(n,a,b){
    if(n<10){
        return n+a;
    }else{
        return n+b;
    }
  });
  new Vue({
    el:"#box",
    data:{
        message: "哈哈哈"
    }
  })
</script>
```

## dateFormat 过滤器

```js
Vue.filter('dateFormat', function (dateStr, pattern = "") {
  // 根据给定的时间字符串，得到特定的时间
  var dt = new Date(dateStr)

  //   yyyy-mm-dd
  var y = dt.getFullYear()
  var m = dt.getMonth() + 1
  var d = dt.getDate()

  // return y + '-' + m + '-' + d

  if (pattern.toLowerCase() === 'yyyy-mm-dd') {
    return `${y}-${m}-${d}`
  } 
  else {
    var hh = dt.getHours()
    var mm = dt.getMinutes()
    var ss = dt.getSeconds()

    return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
  }
})
```

# ES6 部分语法

## var(ES5) & let

### 块级作用域

指定变量能在该块级作用，不影响其他代码块的执行
var ---> don't have;
let ---> have;

```js
//	i 始终 = btns.length
var btns = document.getElementsByTagName('button');
for (var i=0; i<btns.length; i++) {
  btns[i].addEventListener('click', function () {
    console.log('第' + i + '个按钮被点击');
  })
}

// 为什么闭包可以解决问题: 函数是一个作用域.
var btns = document.getElementsByTagName('button');
for (var i=0; i<btns.length; i++) {
  (function (num) { // 0
    btns[i].addEventListener('click', function () {
      console.log('第' + num + '个按钮被点击');
    })
  })(i)
}

//	i = i(0,1,2....)
const btns = document.getElementsByTagName('button')
for (let i = 0; i < btns.length; i++) {
  btns[i].addEventListener('click', function () {
    console.log('第' + i + '个按钮被点击');
  })
}
```

### 详细解释

```js
// 1.情况一: ES5中没有使用闭包(错误的方式)
  i = 2
  {
    // i = 0
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }

  {
    i = 1
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }

  {
    // i = 2
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }

  // 2.情况二: ES5中使用闭包
  var btns = document.getElementsByTagName('button');
  for (var i=0; i<btns.length; i++) {
    (function (i) { // 0
      btns[i].addEventListener('click', function () {
        console.log('第' + i + '个按钮被点击');
      })
    }) (i)
  }

  i = 100000000
  function (i) { // i = 0
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }(0)

  function (i) { // i = 1
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }(1)

  function (i) { // i = 2
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }(2)

  // ES6中的let
  const btns = document.getElementsByTagName('button')
  for (let i = 0; i < btns.length; i++) {
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }
  i = 10000000
  { i = 0
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }

  { i = 1
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }

  { i = 2
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击');
    })
  }
```

## const(指向的内存地址不再改变)

- 1.const修饰的标识符被赋值之后, 不能修改

```js
const name = 'tadm';
name = 'liu'; //	Uncaught TypeError: Assignment to constant variable.
```

- 2.在使用const定义标识符,必须进行赋值

```js
const name = 'tadm';
const name;	//	Uncaught SyntaxError: Missing initializer in const declaration
```

- 3.常量的含义是指向的对象不能修改, 但是可以改变对象内部的属性.

```js
const info = {
  name: 'tadm',
  age: 19,
  height: 1.75
}
console.log(obj);

obj.name = 'liu';
obj.age = 20;
obj.height = 1.78;

console.log(obj);	//	change success
```

## 对象字面量增强写法

```js
const obj = new Object();
obj.xxx = xxx; 
...;

const obj = {
	name : 'tadm',
	study: function() { console.log('study') }
	//	函数增强写法
	study() { console.log('study') }
}
```

## 函数可变参数(...)

```js
function sum(...num){
	console.log(num)
}
sum(23,22,34,5,6)
//	(5) [23, 22, 34, 5, 6]0: 23 1: 22 2: 34 3: 5 4: 6 length: 5__proto__: Array(0)
```

## 模板字符串

```js
const name = 'tadm'
const name = `my test` //	接收空壳回车等
```

## More 待续

# 数组响应式方法

## push()

在数组最后面添加元素

```js
this.letters.push('aaa')
this.letters.push('aaaa', 'bbbb', 'cccc')
```

## pop()

删除数组中的最后一个元素

```js
this.letters.pop();
```

## shift()

删除数组中的第一个元素

```js
this.letters.shift();
```

## unshift()

在数组最前面添加元素

```js
this.letters.unshift()
this.letters.unshift('aaa', 'bbb', 'ccc')
```

## splice()

删除元素/插入元素/替换元素
第一个参数是startPostion
删除元素: 第二个参数传入要删除的元素个数(不传默认删除后面所有的元素)
替换元素: 第二个参数传入要替换的元素个数,接着后面是用于替换前面的元素(可以不等于替换元素个数,不推荐)
插入元素: 第二个参数, 传入0, 并且后面跟上要插入的元素

```js
splice(start)
splice(start)
this.letters.splice(1, 3, 'm', 'n', 'l', 'x')
this.letters.splice(1, 0, 'x', 'y', 'z')
```

## sort()

数组排序
this.letters.sort()

## reverse()

数组逆序
this.letters.reverse()

## 通过索引值修改数组中的元素

```js
this.letters[0] = 'm';	//	非响应式
this.letters.splice(0, 1, 'm')	// 响应式
set(被修改对象, 索引值, 修改后的值)
Vue.set(this.letters, 0, 'm')		// 响应式
Vue.delete(obj,'key')		// 响应式
```

# Cart Demo

![image.png](https://i.loli.net/2020/01/15/SdEjOm9WHh2InDp.png)

```html
<div id="app">
  <div v-if="books.length">
    <table>
      <thead>
      <tr>
        <th></th>
        <th>书籍名称</th>
        <th>出版日期</th>
        <th>价格</th>
        <th>购买数量</th>
        <th>操作</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(item, index) in books">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.date}}</td>
        <td>{{item.price | showPrice}}</td>
        <td>
          <button @click="decrement(index)" v-bind:disabled="item.count <= 1">-</button>
          {{item.count}}
          <button @click="increment(index)">+</button>
        </td>
        <td><button @click="removeHandle(index)">移除</button></td>
      </tr>
      </tbody>
    </table>
    <h2>总价格: {{totalPrice | showPrice}}</h2>
  </div>
  <h2 v-else>购物车为空</h2>
</div>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    books: [
      {
        id: 1,
        name: '《算法导论》',
        date: '2006-9',
        price: 85.00,
        count: 1
      },
      {
        id: 2,
        name: '《UNIX编程艺术》',
        date: '2006-2',
        price: 59.00,
        count: 1
      },
      {
        id: 3,
        name: '《编程珠玑》',
        date: '2008-10',
        price: 39.00,
        count: 1
      },
      {
        id: 4,
        name: '《代码大全》',
        date: '2006-3',
        price: 128.00,
        count: 1
      },
    ]
  },
  methods: {
    // getFinalPrice(price) {
    //   return '¥' + price.toFixed(2)
    // }
    increment(index) {
      this.books[index].count++
    },
    decrement(index) {
      this.books[index].count--
    },
    removeHandle(index) {
      this.books.splice(index, 1)
    }
  },
  computed: {
    totalPrice() {
      // 1.普通的for循环
      let totalPrice = 0
      for (let i = 0; i < this.books.length; i++) {
        totalPrice += this.books[i].price * this.books[i].count
      }

      // 2.for (let i in this.books)
      for (let i in this.books) {
        const book = this.books[i]
        totalPrice += book.price * book.count
      }

      // 3.for (let i of this.books)
      for (let item of this.books) {
        totalPrice += item.price * item.count
      }
      return totalPrice

      // 4.reduce
      return this.books.reduce(function (preValue, book) {
        return preValue + book.price * book.count
      }, 0)
    }
  },
  filters: {
    showPrice(price) {
      return '¥' + price.toFixed(2)
    }
  }
})
```

# 高阶函数

## filter()

filter中的回调函数有一个要求: 必须返回一个boolean值
true: 当返回true时, 函数内部会自动将这次回调的n加入到新的数组中
false: 当返回false时, 函数内部会过滤掉这次的n

```js
const nums = [10, 20, 111, 222, 444, 40, 50]
let newNums = nums.filter(function (n) {
  return n < 100
})
console.log(newNums);
```

## map()

```js
// 20, 40, 80, 100
let new2Nums = newNums.map(function (n) { // 20
  return n * 2
})
console.log(new2Nums);
```

## reduce()

```js
// reduce作用对数组中所有的内容进行汇总
let total = new2Nums.reduce(function (preValue, n) {
  return preValue + n
}, 0)
console.log(total);

第一次: preValue 0 n 20
第二次: preValue 20 n 40
第二次: preValue 60 n 80
第二次: preValue 140 n 100
240
```

## 整合

```js
let total = nums.filter(n => n < 100).map(n => n * 2).reduce(((pre, n) => pre + n),0);
console.log(total);

let total = nums.filter(function (n) {
  return n < 100
}).map(function (n) {
  return n * 2
}).reduce(function (prevValue, n) {
  return prevValue + n
}, 0)
console.log(total);
```

# Vue 组件化开发

## Explanation

https://cn.vuejs.org/v2/guide/components-registration.html
思想：任何的应用都会被抽象成一颗组件树,各个组件构建成完整的应用。

![](https://cn.vuejs.org/images/components.png)

应用：尽可能的将页面拆分成一个个小的、可复用的组件,这样让我们的代码更加方便组织和管理，并且扩展性也更强。

## 步骤

1. 创建组件构造器
2. 注册组件
3. 使用组件
   ![](https://i.loli.net/2020/01/15/uj4R8EOtxSgDcZF.png)

## 定义组件名

### 使用 kebab-case

```js
Vue.component('my-component-name', { /* ... */ })
```

当使用 kebab-case (短横线分隔命名) 定义一个组件时，你也必须在引用这个自定义元素时使用 kebab-case，例如 <my-component-name>

### 使用 PascalCase

```js
Vue.component('MyComponentName', { /* ... */ })
```

当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 <my-component-name> 和 <MyComponentName> 都是可接受的。注意，尽管如此，直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的。

## 全局组件 & 局部组件

### 全局组件

通过Vue.component()注册/定义组件,该组件可以在任意Vue示例下使用

### Example

```html
<div id="example">
  <!-- 2、 组件使用 组件名称 是以HTML标签的形式使用  -->  
  <my-component></my-component>
  <hello-world></hello-world>
</div>
```

```js
// 组件构造器
const cpnC = Vue.extend({
  template: `
    <div>
      <h2>我是标题</h2>
      <p>我是内容,哈哈哈哈啊</p>
    </div>
  `
})

//   注册组件 
// 1、 my-component 就是组件中自定义的标签名
Vue.component('my-component', {
	template: '<div>A custom component!</div>'
})
// 创建根实例
new Vue({
  el: '#example'
})

//	5  如果使用驼峰式命名组件，那么在使用组件的时候，只能在字符串模板中用驼峰的方式使用组件，
//	7、但是在普通的标签模板中，必须使用短横线的方式使用组件
Vue.component('HelloWorld', {
	data: function(){
	  return {
	    msg: 'HelloWorld'
	  }
	},
	//	子组件
	template: '<div>{{ msg }}</div>'
});
    
Vue.component('button-counter', {
  // 1、组件参数的data值必须是函数 
  // 同时这个函数要求返回一个对象  
  data() {
    return {
      count: 0
    }
  },
  //  2、组件模板必须是单个根元素
  //  3、组件模板的内容可以是模板字符串
  //  父组件
  template: `
    <div>
      <button @click="handle">点击了{{ count }}次</button>
      <button>测试</button>
		  // 在字符串模板中可以使用驼峰的方式使用组件	
	    <HelloWorld></HelloWorld>
    </div>`,
  methods: {
    handle: function(){
      this.count ++;
    }
  }
})
```

### 局部组件

只能在该实例下使用

### Example

```html
<div id="app">
  <my-component></my-component>
</div>
```

```js
// 定义组件的模板
var Child = {
  template: '<div>A custom component!</div>'
}
const app = new Vue({
  el: '#app',
  data: {
    message: 'hello tadm'
  },
  //局部注册组件  
  components: {
    // <my-component> 将只在父模板可用  一定要在实例上注册了才能在html文件中使用
    'my-component': Child
    'my-component': {
		  template: `<div>A custom component!</div>`,
		  data() { return { } },
		  methods: { }
		}
  }
})
```

## 组件模板分离写法(推荐)

```html
<template id="cpn">
  <div>
    <h2>我是标题</h2>
    <p>我是内容,呵呵呵</p>
  </div>
</template>
```

```js
Vue.component('cpn', {
  template: '#cpn'
})
const app = new Vue({ })
```

## 组件data必须为一个函数并返回

原因： 因为Vue需要给每个组件对象返回一个新对象(内存地址不一样)，避免各组件间相互影响

## 父子组件数据传递

需求：父组件发送网络请求，收到数据再将其传递至子组件并进一步操作

### 父组件-->子组件

#### 用法

子组件需要通过v-bind绑定指定数据给自己,同时在组件中通过props(properties-属性)接收相应数据

#### Example

```html
<div id="app">
  <!--<cpn v-bind:cmovies="movies"></cpn>-->
  <!--<cpn cmovies="movies" cmessage="message"></cpn>-->
  //	不使用v-bind则不会将其识别为变量而是直接将其当作字符串传过去

  <cpn :cmessage="message" :cmovies="movies"></cpn>
</div>

<!-- 定义 cpn 组件模板 -->
<template id="cpn">
  <div>
    <ul>
      <li v-for="item in cmovies">{{ item }}</li>
    </ul>
    <h2>{{ cmessage }}</h2>
  </div>
</template>
```

```js
//	定义 cpn 组件
const cpn = {
  template: '#cpn',
  // 	可以是一个数组
  // props: ['cmovies', 'cmessage'],
  // 也可以通过对象设置传入以及设置更多默认参数
  props: {
    // 1.类型限制
    // cmovies: Array,
    // cmessage: String,

    // 2.提供一些默认值, 以及必传值
    cmessage: {
      type: String,
      default: 'aaaaaaaa',
      required: true
    },
    // 类型是对象或者数组时, 默认值必须是一个函数
    cmovies: {
      type: Array,
      default() {
        return []
      }
    }
  },
  data() {
    return { }
  },
  methods: { }
}

const app = new Vue({
  el: '#app',
  data: {
    message: 'hello tadm',
    movies: ['海王', '海贼王', '海尔兄弟']
  },
  //	注册 cpn 组件
  components: {
  	//	'cpn': cpn
  	//	ES6 增强写法
    cpn
  }
})
```

#### props驼峰标识

##### 说明

和组件定义一样，在普通标签中只能使用短横线分割

##### example

```html
<div id="app">
  <cpn :c-info="info" :child-my-message="message"></cpn>
</div>

<template id="cpn">
  <div>
    <h2>{{ cInfo }}</h2>
    <h2>{{ childMyMessage }}</h2>
  </div>
</template>
```

```js
const cpn = {
  template: '#cpn',
  props: {
    cInfo: {
      type: Object,
      default() {
        return {}
      }
    },
    childMyMessage: {
      type: String,
      default: ''
    }
  }
}

const app = new Vue({
  el: '#app',
  data: {
    info: {
      name: 'tadm',
      age: 19,
      height: 1.75
    },
    message: 'hello tadm'
  },
  components: {
    cpn
  }
})
```

### 子组件-->父组件

#### 用法

子组件通过$emit()来发射事件以及参数,在父组件中通过@(v-on)来监听传过来的事件并进行相应处理(比如发送网络请求)

#### Example

```html
<!--父组件模板-->
<div id="app">
	<!-- 默认把$emit('function',item)中的item传给父组件,不会传递event(非浏览器对象自然不会产生event)-->
  <cpn @item-click="cpnClick"></cpn>
</div>

<!--子组件模板-->
<template id="cpn">
  <div>
    <button v-for="item in categories" @click="btnClick(item)">
      {{ item.name }}
    </button>
  </div>
</template>
```

```js
const cpn = {
	template: '#cpn',
	data() {
		return {
			categories: [
        {id: 'aaa', name: '热门推荐'},
        {id: 'bbb', name: '手机数码'},
        {id: 'ccc', name: '家用家电'},
        {id: 'ddd', name: '电脑办公'},
      ]
		}
	},
	methods: {
		btnClick(item) {
			this.$emit('item-click',item)
		}
	}
}
const app = new Vue({
	el: '#app',
	data: { },
	components: {
		//	'cpn': cpn
		cpn
	}
	methods: {
		cpnClick(item) {
			console.log('cpnClick',item)
		}
	}
})
```

### 父子组件双向绑定案例

#### v-model原型利用(v-bind,v-on)

```html
<div id="app">
  <cpn :number1="num1" :number2="num2"
       @num1change="num1change" @num2change="num2change"/>
</div>

<template id="cpn">
  <div>
    <h2>props:{{ number1 }}</h2>
    <h2>data:{{ dnumber1 }}</h2>
    <!--<input type="text" v-model="dnumber1">-->
    <input type="text" :value="dnumber1" @input="num1Input">
    <h2>props:{{ number2 }}</h2>
    <h2>data:{{ dnumber2 }}</h2>
    <!--<input type="text" v-model="dnumber2">-->
    <input type="text" :value="dnumber2" @input="num2Input">
  </div>
</template>
```

```js
const app = new Vue({
	el: '#app',
	data: {
		num1: 1,
		num2: 0
	},
	methods: {
		num1change(value) {
      this.num1 = parseFloat(value)
    },
    num2change(value) {
      this.num2 = parseFloat(value)
    }
	},
	components: {
		const cpn = {
			template: '#cpn',
			props: {
				number1: Number,
				number2: Number
			}
			data() {
				return {
					dnumber1: this.number1,
					dnumber2: this.number2
				}
			},
			methods: {
				num1Input(event) {
          // 1.将input中的value赋值到dnumber中
          this.dnumber1 = event.target.value;

          // 2.为了让父组件可以修改值, 发出一个事件
          this.$emit('num1change', this.dnumber1)

          // 3.同时修饰dnumber2的值
          this.dnumber2 = this.dnumber1 * 10;
          this.$emit('num2change', this.dnumber2);
        },
        num2Input(event) {
          this.dnumber2 = event.target.value;
          this.$emit('num2change', this.dnumber2)

          // 同时修饰dnumber2的值
          this.dnumber1 = this.dnumber2 / 10;
          this.$emit('num1change', this.dnumber1);
        }
			}
		}
	}
})
```

#### v-model现用

```html
<div id="app">
  <cpn :number1="num1" :number2="num2"
       @num1-change="num1change" @num2-change="num2change"/>
</div>

<template id="cpn">
  <div>
    <h2>props:{{ number1 }}</h2>
    <h2>data:{{ dnumber1 }}</h2>
    <input type="text" v-model="dnumber1">
    <h2>props:{{ number2 }}</h2>
    <h2>data:{{ dnumber2 }}</h2>
    <input type="text" v-model="dnumber2">
  </div>
</template>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    num1: 1,
    num2: 0
  },
  methods: {
    num1change(value) {
      this.num1 = parseFloat(value)
    },
    num2change(value) {
      this.num2 = parseFloat(value)
    }
  },
  components: {
    cpn: {
      template: '#cpn',
      props: {
        number1: Number,
        number2: Number,
        name: ''
      },
      data() {
        return {
          dnumber1: this.number1,
          dnumber2: this.number2
        }
      },
      watch: {
        dnumber1(newValue) {
          this.dnumber2 = newValue * 10;
          this.$emit('num1-change', newValue);
        },
        dnumber2(newValue) {
          this.dnumber1 = newValue / 10;
          this.$emit('num2-change', newValue);
        }
      }
    }
  }
})
```

### 父子组件直接访问

#### 父组件访问子组件

- 使用$children或$refs reference(引用)--->推荐
- this.$children是一个数组类型，它包含所有子组件对象，访问其中的子组件必须通过索引值(因此有很多子组件需要拿到其中一个时，往往不能确定它的索引值，甚至还可能会发生变化)
- 通常使用$refs来访问子组件：

```html
<div id="app">
  <cpn></cpn>
  <cpn></cpn>
  <cpn></cpn>
  <cpn ref="aaa"></cpn>
  <button @click="btnClick">按钮</button>
</div>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: '你好啊'
  },
  methods: {
    btnClick() {
      // 1.$children
      console.log(this.$children);
      for (let c of this.$children) {
        // console.log(c.name);
        c.showMessage();
      }
      console.log(this.$children[3].name);

      // 2.$refs => 对象类型, 默认是一个空的对象 ref='bbb'
      console.log(this.$refs);
      console.log(this.$refs.aaa.name);
    }
  },
  components: {
    cpn: {
      template: '#cpn',
      data() {
        return {
          name: '我是子组件的name'
        }
      },
      methods: {
        showMessage() {
          console.log('showMessage');
        }
      }
    },
  }
})
```

#### 子组件访问父组件(不常用)

- 使用$parent

#### 子组件访问根组件

- 使用$root

```html
<div id="app">
  <cpn></cpn>
</div>

<template id="cpn">
  <div>
    <h2>我是cpn组件</h2>
    <ccpn></ccpn>
  </div>
</template>

<template id="ccpn">
  <div>
    <h2>我是子组件</h2>
    <button @click="btnClick">按钮</button>
  </div>
</template>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'hello tadm'
  },
  components: {
    cpn: {
      template: '#cpn',
      data() {
        return {
          name: '我是cpn组件的name'
        }
      },
      components: {
        ccpn: {
          template: '#ccpn',
          methods: {
            btnClick() {
              // 1.访问父组件$parent
              console.log(this.$parent);  //  VueComponent {_uid: 1, _isVue: true, $options: {…}, _renderProxy: Proxy, _self: VueComponent, …}
              console.log(this.$parent.name);  // 我是cpn组件的name

              // 2.访问根组件$root
              console.log(this.$root);  //  Vue {_uid: 0, _isVue: true, $options: {…}, _renderProxy: Proxy, _self: Vue, …}
              console.log(this.$root.message);  //  hello tadm
            }
          }
        }
      }
    }
  }
})
```

# Vue slot 插槽

## 默认插槽使用

```html
<div id="app">
  <cpn></cpn>

  <cpn><span>哈哈哈</span></cpn>
  <cpn><i>呵呵呵</i></cpn>

  <cpn>
    <i>呵呵呵</i>
    <div>我是div元素</div>
    <p>我是p元素</p>
  </cpn>
</div>

<!-- 模板 -->
<template id="cpn">
  <div>
    <h2>我是组件</h2>
    <p>我是组件, 哈哈哈</p>
    <slot><button>按钮</button></slot>
    <!--<button>按钮</button>-->
  </div>
</template>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'hello tadm'
  },
  components: {
    cpn: {
      template: '#cpn',
    }
  }
})
```

## 具名插槽使用

```html
<div id="app">
  <cpn><span slot="center">标题</span></cpn>
  <cpn><button slot="left">返回</button></cpn>
</div>

<!-- 模板 -->
<template id="cpn">
  <div>
    <slot name="left"><span>左边</span></slot>
    <slot name="center"><span>中间</span></slot>
    <slot name="right"><span>右边</span></slot>
  </div>
</template>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'hello tadm'
  },
  components: {
    cpn: {
      template: '#cpn',
    }
  }
})
```

# Vue slot 插槽(2.6.0+)

## 默认插槽

### Example

```html
<navigation-link url="/profile">
  <!-- 添加一个 Font Awesome 图标 -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>

<!-- 模板 -->
<template id="nav-link">
  <div>
		<a :href="url" class="nav-link">
		  <slot></slot>
		</a>
	</div>
</template>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'hello tadm'
  },
  components: {
    navigation-link: {
      template: '#nav-link'
    }
  }
})
```

### 注意

- 如果 <navigation-link> 没有包含一个 <slot> 元素，则该组件起始标签和结束标签之间的任何内容都会被抛弃。
- 如果 <navigation-link> 中有多个标签，则会被同时插入到插槽中

## 具名插槽

### Example

```html
<!-- 模板 -->
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>

<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <!-- 默认插槽也可如下 -->
  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

### Result

```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

### 注意

v-slot 只能添加在 <template> 上 (<a href="https://cn.vuejs.org/v2/guide/components-slots.html#%E7%8B%AC%E5%8D%A0%E9%BB%98%E8%AE%A4%E6%8F%92%E6%A7%BD%E7%9A%84%E7%BC%A9%E5%86%99%E8%AF%AD%E6%B3%95">只有一种例外情况</a>)，这一点和已经废弃的 slot attribute不同。

## 编译作用域

### 官方准则

> 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

### Example

```html
<div id="app">
  <cpn v-show="isShow"></cpn>
  <!-- isShow: true -->
  <cpn v-for="item in names"></cpn>
</div>

<template id="cpn">
  <div>
    <h2>我是子组件</h2>
    <p>我是内容, 哈哈哈</p>
    <button v-show="isShow">按钮</button>
    <!-- isShow: false -->
  </div>
</template>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'hello tadm',
    isShow: true
  },
  components: {
    cpn: {
      template: '#cpn',
      data() {
        return {
          isShow: false
        }
      }
    },
  }
})
```

## 作用域插槽

由于插槽具有作用域，而作用域插槽使得子组件在父组件插槽能访问到子组件中的data并个性化使用数据(替换插槽默认处理方式)

### Example(废弃但便于理解)

```html
<div id="app">
  <cpn></cpn>

  <cpn>
    <!--目的是获取子组件中的pLanguages-->
    <template slot-scope="slot">
      <!--<span v-for="item in slot.data"> - {{item}}</span>-->
      <span>{{slot.data.join(' - ')}}</span>
    </template>
  </cpn>
</div>

<!-- 模板 -->
<template id="cpn">
  <div>
    <slot :data="pLanguages">
      <ul>
        <li v-for="item in pLanguages">{{item}}</li>
      </ul>
    </slot>
  </div>
</template>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: '你好啊'
  },
  components: {
    cpn: {
      template: '#cpn',
      data() {
        return {
          pLanguages: ['JavaScript', 'C++', 'Java', 'C#', 'Python', 'Go', 'Swift']
        }
      }
    }
  }
})
```

### 新版用法如下

https://cn.vuejs.org/v2/guide/components-slots.html#%E4%BD%9C%E7%94%A8%E5%9F%9F%E6%8F%92%E6%A7%BD

```html
<div id="app">
	<cpn></cpn>
	<cpn v-slot:text="myslotProps">
		<span>{{ myslotProps.data.join(' - ') }}</span>
			<!-- <span>{{ myslotProps.my.join(' - ') }}</span> -->
	</cpn>
</div>

<template id="cpn">
  <div>
    <slot name="text" :data="pLanguages">
    <!-- <slot name="text" :data="pLanguages" :my="tadm"> -->
    <!-- <slot name="text" :data=[pLanguages,tadm]> -->
      <ul>
        <li v-for="item in pLanguages">{{item}}</li>
      </ul>
    </slot>
  </div>
</template>
```

```js
const app = new Vue({
	el: '#app',
	data: { },
	methods: {

	},
	components: {
    cpn: {
      template: '#cpn',
      data() {
        return {
          pLanguages: ['JavaScript', 'C++', 'Java', 'C#', 'Python', 'Go', 'Swift']
        }
      }
    }
  }
})
```

###	注意

- slot-text 与 v-slot:text 保持一致(这里是 text )
- v-slot:text="myslotProps" 与 {{ myslotProps.data }} 保持一致(这里是 myslotProps)
- :data = "pLanguages" 与 {{ myslotProps.data }} 保持一致(这里是 data)
- :data = "pLanguages" 与 子组件中 data 里的数据名保持一致(这里是 pLanguages)

# 模块化开发

解决变量重名，导入JS顺序问题以及提高复用性

## 利用模块导出(了解)

```js
// a.js
var moduleA = ({
	var obj = {}
	obj.flag = true
	obj.func = function(name){
		console.log(name)
	}
	return obj
})()

// b.js
if(moduleA.flag){
	console.log('success')
}
moduleA.func('tadm')
```

## CommonJs(node/webpack)

### exprots

```js
// a.js
module.exports{
	variable,
	function(...){}
}
```

### import

```js
let {...} = require('./a.js')
flag = ....flag
if(flag){ do more }
```

## export && import

### html导入设置

```html
<script type="module" src="./a.js"></script>
<script type="module" src="./b.js"></script>
```

### export

```js
// a.js
// variable
export let name = 'tadm'

let name = 'tadm' 
export { name,... }

// function or class
export function mul(num1, num2) {
  return num1 * num2
}
export class Person {
  run() {
    console.log('coding');
  }
}

export { mul,Person,... }
```

### export default

export default在同一个模块中，不允许同时存在多个(唯一性,防止导入混乱)

```js
// b.js
const address = '苏州市'
export default address

export default function (argument) {
  console.log(argument);
}
```

### import

```js
// import from export
import { name,... } form "./a.js"
import { mul,Person,... } form "./a.js"

// import from export default
import addr from "./b.js"
console.log(addr)
import addrfunc from "./b.js"
addrfunc('tadm')

// import all
import * as all from './a.js'
console.log(all.flag)
all.mul(2,3)
```

# webpack(other: gulp/grunt)

## Explanation

Home: https://www.webpackjs.com/
Document: https://www.webpackjs.com/guides/
webpack是一个现代的JavaScript应用的静态模块打包工具

## VS Gulp/Grunt

### grunt/gulp 的核心是 Task

使用时可以配置一系列的task,并且定义task要处理的事务（例如ES6、ts转化，图片压缩，scss转成css）
之后让grunt/gulp来依次执行这些 task,而且让整个流程自动化。所以grunt/gulp也被称为前端自动化任务管理工具。

### 什么时候用grunt/gulp呢？

当工程模块依赖非常简单,甚至是没有用到模块化的概念,只需要进行简单的合并、压缩，就使用grunt/gulp即可。
但是如果整个项目使用了模块化管理，而且相互依赖非常强，我们就可以使用更加强大的webpack了。

### grunt/gulp 和 webpack 差异

grunt/gulp更加强调的是前端流程的自动化,模块化不是它的核心。
webpack更加强调模块化开发管理,而文件压缩合并、预处理等功能,是其附带的功能。

## Install

Install node.js first to use npm(node package manager)

```cmd
npm install --save-dev webpack
npm install --save-dev webpack@<version>

<!-- 指定3.6.0版本进行全局安装 -->
cnpm i webpack@3.6.0 -g

<!-- 更多需要局部安装 webpack 解决 version 不一致 -->
cnpm i webpack@3.6.0 --save-dev

<!-- check version -->
webpack --version
3.6.0
```

## webpack初使用

### commonJS

```js
// mathUtils.js
function add(num1, num2) {
  return num1 + num2
}
function mul(num1, num2) {
  return num1 * num2
}

module.exports = {
  add,
  mul
}
```

### ES6

```js
// info.js
export const name = 'tadm';
export const age = 19;
export const height = 1.75;

export { name,age,height }
```

### import

```js
// main.js
// 1.使用commonjs的模块化规范
const {add, mul} = require('./mathUtils.js')

console.log(add(2, 3),mul(2, 3));

// 2.使用ES6的模块化的规范
import {name, age, height} from "./info";

console.log(name,age,height);
```

### webpack package

```cmd
webpack src/main.js dist/bundle.js
```

### HTML Use

```html
<script type="text/javascript" src="./dist/bundle.js"></script>
```

## webpack Config

### npm init

生成 package.json (about pakage info)

```json
{
  "name": "meetwebpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "tadm",
  "license": "ISC",
  // 开发时依赖
  "devDependencies": {
    "webpack": "^3.6.0"
  }
}
```

### Create webpack.config.js

```js
const path = require('path')

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),	//	绝对路径
    filename: 'bundle.js'
  },
}
```

接着 npm run build 即可完成打包

### 注意

package.json中的scripts的脚本在执行时，会按照一定的顺序寻找命令对应的位置。
首先，会寻找本地的node_modules/.bin路径中对应的命令。
如果没有找到，会去全局的环境变量中寻找

## loader(webpack转换某些类型的模块,一个转换器)

Documention: https://www.webpackjs.com/loaders/

### Install(css-loader && style-loader为例)

style-loader 将模块的导出作为样式添加到 DOM 中
css-loader 解析 CSS 文件后，使用 import 加载，并且返回 CSS 代码
url-loader 像 file loader 一样工作，但如果文件小于限制，可以返回 data URL

```cmd
cnpm install --save-dev css-loader
cnpm install style-loader --save-dev
cnpm install --save-dev url-loader
cnpm install --save-dev file-loader
cnpm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```

### Use css-loader && style-loader

将css加入依赖并配置webpack.config.js

```js
import css from 'file.css';
// 3.依赖css文件
require('./css/normal.css')
require('./css/back.less')

import img from './image.png'
```

```js
module.exports = {
	entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),	//	绝对路径
    filename: 'bundle.js',
    publicPath: 'dist/'		//	使利用url-loader加载的文件打包到dist下,不加webpack会将生成的路径直接返回给使用者
  },
  module: {
    rules: [
      {
        test: /\.css$/,	//	以.css结尾的文件
        // 使用多个loader时, 是从右向左的顺序
        use: [ 'style-loader', 'css-loader' ]
      },
      {
        test: /\.less$/,
        use: [{
          loader: "style-loader", // creates style nodes from JS strings
        }, {
          loader: "css-loader" // translates CSS into CommonJS
        }, {
          loader: "less-loader", // compiles Less to CSS
        }]
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
            	// 当加载的图片, 小于limit时, 会将图片编译成base64字符串形式.
              // 当加载的图片, 大于limit时, 需要使用file-loader模块进行加载.
              limit: 8192,
       				// img：文件要打包到的文件夹
							// name：获取图片原来的名字，放在该位置
							// hash:8：为了防止图片名称冲突，依然使用hash，但是我们只保留8位
							// ext：使用图片原来的扩展名
              name: 'img/[name].[hash:8].[ext]'
            }
          }
        ]
      },
      {
	      test: /\.js$/,
	      exclude: /(node_modules|bower_components)/,
	      use: {
	        loader: 'babel-loader',
	        options: {
	          presets: ['@babel/preset-env']
	        }
	      }
	    }
    ]
  }
}
```

## Plugin(对webpack本身的扩展,一个扩展器)

### 使用方法

步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要安装)
步骤二：在webpack.config.js中的plugins中配置插件

### 内置插件实例

```js
// webpack.config.js
const webpack = require('webpack')

plugins: [
  new webpack.BannerPlugin('最终版权归Tadm所有')
]
```

Result:在bundle.js文件头部生成对应版权信息

### 新装插件

#### html打包

```cmd
cnpm install html-webpack-plugin --save-dev
```

```js
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin')
// 此时不再需要 publicPath: 'dist/'
plugins: [
  new HtmlWebpackPlugin({
    template: 'index.html'
  })
]
```

#### JS压缩(打包时)

```cmd
<!-- 指定版本与 vue-cli 保持一致 -->
cnpm install uglifyjs-webpack-plugin@1.1.1 --save-dev
```

```js
// webpack.config.js
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
// 此时不再需要 publicPath: 'dist/'
plugins: [
  new UglifyjsWebpackPlugin()
]
```

## Webpack Config Vue

### Install

```cmd
cnpm install vue --save
<!-- 加 'dev' 是开发时依赖，正式打包运行时不需要,但是若要开发和运行时都使用则用 '--save' -->
```

### Primary Use

```js
// main.js
import Vue from 'vue'

new Vue({
	el: '#app',
	data: { ... }
})
```

```html
<!-- index.html -->
<div id="app">{{ ... }}</div>
```

### maybe some errors

Vue 构建版本 runtime-only(不解析template[vue实例就是一个template]) 和 runtime-compiler
Resolve: add something in webpack.config.js

```js
// webpack.config.js
resolve: {
	alias: {
		// alias: 别名
    extensions: ['.js', '.css', '.vue'],
		'vue$': 'vue/dist/vue.esm.js'	//	构建版本指定为runtime-compiler
	}
}
```

### Ultimate Use

#### Install vue-loader(解析.vue文件)

```cmd
cnpm install vue-loader vue-template-compiler --save-dev
```

#### Use

add something in webpack.config.js

```js
// webpack.config.js
module: {
	rules: [
		{
      test: /\.vue$/,
      use: ['vue-loader']
    }
	]
}
```

```html
<div id="app">
</div>

<script src="./dist/bundle.js"></script>
```

```js
// main.js
import Vue from 'vue'
import App from './vue/App.vue'

new Vue({
  el: '#app',
  template: '<App/>',
  components: {
  	// 注册App组件
    App
  }
})
```

```vue
<!-- App.vue -->
<template>
  <div>
    <h2 class="title">{{ message }}</h2>
    <button @click="btnClick">按钮</button>
    <h2>{{ name }}</h2>
    <Cpn/>
  </div>
</template>

<script>
	<!-- 再导入Cpn作为子组件,此处定义略 -->
  import Cpn from './Cpn'

  export default {
    name: "App",
    data() {
      return {
        message: 'Hello Webpack in Vue',
        name: 'tadm'
      }
    },
    methods: {
      btnClick() { doSomething }
    },
    components: {
    	Cpn
    },
  }
</script>

<style scoped>
  .title {
    color: aqua;
  }
</style>
```

## webpack 搭建本地服务器

### Install

```cmd
cnpm install --save-dev webpack-dev-server@2.9.1
```

```js
// webpack.config.js
module.exports={
	devServer: {
    contentBase: './dist',	//	默认 根文件夹
    port: 8000,		//	默认 8080
    inline: true
  }
}
```

```json
// package.json
"scripts": {
    "dev": "webpack-dev-server --open"
  }
```

## webpack 配置文件分离

### base.config.js

```js
const path = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  entry: './src/main.js',
  output: {
  	// 此时配置文件路径发生变化需要重新配置
    path: path.resolve(__dirname, '../dist'),
    filename: 'bundle.js',
    // publicPath: 'dist/'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        // 使用多个loader时, 是从右向左
        use: [ 'style-loader', 'css-loader' ]
      },
      {
        test: /\.less$/,
        use: [{
          loader: "style-loader", // creates style nodes from JS strings
        }, {
          loader: "css-loader" // translates CSS into CommonJS
        }, {
          loader: "less-loader", // compiles Less to CSS
        }]
      },
      {
        test: /\.(png|jpg|gif|jpeg)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              // 当加载的图片, 小于limit时, 会将图片编译成base64字符串形式.
              // 当加载的图片, 大于limit时, 需要使用file-loader模块进行加载.
              limit: 13000,
              name: 'img/[name].[hash:8].[ext]'
            },
          }
        ]
      },
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['es2015']
          }
        }
      },
      {
        test: /\.vue$/,
        use: ['vue-loader']
      }
    ]
  },
  resolve: {
    // alias: 别名
    extensions: ['.js', '.css', '.vue'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    }
  },
  plugins: [
    new webpack.BannerPlugin('最终版权归Tadm所有'),
    new HtmlWebpackPlugin({
      template: 'index.html'
    })
  ]
}
```

### dev.config.js

```js
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')

module.exports = webpackMerge(baseConfig, {
  devServer: {
    contentBase: './dist',
    inline: true
  }
})
```

### pro.config.js

```js
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')

module.exports = webpackMerge(baseConfig, {
  plugins: [
    new UglifyjsWebpackPlugin()
  ]
})
```

### package.json

```json
"scripts": {
	// 默认以 webpack.config.js 作为配置文件所以需要重新指定配置文件
  "build": "webpack --config ./build/prod.config.js",
  "dev": "webpack-dev-server --open --config ./build/dev.config.js"
}
```

### 基础文件与开发配置文件或者生产配置文件合并

webpack-merge('./baseConfigFile',{ devConfig/proConfig })

# Vue-Cli 脚手架

## Runtime-Compiler和Runtime-only的区别(理解原理)

### 简单总结

- 如果在之后的开发中，依然使用template，就需要选择Runtime-Compiler
- 如果你之后的开发中，使用的是.vue文件开发，那么可以选择Runtime-only

### Vue 解析运行过程

![Vue 解析运行过程](https://i.loli.net/2020/01/17/s4YWQKXtPAN25VR.png)

- parse---解析
- ast---abstract syntax tree---抽象语法树
- compile---编译

- runtime-compiler(v1)
  - template -> ast -> render -> virtual Dom -> UI

- runtime-only(v2)(lighter)(1.性能更高 2.下面的代码量更少)
  - render -> vdom -> UI

### Example

```js
// 	main.js
import Vue from 'vue'
import App from './App'

// 	runtime-compiler
new Vue({
  el: '#app',
  template: '<App/>',
  components: { App }
})

// 	runtime-only
new Vue({
  el: '#app',
  render: function (h) {
    return h(App)
  }
})
```

### render 函数使用

```js
import Vue from 'vue'
import App from './App'

console.log(App)	//	App content(include render)
new Vue({
  el: '#app',
  render: function (createElement) {
    // 1.普通用法: createElement('标签', {标签的属性}(可选), ['内容数组'])
    return createElement('h2',
      {class: 'box'},
      ['Hello World', createElement('button', ['按钮'])])

    // 2.传入组件对象:
    return createElement(App)
  }
})

new Vue({
  el: '#app',
  render: function (h) {
    return h(App)
  }
})

//  Lastest
new Vue({
  render: h => h(App)
}).$mount('#app')
```

### Explanation

Runtime-only 比 Runtime-Compiler 省掉了template --> ast --> render
App.vue 文件中的 template 是由("vue-template-compiler": "^2.5.2") 所编译(可参照前文vue文件的解析)


## Install && Create

```cmd
cnpm install -g @vue/cli

<!-- 命令行 -->
vue create my-vue-cli

<!-- web ui 配置 -->
vue ui
```

https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create

## Args Explanation

![Args Explanation](https://i.loli.net/2020/01/17/eQRZyhjOYG75L8l.png)

## Directory Structure

![Directory Structure](https://i.loli.net/2020/01/17/yrxBNFDklQwnvUa.png)

## Config

- 新版 vue-cli 已经省略掉配置文件夹(争取0配置)
  - 若需要查看配置则可进入 node_modules/@vue/cli-service 来浏览
  - 若需要添加更改配置,则在根目录添加 vue.config.js 文件进行相应配置

```js
	module.exports = { }
```

## Use

```cmd
npm run serve
npm run build
```

# 箭头函数

## Use

```js
// 1.1.无参
const demo = () => {
  console.log('Hello Demo');
}

// 1.2.放入一个参数
const power = num => {
  return num * num
}

// 1.3.放入两个参数或者多个
const sum = (num1, num2, ...) => {
  return num1 + num2 + ...
}

// 2.1.函数代码块中只有一行代码
const mul = (num1, num2) => num1 * num2

// 2.2.函数代码块中有多行代码时
const test = () => {
  console.log('Hello World');
  console.log('Hello Vuejs');
}

const demo = () => console.log('Hello Demo')
console.log(demo());	//	undefined
```

## 箭头函数---this

本身无this,所以需要向外层作用域中, 一层层查找this, 直到有this的定义.

```js
setTimeout(function () {
  console.log(this);	// window
}, 1000)

this-->window

setTimeout(() => {
  console.log(this);	// window
}, 1000)

const obj = {
  aaa() {
    setTimeout(function () {
      console.log(this); // window
    })

    setTimeout(() => {
      console.log(this); // obj对象
    })
  }
}
obj.aaa()

const obj = {
  aaa() {
  	// setTimeout 第一个是要执行的代码或函数,第二个是延迟的时间.
    setTimeout(function () {
      setTimeout(function () {
        console.log(this); // window
      })

      setTimeout(() => {
        console.log(this); // window
      })
    })

    setTimeout(() => {
      setTimeout(function () {
        console.log(this); // window
      })

      setTimeout(() => {
        console.log(this); // obj
      })
    })
  }
}
```

## 注意

> setTimeout中延迟执行函数中的this,永远指向window(setTimeout中箭头函数上面已经讲述)
> 超时调用的代码都是在全局作用域中执行的，因此函数中this的值在非严格模式下指向window对象,在严格模式下是undefined

javascript 事件执行顺序讲解：http://www.ruanyifeng.com/blog/2014/10/event-loop.html

> setTimeout(fn,0)的含义是，指定某个任务在主线程最早可得的空闲时间执行，也就是说，尽可能早得执行。
> 它在"任务队列"的尾部添加一个事件(立即插入队列，但不是立即执行),因此要等到同步任务和"任务队列"现有的事件都处理完,才会得到执行。

# Vue-Router

Document: https://router.vuejs.org/zh/guide/#html

## web路由发展

### 后端路由

![01-后端路由阶段.png](https://i.loli.net/2020/01/17/8sWwoECl2vD3Znz.png)

### 前后端分离

![02-前端后端分离阶段.png](https://i.loli.net/2020/01/17/Jy1RZNa9LvszdSV.png)

### SPA页面

![03-SPA页面页面的阶段.png](https://i.loli.net/2020/01/17/ns2DKiv5kdaXOub.png)

### 前端路由

![04-前端路由中url和组件的关系.jpg](https://i.loli.net/2020/01/17/ym8Fg9a7eURWn6v.jpg)

## Hash 模式

URL的hash也就是锚点(#), 本质上是改变window.location的href属性.(href---hyper reference)
我们可以通过直接赋值location.hash来改变href, 但是页面不发生刷新

## HTML5 History 模式(改变URL而不刷新页面)

history.pushState()---(一个状态对象, 一个标题 (目前被忽略), 和 (可选的) 一个URL)
history.replaceState()
history.go()---(无参---refresh or 0 or 1 or -1)
history.back() 等价于 history.go(-1)
history.forward() 则等价于 history.go(1)

## Use

```js
//	router/index.js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component: Home
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = new VueRouter({
  mode: 'history',	//	or hash
  base: process.env.BASE_URL,
  routes
})

export default router
```

```js
//	main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```

```vue
<!-- Home.vue -->
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  components: {
    HelloWorld
  }
}
</script>
```

```vue
<!-- App.vue -->
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style scopped>
</style>
```

## router-link 属性

Document: https://router.vuejs.org/zh/api/#to

- to: 用于指定跳转的路径.
- tag: tag可以指定<router-link>之后渲染成什么组件(tag = 'button')
- replace: replace不会留下history记录,后退键返回不能返回到上一个页面中
- active-class: 当<router-link>对应的路由匹配成功时, 会自动给当前元素设置一个router-link-active的class, 设置active-class可以修改默认的名称

如果需要所有router-link应用active class:

```js
const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  linkActiveClass: 'aqua',
  routes
})
```

## 路由跳转

```js
this.$router.push('/main')
this.$router.replace('/main')
```

## 动态路由(拼接参数)

Document: https://router.vuejs.org/guide/essentials/dynamic-matching.html

![](https://i.loli.net/2020/01/18/s8XgniwQK7yzqHA.png)

## 路由懒加载(访问才加载)

### 用途

避免所有JS文件都打包到app.bla.js中影响用户体验

### 写法

方式一: 结合Vue的异步组件和Webpack的代码分析.

```js
const Home = resolve => { 
	require.ensure(['../components/Home.vue'], () => { resolve(require('../components/Home.vue')) })
};
```

方式二: AMD写法

```js
const About = resolve => require(['../components/About.vue'], resolve);
```

方式三: ES6写法(recommend)

```js
const Home = () => import('../components/Home.vue')
```

![router-lazy-loader](https://i.loli.net/2020/01/18/vd8tUAezZqmEh3u.png)

## 嵌套路由

Document: https://router.vuejs.org/zh/guide/essentials/nested-routes.html

```js
//	lazy-loader
const Home = () => import('../components/Home')
const HomeNews = () => import('../components/HomeNews')
const HomeMessage = () => import('../components/HomeMessage')

const routes = [
{
  path: '',
  // redirect重定向
  redirect: '/home'
},
{
  path: '/home',
  component: Home,
  meta: {
    title: '首页'
  },
  children: [
    {
      path: '',
      redirect: 'news'
    },
    {
      path: 'news',
      component: HomeNews
    },
    {
      path: 'message',
      component: HomeMessage
    }
  ]
}
```

```vue
<template>
  <div>
    <h2>我是首页</h2>
    <p>我是首页内容, 哈哈哈</p>

    <router-link to="/home/news">新闻</router-link>
    <router-link to="/home/message">消息</router-link>

    <router-view></router-view>

    <h2>{{ message }}</h2>
  </div>
</template>
```

## 路由传递参数

### Explanation

- params (Object) (this.$route.params):

配置路由格式: /router/:id
传递的方式: 在path后面跟上对应的值
传递后形成的路径: /router/123, /router/abc

```html
<router-link v-bind:to="'/user/'+userId"> user </router-link>
<p>{{ this.$route.params }}</p>
```

- query (Object) (this.$route.query):

配置路由格式: /router, 也就是普通配置
传递的方式: 对象中使用query的key作为传递方式
传递后形成的路径: /router?id=123, /router?id=abc

```html
<router-link v-bind:to="{ path: '/profile/',query: {name:'tadm',...} }"> profile </router-link>
<p>{{ this.$route.query }}</p>
```

or

```js
export default {
	name: 'App',
	methods: {
		toProfile() {
			this.$router.push({
				path: '/profile/',
				query: { name:'tadm',... }
			})
		}
	}
}
```

### route vs router

- $router为VueRouter实例，想要导航到不同URL，则使用$router.push方法
- $route为当前router跳转对象里面可以获取name、path、query、params等 

## 路由导航守卫

Document: https://router.vuejs.org/zh/guide/advanced/navigation-guards.html

```js
// 前置守卫(guard)
router.beforeEach((to, from, next) => {
  // 从from跳转到to
  document.title = to.matched[0].meta.title
  // console.log(to);
  next()
})

// 后置钩子(hook)
router.afterEach((to, from) => {
  // console.log('----');
})
```

## Keep-Alive

- keep-alive 是 Vue 内置的一个组件,可以使被包含的组件保留状态,或避免重新渲染(即不会在离开或者进入时频繁调用created/destoryed)
  - include - 字符串或正则表达，只有匹配的组件会被缓存
  - exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存
- router-view 也是一个组件，如果直接被包在 keep-alive 里面，所有路径匹配到的视图组件都会被缓存

```js
<keep-alive exclude="Profile,User">
	<router-view/>
</keep-alive>
```

下面两个函数(activated/deactivated), 只有该组件被保持了状态使用了keep-alive时, 才是有效的

```js
activated() {	//	活跃
  this.$router.push(this.path);
  console.log('activated');
},
deactivated() {	//	离开
  console.log('deactivated');
},	//	记录离开前路由
beforeRouteLeave (to, from, next) {
  console.log(this.$route.path);
  this.path = this.$route.path;
  next()
}
```

# TabBar 实例

Github: https://github.com/Liuhongwei3/TabBarExample

# Promise

## Explanation

Promise是异步编程的一种解决方案(太笼统了)
http://es6.ruanyifeng.com/#docs/promise

More(update: 2020年1月25日16:52:30)：

1. Promise 是一个**构造函数**，既然是构造函数， 那么，我们就可以  new Promise() 得到一个 Promise 的实例；
2. 在 Promise 上，有两个函数，分别叫做 resolve（成功之后的回调函数） 和 reject（失败之后的回调函数）
3. 在 Promise 构造函数的 **Prototype** 属性上，有一个 **.then()** 方法，也就说，只要是 Promise 构造函数创建的实例，都可以访问到 .then() 方法
4. Promise 表示一个 异步操作；每当我们 new 一个 Promise 的实例，这个实例，就表示一个具体的异步操作；
5. 既然 Promise 创建的实例，是一个异步操作，那么，这个 异步操作的结果，只能有两种状态：
   5.1 状态1： 异步**执行成功**了，需要在内部调用 成功的回调函数 **resolve** 把结果返回给调用者；
   5.2 状态2： 异步**执行失败**了，需要在内部调用 失败的回调函数 **reject** 把结果返回给调用者；
   5.3 由于 Promise 的实例，是一个异步操作，所以，内部拿到 操作的结果后，无法使用 return 把操作的结果返回给调用者； 这时候，只能使用回调函数的形式，来把 成功 或 失败的结果，返回给调用者；
6. 我们可以在 new 出来的 Promise 实例上，调用 .then() 方法，【预先】 为 这个 Promise 异步操作，指定 成功（resolve） 和 失败（reject） 回调函数；

## Example

```js
new Promise((resolve, reject) => {
  setTimeout(() => {
    // 成功的时候调用resolve
    resolve('Hello World')
    // 失败的时候调用reject
    // reject('error message')
  }, 1000)
}).then((data) => {
  // 1.100行的处理代码
  console.log(data);	//	Hello World
}).catch((err) => {
  console.log(err);	//	error message
})

new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Hello Vuejs')
    // reject('error message')
  }, 1000)
}).then(data => {
  console.log(data);
}, err => {
  console.log(err);
})
```

## 链式调用

```js
// 参数 -> 函数(resolve, reject)
// resolve, reject本身它们又是函数
// 链式编程
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa')
  }, 1000)
}).then(res => {
  // 1.自己处理10行代码
  console.log(res, '第一层的10行处理代码');

  // 2.对结果进行第一次处理
  return new Promise((resolve, reject) => {
    resolve(res + '111')
    // reject('err')
  })
}).then(res => {
  console.log(res, '第二层的10行处理代码');

  return new Promise(resolve => {
    resolve(res + '222')
  })
}).then(res => {
  console.log(res, '第三层的10行处理代码');
}).catch(err => {   // 捕获某一层抛出/发生的异常
  console.log(err);
})

// new Promise(resolve => resolve(结果))简写
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa')
  }, 1000)
}).then(res => {
  // 1.自己处理10行代码
  console.log(res, '第一层的10行处理代码');

  // 2.对结果进行第一次处理
  return Promise.resolve(res + '111')
}).then(res => {
  console.log(res, '第二层的10行处理代码');

  return Promise.resolve(res + '222')
}).then(res => {
  console.log(res, '第三层的10行处理代码');
})

// 省略掉Promise.resolve
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa')
  }, 1000)
}).then(res => {
  // 1.自己处理10行代码
  console.log(res, '第一层的10行处理代码');

  // 2.对结果进行第一次处理
  return res + '111'
}).then(res => {
  console.log(res, '第二层的10行处理代码');

  return res + '222'
}).then(res => {
  console.log(res, '第三层的10行处理代码');
})


new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa')
  }, 1000)
}).then(res => {
  // 1.自己处理10行代码
  console.log(res, '第一层的10行处理代码');

  // 2.对结果进行第一次处理
  // return Promise.reject('error message')
  throw 'error message'
}).then(res => {
  console.log(res, '第二层的10行处理代码');

  return Promise.resolve(res + '222')
}).then(res => {
  console.log(res, '第三层的10行处理代码');
}).catch(err => {
  console.log(err);
})
```

## all method

```js
Promise.all([
	new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({name: 'why', age: 18})
    }, 2000)
  }),
  new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({name: 'kobe', age: 19})
    }, 1000)
  })
]).then(results => {
  console.log(results); //  Array
})
```

# Async await

(update: 2020年1月25日16:44:54)
比较详细：https://segmentfault.com/a/1190000007535316#item-1

# Vuex

Document: https://vuex.vuejs.org/zh/

## 单页面状态管理

![](https://vuex.vuejs.org/flow.png)

### Args

- state，驱动应用的数据源；
- view，以声明方式将 state 映射到视图；
- actions，响应在 view 上的用户输入导致的状态变化。

```js
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
```

## 多页面状态管理

![](https://vuex.vuejs.org/vuex.png)
单一状态树--->一个项目对应一个 store

### state

```js
state: {
  count: 0,
  students: [
    {id: 110, name: 'why', age: 18},
    {id: 111, name: 'kobe', age: 24},
    {id: 112, name: 'james', age: 30},
    {id: 113, name: 'curry', age: 10}
  ]
}

this.$store.state.count 	//	访问 state 中的 count
```

### getters(as computed 数据需要做处理再返回给组件使用)

> Getter 接受 state 作为其第一个参数,也可以接受其他 getter 作为第二个参数,也可以通过让 getter 返回一个函数，来实现给 getter 传参:

```js
getters: {
	more20stu(state) {
    return state.students.filter(s => s.age > 20)
  },
  //  利用 getters 中已处理的数据进行再加工
  more20stuLength(state, getters) {
    return getters.more20stu.length
    //  Of course but not recommand
    //  return state.students.filter(s => s.age > 20).length
  },
  // 	传参(需要返回一个函数进行处理)
  moreAgeStu(state) {
    return function (age) {
      return state.students.filter(s => s.age > age)
    }
    // return age => {
    //   return state.students.filter(s => s.age > age)
    // }
  }
}

this.$store.getters.more20stu
this.$store.getters.more20stuLength
this.$store.getters.moreAgeStu(12)
```

### mutations(状态更新)

> 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation
> Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler);这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
mutations: {
  increment (state) {
    // 变更状态
    state.count++
  },
  incrementCount(state, count) {
    console.log(count);
    state.count += count
  },
  incrementCount(state, payload) {
    console.log(payload);  //		Object
    state.count += payload.count
  },
  addStudent(state, stu) {
    state.students.push(stu)
  }
}

methods: {
  addition() {
    this.$store.commit(increment)
  },
  addCount(count) {
    // payload: 负载
    // 1.普通的提交封装
    this.$store.commit('incrementCount', count)

    // 2.特殊的提交封装
    this.$store.commit('incrementCount', {
		  count: 10
		})
    this.$store.commit({
      type: 'incrementCount',
      count
    })
  },
  addStudent() {
    const stu = { id: 114, name: 'alan', age: 28 }
    this.$store.commit('addStudent', stu)
  },
  updateInfo(state) {
    state.info.name = 'tadm'
  }
}

<h2>{{ $store.state.count }}</h2>
<button @click="addition">+</button>
<button @click="addCount(5)">+5</button>

<button @click="addStudent">Add Student</button>
```

- 一条重要的原则就是要记住 mutation 必须是同步函数(异步会使得devtools无法正常跟踪)

```js
mutations: {
	asyncUpdate(state, payload){
		setTimeout(() => {
			state.info = { ...state.info, 'name': payload.name }
		}, 1000)
	}
}
```

- Mutation 需遵守 Vue 的响应规则:
  - 既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。
  - 最好提前在你的 store 中初始化好所有所需属性。
  - 当需要在对象上添加新属性时，你应该使用 ```Vue.set(obj, 'newProp', 123)```, 或者以新对象替换老对象。例如，利用对象展开运算符我们可以这样写：

```js
state.obj = { ...state.obj, newProp: 123 }
```

> mutation 常量类型

```js
export const INCREMENT = 'increment'

import { INCREMENT } from "./mutations-types";
mutations: {
	[INCREMENT](state) {
    state.counter++
  }
}
addition() {
  this.$store.commit(INCREMENT)
}
```

### actions

> Action 类似于Mutation, 但是是用来代替Mutation进行异步操作
> Action 提交的是 mutation，而不是直接变更状态。

```js
actions: {
	increment (context) {
    context.commit('increment')
  }
}

this.$store.dispatch('increment')

// Actions.js
aUpdateInfo(context, payload) {
  setTimeout(() => {
    context.commit('updateInfo')
    console.log(payload.message);
    payload.success()
  }, 1000)
},
aUpdateInfo(context, payload) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      context.commit('updateInfo');
      console.log(payload);

      resolve('1111111')
    }, 1000)
  })
}

methods:{
	updateInfo() {
	  this.$store.dispatch('aUpdateInfo', {
	    message: '我是携带的信息',
	    success: () => {
	      console.log('里面已经完成了');
	    }
	  })
	  this.$store
	    .dispatch('aUpdateInfo', '我是携带的信息')
	    .then(res => {
	      console.log('里面完成了提交');
	      console.log(res);
	    })
	}
}
```

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters

### modules

Document: https://vuex.vuejs.org/zh/guide/modules.html

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。
为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割

```js
const moduleA = {
  state: { ... },
  getters: { ... },
  mutations: { ... },
  actions: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
	state,
  mutations,
  actions,
  getters,
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态

getters/mutations 依然是通过 this.$store 来调用(use 局部 state)
// 模块内部的 getter，根节点状态会作为第三个参数暴露出来(rootState)
getters: {
  sumWithRootCount (state, getters, rootState) {
    return state.count + rootState.count
  }
}

Actions 
//	局部状态通过 context.state 暴露出来，根节点状态则为 context.rootState
const moduleA = {
  // ...
  actions: {
  	// 	ES6 解构赋值：{ state, commit, rootState } = context
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

> 当 vuex 中内容过多我们可以选择将 state/getters/mutations 等抽取成单独的 js 文件 export default 再 import 放入 store 对象中并挂载到实例中

# 变量解构赋值

Document: http://es6.ruanyifeng.com/#docs/destructuring

# Axios

Document: http://www.axios-js.com/zh-cn/docs/

## axios的基本使用

```js
import axios from 'axios'

axios({
  url: 'http://123.207.32.32:8000/home/multidata',
  // method: 'post'
}).then(res => {
  console.log(res);
})

axios({
  url: 'http://123.207.32.32:8000/home/data',
  // 专门针对get请求的参数拼接
  params: {
    type: 'pop',
    page: 1
  }
}).then(res => {
  console.log(res);
})
```

## axios发送并发请求

```js
axios.all([axios({
  url: 'http://123.207.32.32:8000/home/multidata'
}), axios({
  url: 'http://123.207.32.32:8000/home/data',
  params: {
    type: 'sell',
    page: 5
  }
})]).then(results => {
  console.log(results);
  console.log(results[0]);
  console.log(results[1]);
})
```

## 使用全局的axios和对应的配置在进行网络请求

```js
axios.defaults.baseURL = 'http://123.207.32.32:8000'
axios.defaults.timeout = 5000

axios.all([axios({
  url: '/home/multidata'
}), axios({
  url: '/home/data',
  params: {
    type: 'sell',
    page: 5
  }
})]).then(axios.spread((res1, res2) => {
  console.log(res1);
  console.log(res2);
}))
```

## 创建对应的axios的实例

```js
const instance1 = axios.create({
  baseURL: 'http://123.207.32.32:8000',
  timeout: 5000
})

instance1({
  url: '/home/multidata'
}).then(res => {
  console.log(res);
})

instance1({
  url: '/home/data',
  params: {
    type: 'pop',
    page: 1
  }
}).then(res => {
  console.log(res);
})


const instance2 = axios.create({
  baseURL: 'http://222.111.33.33:8000',
  timeout: 10000,
  // headers: {}
})
```

## 封装request模块

```js
export function request(config) {
  return new Promise((resolve, reject) => {
    // 1.创建axios的实例
    const instance = axios.create({
      baseURL: 'http://123.207.32.32:8000',
      timeout: 5000
    })

    // 发送真正的网络请求
    instance(config)
      .then(res => {
        resolve(res)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export function request(config) {
  // 1.创建axios的实例
  const instance = axios.create({
    baseURL: 'http://123.207.32.32:8000',
    timeout: 5000
  })

  // 发送真正的网络请求
  instance(config.baseConfig)
    .then(res => {
      // console.log(res);
      config.success(res);
    })
    .catch(err => {
      // console.log(err);
      config.failure(err)
    })
}

export function request(config, success, failure) {
  // 1.创建axios的实例
  const instance = axios.create({
    baseURL: 'http://123.207.32.32:8000',
    timeout: 5000
  })

  // 发送真正的网络请求
  instance(config)
    .then(res => {
      // console.log(res);
      success(res);
    })
    .catch(err => {
      // console.log(err);
      failure(err)
    })
}
```

## Use

```js
import {request} from "./network/request";

request({
 url: '/home/multidata'
}, res => {
 console.log(res);
}, err => {
 console.log(err);
})

request({
	baseConfig: { },
	success: function (res) { },
  failure: function (err) { }
})

request({
  url: '/home/multidata'
}).then(res => {
  console.log(res);
}).catch(err => {
  // console.log(err);
})
```

## Ultimate Use && Interceptors

```js
import axios from 'axios'

export function request(config) {
  // 1.创建axios的实例
  const instance = axios.create({
    baseURL: 'http://123.207.32.32:8000',
    timeout: 5000
  })

  // 2.axios的拦截器
  // 2.1.请求拦截的作用
  instance.interceptors.request.use(config => {
    // console.log(config);
    // 1.比如config中的一些信息不符合服务器的要求

    // 2.比如每次发送网络请求时, 都希望在界面中显示一个请求的图标

    // 3.某些网络请求(比如登录(token)), 必须携带一些特殊的信息
    return config
  }, err => {
    // console.log(err);
  })

  // 2.2.响应拦截
  instance.interceptors.response.use(res => {
  	// (数据过滤或者其他处理)
    console.log(res);
    return res.data
  }, err => {
  	// (根据错误码为用户提供合理性展示)
    console.log(err);
  })

  // 3.发送真正的网络请求
  return instance(config)  //	Promise
}
```

# 项目实战

## Directory Structure

```
HYMall
├── .browserslistrc
├── .editorconfig                            // 编辑器配置统一项目
├── .gitignore
├── babel.config.js
├── package-lock.json
├── package.json                             // 项目依赖版本管理
├── postcss.config.js
├── public
│   ├── index.html
│   └── logo.png
├── README.md
├── src                                      // 源代码文件夹
│   ├── App.vue
│   ├── assets                               // 静态资源目录
│   │   ├── css
│   │   │   ├── base.css
│   │   │   └── normalize.css
│   │   └── img
│   │       ├── cart  
│   │       │   └── tick.svg
│   │       ├── common
│   │       │   ├── back.svg
│   │       │   ├── collect.svg
│   │       │   ├── placeholder.png
│   │       │   └── top.png
│   │       ├── detail
│   │       │   └── detail_bottom.png
│   │       ├── home
│   │       │   └── recommend_bg.jpg
│   │       └── tabbar
│   │           ├── cart.svg
│   │           ├── cart_active.svg
│   │           ├── category.svg
│   │           ├── category_active.svg
│   │           ├── home.svg
│   │           ├── home_active.svg
│   │           ├── profile.svg
│   │           └── profile_active.svg
│   ├── common                              // 公共文件
│   │   ├── const.js
│   │   ├── mixin.js
│   │   └── utils.js
│   ├── components 													// vue组件
│   │   ├── common                          // 公共组件
│   │   │   ├── gridView
│   │   │   │   └── GridView.vue
│   │   │   ├── navbar
│   │   │   │   └── NavBar.vue
│   │   │   ├── scroll
│   │   │   │   └── Scroll.vue
│   │   │   ├── swiper
│   │   │   │   ├── index.js
│   │   │   │   ├── Swiper.vue
│   │   │   │   └── SwiperItem.vue
│   │   │   └── tabbar
│   │   │       ├── TabBar.vue
│   │   │       └── TabBarItem.vue
│   │   └── content                          // 内容组件
│   │       ├── backTop
│   │       │   └── BackTop.vue
│   │       ├── Icon
│   │       │   ├── Icon.vue
│   │       │   └── svg.vue
│   │       ├── mainTabbar
│   │       │   └── MainTabBar.vue
│   │       └── tabControl
│   │           └── TabControl.vue
│   ├── main.js                              // 主JS文件
│   ├── network                              // 网络配置(axios)
│   │   ├── axios.js
│   │   ├── category.js
│   │   ├── detail.js
│   │   └── home.js
│   ├── router                               // 路由配置
│   │   └── index.js
│   ├── store                                // vuex状态管理
│   │   ├── actions.js
│   │   ├── getters.js
│   │   ├── index.js
│   │   └── mutations.js
│   └── views                                // 视图
│       ├── cart
│       │   ├── Cart.vue
│       │   └── childComps
│       │       ├── BottomBar.vue
│       │       ├── CartList.vue
│       │       ├── CartListItem.vue
│       │       └── CheckButton.vue
│       ├── category
│       │   ├── Category.vue
│       │   └── childComps
│       │       ├── TabContent.vue
│       │       ├── TabContentCategory.vue
│       │       ├── TabContentDetail.vue
│       │       └── TabMenu.vue
│       ├── detail
│       │   ├── childComps
│       │   │   ├── DetailBaseInfo.vue
│       │   │   ├── DetailBottomBar.vue
│       │   │   ├── DetailCommentInfo.vue
│       │   │   ├── DetailGoodsInfo.vue
│       │   │   ├── DetailNavBar.vue
│       │   │   ├── DetailParamInfo.vue
│       │   │   ├── DetailRecommendInfo.vue
│       │   │   ├── DetailShopInfo.vue
│       │   │   └── DetailSwiper.vue
│       │   └── Detail.vue
│       ├── home
│       │   ├── childComps
│       │   │   ├── FeatureView.vue
│       │   │   ├── GoodsList.vue
│       │   │   ├── GoodsListItem.vue
│       │   │   ├── HomeSwiper.vue
│       │   │   └── RecommendView.vue
│       │   └── Home.vue
│       └── profile
│           ├── childComps
│           │   ├── ListView.vue
│           │   └── UserInfo.vue
│           └── Profile.vue
└── vue.config.js                             // 项目配置文件
```

## Github Address

目前只是完成了Home部分功能，其余暂且闲置
https://github.com/Liuhongwei3/BliVueDemoTab

## CSS

https://github.com/necolas/normalize.css/

## Config

> vue.config.js

```js
module.exports = {
	// 	configure webpack
	configureWebpack: {
		resolve: {
			extensions: [],
			alias: {
				//  vue default
				'@': 'src',
				'assets': '@assets',
				'components': '@components',
				...
			}
		}
	}
}
```

> Use

```html
<template>
	<img src="~assets/images/logo.jpg" />
</template>

<script>
	import 'assets/css/style.css'
</script>

<style scopped>
	.pic {
	 	background: url(~asset/images/bg.jpg)
	}
</style>
```

```js
let imageUrl = require("./img/marker_green.png");
```

> .editorconfig(项目统一化管理)

```
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
```

# End

- [TadmPlayer](https://liuhongwei3.github.io/2020/02/01/TadmPlayer/)
- [Vue)Shop](https://github.com/Liuhongwei3/vue_shop)
