# svelte教程（4）逻辑
### 条件逻辑
有条件的渲染一些元素，使用 "if" 代码块:
```
<script>
	let user = { loggedIn: true };

	function toggle() {
		user.loggedIn = !user.loggedIn;
	}
</script>

{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{/if}
```
使用else代码块
```
<script>
	let user = { loggedIn: false };

	function toggle() {
		user.loggedIn = !user.loggedIn;
	}
</script>

{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{:else}
	<button on:click={toggle}>
		Log in
	</button>
{/if}
```
多条件可以使用 else if：
```
<script>
	let x = 7;
</script>

{#if x > 10}
	<p>{x} is greater than 10</p>
{:else if 5 > x}
	<p>{x} is less than 5</p>
{:else}
	<p>{x} is between 5 and 10</p>
{/if}
```
### 循环逻辑
each 可以遍历数组、类数组对象、以及可迭代对象 each [...iterable]。
```
<script>
  let list = [{ name: "aaa" }, { name: "bbb" }, { name: "ccc" }];
</script>

{#each list as l}
  <p>This is {l.name}</p>
{/each}
```
使用list为空时渲染else代码块里的内容。
```
<script>
  let list = [];
</script>
{#each list as l}
  <p>This is {l.name}</p>
{:else}
  <p>The list is empty</p>
{/each}
```
默认情况下，修改each块的值时，它将在块的末尾添加和删​​除项目，并更新所有已更改的值。那可能不是您想要的。看下面一个例子,你会发现删除掉的是最后一个元素，然而我们实际想要删除的是第一个。
```
// Thing.svelte
<script>
	// `current` is updated whenever the prop value changes...
	export let current;

	// ...but `initial` is fixed upon initialisation
	const initial = current;
</script>

<p>
	<span style="background-color: {initial}">initial</span>
	<span style="background-color: {current}">current</span>
</p>

<style>
	span {
		display: inline-block;
		padding: 0.2em 0.5em;
		margin: 0 0.2em 0.2em 0;
		width: 4em;
		text-align: center;
		border-radius: 0.2em;
		color: white;
	}
</style>
```
```
<script>
  import Thing from "../components/Thing";
  let things = [
    { id: 1, color: "#0d0887" },
    { id: 2, color: "#6a00a8" },
    { id: 3, color: "#b12a90" },
    { id: 4, color: "#e16462" },
    { id: 5, color: "#fca636" }
  ];
  function handleClick() {
    things = things.slice(1);
  }
</script>

<button on:click={handleClick}>
	Remove first thing
</button>

{#each things as thing}
	<Thing current={thing.color}/>
{/each}
```
为了能够删除掉指定的元素，我们需要给元素加上唯一标识key：
```
<script>
  import Thing from "../components/Thing";
  let things = [
    { id: 1, color: "#0d0887" },
    { id: 2, color: "#6a00a8" },
    { id: 3, color: "#b12a90" },
    { id: 4, color: "#e16462" },
    { id: 5, color: "#fca636" }
  ];
  function handleClick() {
    things = things.slice(1);
  }
</script>

<button on:click={handleClick}>
    Remove first thing
</button>

{#each things as thing (thing.id)}
    <Thing current={thing.color}/>
{/each}
```
有时我们还需要用到索引
```
// Thing.svelte
<script>
	// `current` is updated whenever the prop value changes...
	export let current;
  export let index;
	// ...but `initial` is fixed upon initialisation
	const initial = current;
</script>

<p>
	<span style="background-color: {initial}">initial {index}</span>
	<span style="background-color: {current}">current {index}</span>
</p>

<style>
	span {
		display: inline-block;
		padding: 0.2em 0.5em;
		margin: 0 0.2em 0.2em 0;
		width: 4em;
		text-align: center;
		border-radius: 0.2em;
		color: white;
	}
</style>
```
```
<script>
  import Thing from "../components/Thing";
  let things = [
    { id: 1, color: "#0d0887" },
    { id: 2, color: "#6a00a8" },
    { id: 3, color: "#b12a90" },
    { id: 4, color: "#e16462" },
    { id: 5, color: "#fca636" }
  ];
  function handleClick() {
    things = things.slice(1);
  }
</script>

<button on:click={handleClick}>
    Remove first thing
</button>

{#each things as thing, i (thing.id)}
    <Thing current={thing.color} index={i}/>
{/each}
```
### 异步数据
svelte还可以处理异步数据。
```
<script>
  let num = 0;
  let promise = getNumber();
  function sleep(duration) {
    return new Promise(function(resolve, reject) {
      setTimeout(resolve, duration);
    });
  }
  async function getNumber() {
    await sleep(3000)
    if (num < 10) {
      return num;
    } else {
      throw new Error(`The ${num} is too big`);
    }
  }
  function handleClick() {
    num += 1;
    promise = getNumber();
  }
</script>

<button on:click={handleClick}>btn</button>
{#await promise}
  <p>...waiting</p>
{:then value}
  <p>The number is {value}</p>
{:catch error}
  <p style="color: red">{error}</p>
{/await}

```
如果在promise resolve调用之前不希望现实任何数据：
```
{#await promise then value}
  <p>The number is {value}</p>
{:catch error}
  <p style="color: red">{error}</p>
{/await}

```
如果确定不会执行reject，可以省略catch:
```
{#await promise then value}
  <p>The number is {value}</p>
{/await}

```
本教程的所有代码均上传到github有需要的同学可以参考 [https://github.com/sullay/svelte-learn](https://github.com/sullay/svelte-learn)。