# svelte教程（3）props

在任何实际应用程序中，您都需要将数据从一个组件传递到其子组件。为此，我们需要声明properties，通常将其缩写为'props'。
注意：Svelte中，我们使用export关键字来实现。export的工作原理与JavaScript中不同。
```
// Nested.svelte
<script>
	export let answer;
</script>
<p>The answer is {answer}</p>


// index.svelte
<script>
	import Nested from '../components/Nested';
</script>
<Nested answer={42}/>
```
指定props默认值，例如：
```
// Nested.svelte
<script>
	export let answer='a mystery';
</script>
<p>The answer is {answer}</p>


// index.svelte
<script>
	import Nested from '../components/Nested';
</script>
<Nested answer={42}/>
<Nested/>
```
如果您有一个属性对象，则可以将它们“扩展”到一个组件上，而不用指定每个对象：
```
// Info.svelte
<script>
	export let name;
	export let github;
	export let version;
</script>
<div>
	<p>The name is {name}</p>
	<a href={github}>The github is {github}</a>
	<p>The version is {version}</p>
</div>

// index.svelte
<script>
  import Info from "../components/Info";
  let info = {
    name: "sullay",
    github: "https://github.com/sullay/svelte-learn",
    version: "1.0.0"
  };
</script>
<Info {...info} />

```

本教程的所有代码均上传到github有需要的同学可以参考 [https://github.com/sullay/svelte-learn](https://github.com/sullay/svelte-learn)。