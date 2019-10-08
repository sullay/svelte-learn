# svelte教程（2）反应性
Svelte的核心是功能强大的反应系统，用于使DOM与您的应用程序状态保持同步-例如，响应事件。
```
<script>
	let count = 0;
	function handleClick() {
		count += 1;
	}
</script>
<button on:click={handleClick}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```
### 声明
当组件的状态更改时，Svelte会自动更新DOM。通常，组件状态的某些部分需要从其他部分计算并在它们发生任何变化时重新计算。使用$:声明后只要任何引用的值发生更改，都要重新运行此代码。
我们可以将他用作计算属性。
```
<script>
	let count = 0;
	$: doubled = count * 2;
	
	function handleClick() {
		count+=1;
	}
</script>
<button on:click={handleClick}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
<p>{count} doubled is {doubled}</p>
```
除此之外，我们可以反应性的运行任意语句。
```
<script>
	let count = 0;
	$: doubled = count * 2;
	$: console.log(`the count is ${count}`)
	$: {
		console.log(`I SAID THE COUNT IS ${count}`);
	}
	$: if(count>=5){
		console.log(`count is dangerously high!`);
	}
	function handleClick() {
		count+=1;
	}
</script>
<button on:click={handleClick}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
<p>{count} doubled is {doubled}</p>
```
注意：svelte的反应性是由赋值触发的，所以使用push、pop、splice、shift、unshift等数组操作不会导致更新。
```
<script>
  function add() {
    arr.push(arr[arr.length - 1] + 1);
  }
</script>
<button on:click={add}>add</button>
<p>The last one is {arr[arr.length - 1]}</p>

```
测试后发现并没有触发更新。
修改一下上面的add方法，如下：
```
<script>
  function add() {
    arr.push(arr[arr.length - 1] + 1);
    arr = arr;
  }
  // function add(){
  // 	arr=[...arr,arr[arr.length-1]+1];
  // }
  // function add(){
  // 	arr[arr.length]=arr[arr.length-1]+1;
  // }
</script>
<button on:click={add}>add</button>
<p>The last one is {arr[arr.length - 1]}</p>

```

本教程的所有代码均上传到github有需要的同学可以参考 [https://github.com/sullay/svelte-learn](https://github.com/sullay/svelte-learn)。
