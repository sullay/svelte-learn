# svelte教程（8）stores
有时，您将需要多个不相关的组件或常规的JavaScript模块访问这些值。

在Svelte，我们通过store来做到这一点。store只是一种对象，该对象具有一种subscribe方法，该方法允许在store的value发生变化时通知订阅过的组件。

### 可写 store
通过 writable  方法可以创建一个可写store，传入两个参数value, start。
- value：初始值
- start：获得第一个订阅者时调用，拥有一个参数为set的回调。可以返回一个stop方法，该方法在最后一个订阅者退订时执行。
```
import { writable } from 'svelte/store';

export const count = writable(0,(set)=>{
  console.log('subscribe count')
  set(100)
  return ()=>{
    console.log('clear count')
  }
});
```
可读store拥有三个方法：update 、set、subscribe。
- set: 设置value值。
- update: 更新value值，接受一个参数为value的方法，return一个新的value值。
```
function update(fn) {
   set(fn(value));
}
```
- subscribe: 订阅该store，接受一个参数为value的方法，用于获得value值并进行处理。返回值为一个退订方法，执行该方法完成退订。
```
<script>
  import { onDestroy } from "svelte";
  import { count } from "../stores";
  let count_value;
  const unsubscribe = count.subscribe(value => {
    count_value = value;
  });
  onDestroy(unsubscribe());
</script>

<h1>The count is {count_value}</h1>
<button
  on:click={() => {
    count.update(c => c - 1);
  }}>
  -
</button>
<button
  on:click={() => {
    count.update(c => c + 1);
  }}>
  +
</button>
<button
  on:click={() => {
    count.set(0);
  }}>
  reset
</button>

```
### 自动订阅
使用$进行自动订阅，自动订阅的store将在组件销毁时自动调用停止订阅方法。
```
<script>
  import { onDestroy } from "svelte";
  import { count } from "../stores";
</script>

<h1>The count is {$count}</h1>
<button
  on:click={() => {
    count.update(c => c - 1);
  }}>
  -
</button>
<button
  on:click={() => {
    count.update(c => c + 1);
  }}>
  +
</button>
<button
  on:click={() => {
    count.set(0);
  }}>
  reset
</button>

```
### 只读store
只读store就是没有暴露update、set方法的可写store。
```
import {
  readable
} from 'svelte/store';

export const time = readable(new Date(), function start(set) {
  const interval = setInterval(() => {
    set(new Date());
  }, 1000);

  return function stop() {
    clearInterval(interval);
  };
});
```
```
<script>
	import { time } from '../stores';

	const formatter = new Intl.DateTimeFormat('en', {
		hour12: true,
		hour: 'numeric',
		minute: '2-digit',
		second: '2-digit'
	});
</script>

<h1>The time is {formatter.format($time)}</h1>
```
### 派生store
您可以使用创建一个store，并且这个store基于其他一个或者多个store，可以使用派生store。
```
export const elapsed = derived(
  time,
  $time => Math.round(($time - start) / 1000)
);
```
derived接受三个参数：
- stores：可以为一个store对象，或者为一个数组。
- fn：接受一个方法包含两个参数values，set。如果stores为数组，values也为数组，如果stores为store对象，values为改store的value。如果没有set参数，派生store的value为fn的返回值，如果包含set参数可以使用set方法指定value值。
- initial_value：初始值（异步时使用）

### 绑定 store
如果store是可写的（即它具有set方法），则可以绑定其值，就像可以绑定到本地组件一样。

也可以通过直接赋值将value写进store。
```
<input bind:value={$count}>

<button on:click="{() => $count += 1}">
	+ 1
</button>
```
本教程的所有代码均上传到github有需要的同学可以参考 [https://github.com/sullay/svelte-learn](https://github.com/sullay/svelte-learn)。