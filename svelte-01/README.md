# svelte教程（1）简介
# 什么是svelte?
Svelte是用于构建快速Web应用程序的工具。

它类似于React和Vue这样的JavaScript框架。但是有一个关键的区别：Svelte在构建时将您的svelte代码转换为理想的JavaScript 代码，而不是在运行时解释您的代码。想要详细了解请查看[svelte简介](https://svelte.dev/blog/svelte-3-rethinking-reactivity)。
注：本教程中使用了Sapper框架进行svelte程序的开发。学习语法环节不需要掌握sapper框架的使用，想要了解的话请查看[sapper文档](https://sapper.svelte.dev/docs/)。
本教程的所有代码均上传到github有需要的同学可以参考 [https://github.com/sullay/svelte-learn](https://github.com/sullay/svelte-learn)。


###  组件 components
在Svelte中，一个应用程序由一个或多个组件组成。组件是可重用的独立代码块，它封装在一起的HTML，CSS和JavaScript，并写入.svelte文件中。

### 添加数据
在组件中声明一个变量name并在html中进行引用。“{}”内可以使用任何JavaScript代码。
```
<script>
	let name = 'world';
</script>

<h1>Hello {name}!</h1>
<h1>Hello {name.toUpperCase()}!</h1>
```
如果变量名于属性名相同的情况下，可以简写例如src={src}。
```
<script>
	let name = 'world';
	let src ="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570390818923&di=1b477739d17e14e5d8ee39b19cb40748&imgtype=0&src=http%3A%2F%2Fimg.zcool.cn%2Fcommunity%2F012e3b5954ee3da8012193a387c830.png%401280w_1l_2o_100sh.png"
</script>

<img {src} alt="Hello {name}!">
```
### 样式style
组件中可以使用<style>添加样式并且这些css规则只适用于当前组件。
实现方式是给组件内的所有css选择器都添加一个相同的class选择器并且与其他组件不相同，例如：
```
h1{
 text-align: center;
}

h1.svelte-17zljpa{
 text-align: center;
}
```
同时也给所有的标签添加一个相同的class属性。

```
<style>
h1{
	text-align: center;
}
img{
	width: 100%;
}
</style>

<script>
	let name = 'world';
	let src ="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570390818923&di=1b477739d17e14e5d8ee39b19cb40748&imgtype=0&src=http%3A%2F%2Fimg.zcool.cn%2Fcommunity%2F012e3b5954ee3da8012193a387c830.png%401280w_1l_2o_100sh.png"
</script>

<h1>Hello {name}!</h1>
<h1>Hello {name.toUpperCase()}!</h1>
<img {src} alt="Hello {name}!">
```
如果希望css规则能够全局生效可以使用:global关键字。
实现方式是global修饰的选择器不会添加class选择器。
```
<style>
/* 所有h1标签 */
:global(h1){
    color: red;
}
/* main标签内的所有h1标签 */
main :global(h1){
    color: red;
}
</style>
```
### 引入子组件
注意：自定义组件变量名首字母需为大写字母。并且父组件中的样式不会对子组件生效，反之也一样。
``` 
// sullay.svelte
<h1>I am sullay!</h1>

// index.svelte
<script>
	import Sullay from '../components/sullay.svelte'
</script>
<Sullay/>
```
### 原样显示
在Svelte中，您可以使用特殊{@html ...}标签进行原样显示。
```
<style>
.html h1{
	color: chartreuse;
}
</style>

<script>
	let html="<h1>I am sullay!</h1>"
</script>

<div class="html">{@html html}</div>
```
测试发现上面代码中的color: chartreuse;并没有生效。
原因是因为{@html ...}中的标签并没有添加对应此组件的class属性。
修改一下上面的代码，如下：
```
<style>
.html :global(h1){
	color: chartreuse;
}
</style>

<script>
	let html="<h1>I am sullay!</h1>"
</script>

<div class="html">{@html html}</div>
```
阅读本文后相信你对svelte的使用已经有一个简单的认识，后续还会更多svelte教程的内容。部分内容来自于官方教程，有兴趣的可以结合起来一起看。