# svelte教程（5）事件
### DOM 事件
事件绑定
```
<script>
  let m = { x: 0, y: 0 };
  function handleMousemove(event) {
    m.x = event.clientX;
    m.y = event.clientY;
  }
</script>

<style>
  div {
    margin: 0 auto;
    width: 500px;
    height: 500px;
    background: seagreen;
  }
</style>

<div on:mousemove={handleMousemove}>The mouse position is {m.x} x {m.y}</div>

```
您还可以内联声明事件处理程序,并且因为svelte实际上是一个编译器，所以并不会影响性能。
```
<script>
  let m = { x: 0, y: 0 };
</script>

<style>
  div {
    margin: 20px auto;
    width: 500px;
    height: 500px;
    background: seagreen;
  }
</style>
<div
  on:mousemove={e => {
    m.x = e.clientX;
    m.y = e.clientY;
  }}>
  The mouse position is {m.x} x {m.y}
</div>
```
### 事件修饰符
事件可以拥有改成其行为的修饰符。
- preventDefault：阻止默认事件
- stopPropagation：停止事件的传播
- passive：改善了触摸/滚轮事件的滚动性能（Svelte会在安全的地方自动添加）
- capture：事件在捕获阶段触发
- once：事件运行一次后取消监听
- self：仅当event.target是元素本身时才触发

您可以将修饰符链接在一起，例如on:click|once|capture={...}

### 组件事件
组件也可以触发事件。
```
Inner.svelte
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>

index.svelte
<Inner on:message={e=>{alert(e.detail.text)}}/>
```
createEventDispatcher必须在首次实例化组件时调用它-稍后无法在内部（例如setTimeout回调）进行调用。

### 事件转发
createEventDispatcher方法触发的事件不会进行冒泡。我们可以通过在外层组件监听message方法再转发到index.svelte。
```
// outer.svelte
<script>
  import Inner from "./Inner";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  function forward(event) {
    dispatch("message", event.detail);
  }
</script>
<Inner on:message={forward} />

// index.svelte
<Outer on:message={e => {
    alert(e.detail.text);
  }} />

```
如果on:message没有值可以转发所有message事件：
```
// outer.svelte
<script>
  import Inner from "./Inner";
</script>
<Inner on:message/>

// index.svelte
<Outer on:message={e => {
    alert(e.detail.text);
  }} />

```
### DOM 事件转发
事件转发一样可以用在dom事件上：
```
// FancyButton.svelte
<style>
	button {
		font-family: 'Comic Sans MS';
		font-size: 2em;
		padding: 0.5em 1em;
		color: royalblue;
		background: gold;
		border-radius: 1em;
		box-shadow: 2px 2px 4px rgba(0,0,0,0.5);
	}
</style>

<button on:click>
	Click me
</button>

// index.svelte
<script>
	import FancyButton from './FancyButton.svelte';

	function handleClick() {
		alert('clicked');
	}
</script>

<FancyButton on:click={handleClick}/>
```
本教程的所有代码均上传到github有需要的同学可以参考 [https://github.com/sullay/svelte-learn](https://github.com/sullay/svelte-learn)。