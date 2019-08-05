v-bind:title = "message"title和vue实例的message属性保持一致
v-if="seen"seen可动态变化
v-for="todo in todos"todos，数组结构
v-on:clike="method"监听点击事件运行method方法
v-model="data"data与vue实例中的data双向绑定

```
Vue.conponent('todo-item',{
	template:'<li>这是一个待办项</li>'})
//注册了一个组件
<ol><todo-iteme></todo-item></ol>
//构建了另一个组建模板
```
```
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```
组件的应用模板

var vue = new Vue({//选项})
所有的vue组件都是Vue实例

Object.freeze(obj)会阻止修改现有属性
vm.$data data是Vue实例自带的属性和方法，需要在前面加$

钩子：
```
new Vue({
    data:{a:1},
    created:function(){
    //a = "HelloWorld!"
    }})
```
其它钩子：created,mounted,updated,desdroyed

```
<span>Message:{{message}}</span> //动态绑定
```
通过使用v-once，也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定
`<span v-once></span>`

{{}}会被解释为文本不是HTML代码，如有需要，加v-HTML
`<span v-HTML="html"></span>` html为HTML代码
绝不要对用户提供的内容使用插值 VSS攻击（重定向攻击）

Vue.js提供了完全的JavaScript支持
    限制是：每个绑定只能包含单个表达式，
模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。

指令和参数、修饰符
指令：v-开头的特殊特性；职责：表达式改变的时候，将产生连带影响
`<p v-if="ok">hello</p>`
参数：有的指令能够接收一个参数
`<a v-on:click="doSomething">...</a>`
修饰符：.指明的特殊后缀
`<form v-on submit.prevent="onSubmit">...</form>`
.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()

缩写：
```
v-blind: =:
v-on = @
```

计算器和侦听器
计算器（我的理解就是有依赖的方法）
```
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')}}
```
依赖于this.message，该vue实例的message
与方法的区别：计算属性响应式依赖自动更新，产生缓存（方法不会产生缓存）
计算属性的setter：
```
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName},
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1] }}}
```
（我理解的：setter自身改变，set依赖自己的其他属性，getter，其它属性改变，自己get改变更新值）
侦听器：
```
watch:{
    question: function(){return HelloWorld}}
```
question发生改变，执行function()

Class与Style绑定
Class绑定：
对象语法：
```vue
<div v-blind:class="{active:isActive,text-danger:hasError}">...</div>
data{isActive:true,hasError:false}
--><div class="active">...</div>
```
很强大的：绑定计算属性：
```vue
<div v-blind:class="classObject">...</div>
data{...},
computed:{classObject:functiong(){...}}
```
数组语法：
```vue
<div v-bind:class="[activeClass, errorClass]">...</div>
data: {activeClass: 'active',errorClass: 'text-danger'}   or
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>   or
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```
用在组件上：.........
Style绑定：
对象语法：

```vue
<div v-blind:style="styleObject">...</div>
data:{styleObject:{color:'red',fontSize:'20px'}}
```
数组语法：
```
<div v-bind:style="[baseStyles, overridingStyles]">...</div>（将多个样式对象应用到同一个元素上）
```
条件渲染：
```vue
<template v-if="ok">
  <p v-if="age<16">age<16</p>
  <p v-else-if="a>18">16<age&&age<18</p>
  <p v-else="age>18">age>18</p>
</template>
```
用key管理可复用的元素：
```vue
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
因为key不同，所以不复用，所以每次重新渲染
v-show：
v-show不支持<template>元素，也不支持v-else
不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display，v-if销毁重新渲染
如果切换少，用v-if，切换开销小，不用不渲染，切换多，用v-show，切换开销大，都渲染可复用，渲染开销小
3-11-13

列表渲染：
```
for in:
<div v-for="(value,key,index) in object":key="key">...</div>
```
尽量在用v-for的时候使用key
数组更新检测：
变异方法：push(),pop(),sort(),reverse()等
替换数组：非变异数组：concat(),filter()等，虽然不改变数组，但总是产生一个新的数组替换原来的数组
注意事项：
不能检测`vm.items[index]=value,vm.items.length=newlength,vm.data.key=value`
解决：`Vue.set(vm.items,index,value),vm.iterms.splice(newlength),Vue.set(vm.data,key,value)`
显示过滤/排序的结果：用计算属性或者方法返回排序后的新的数组
```
v-for with v-if
<li v-for="one in many"v-if="one != 0">...</li>
```

监听事件：
```vue
<div id="button_example"> <button v-on:click="method('Hello')">button</button></div>
var vm=new Vue({el:#button_example,data{},methods:{method:function(word){return word}}})
```
时间修饰符：`v-on:click.stop.prevent.capture.self.once.passive`等(顺序很重要）
按键修饰符：
`<input v-on:keyup.enter="submit">||<input @keyup.enter="submit">`
可通过全局 config.keyCodes 对象自定义按键修饰符别名：Vue.config.keuCodes.f2=113
系统修饰符：`<div @click.ctrl="clike_while_ctrl">Click it while Ctrl</div>`
[表单输入绑定](https://vuejs.bootcss.com/v2/guide/forms.html)

组件基础：组件是可复用的Vue实例
```vue
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }}times.</button>'
})
```
使用：
```
<div id='nothing'><buttion-couter></button-counter></div>
new Vue({el:'#nothing'})
```
组件是可复用的，一个组件的data必须是函数（否则共享(例如上个例子的count))
组件注册：
Vue.component()全局注册，还有其它注册方式
通过Prop向子组件传递数据：
Prop 是你可以在组件上注册的一些自定义特性。当一个值传递给一个 prop 特性的时候，它就变成了那个组件实例的一个属性（我的理解是Prop是组件实例化的时候的自身特性）
```
Vue.component('name'{props:['spicial-1','spicial-2'],template:'<h3>{{title}}</h3}})
<name special-1='1',special-2='2'>...</name>
<name specail-1='2',special-2='1'>...</name>
new Vue({
  el: '#blog-post-demo',
  data: {posts: [{ id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' }]}})
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```
原话是：我们可以使用v-blind来动态传递prop。在不清楚要渲染的具体内容的时候非常有用。
(这是实现Prop的另一种方法？暂时不懂，没有用到Prop，只是简单的v-blind）
(或者是用另一种方法实例化几个title不同的组件，大概是这个意思吧）
单个根元素：
every component must hava a single root element(每个组件必须只有一个根元素)
解决：将模板的内容包裹在一个父元素内
当有很多个prop的时候，我们让它变成一个单独的post prop(我的理解是：将所有的prop打包，通过例如post.title,post.data,post.author等的方法引用）
通过事件向父级组件发送消息：
用`$emit`方法向父级触发一个事件
`<button v-on:click="$emit('event-name',a-value)">button name</button>`
用on-click在组件上监听这个事件
`component-name v-on:evnet-name="do something or a $a-value">...</component-name>`
`if event-name="a-method",a-value`将作为这个方法的第一个参数
在组件上使用v-model：
自定义事件也可以用于创建支持 v-model 的自定义输入组件。记住：
`<input v-model="searchText">`
等价于：
```
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value">
 (EORRO:我确定现在还没有看v-model,好吧，现在就是要学v-model)
 Vue.component('custom-input', {
  props: ['value'],
  template: '
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >'
})
```
使用：`<custom-input v-model="searchText">...</custom-input>`
每个component都必须要有一个prop:value
例：将searchText传递给了value并实现动态绑定
通过插槽分发内容：
```
Vue.component('alert-box', {
  template: `

    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `})
 (EORRO:没看懂.jpg)
```
动态组件：
在不同的组件之间切换：
<!--组件会在currentTabComponent改变时改变-->
`<component v-bind:is="currentTabComponent"></component>`

</组件基础>
</Vue基础>

<Vue不那么基础>
<Vue深入了解组件>
<Vue插槽>
slot,slot是一个插槽，可以插入任何东西包括另一个组件
具名插槽：带有名字的插槽：slot name="Aname"
</Vue插槽>
<Vue处理边界情况>
在JavaScript里直接访问一个子组件：
ref：为这个子组件赋予一个ID引用
`<base-input ref="usernameInput"></base-input>`
this.$refs.usernameInput(使用)
</Vue处理边界情况>
</Vue深入了解组件>
</Vue不那么基础>

<Vue常用的API>
ref：ref被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs 对象上
重要说明：因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在
</Vue常用的API>
