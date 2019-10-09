# svelte教程（7）生命周期
### onMount
onMount将在组件首次呈现到DOM之后运行。除了onDestroy之外，生命周期函数不会在SSR期间运行，这意味着一旦组件挂在到DOM上，我们就可以避免在DOM构建之前获取到应在DOM构建以后加载的数据。

必须在逐渐初始化时调用生命周期函数，不能再setTimeout中调用。

如果onMount返回一个函数，则在销毁组件时将调用该函数。
```
<script>
	import { onMount } from 'svelte';

	let photos = [];

	onMount(async () => {
		const res = await fetch(`https://jsonplaceholder.typicode.com/photos?_limit=20`);
		photos = await res.json();
	});
</script>

<style>
	.photos {
		width: 100%;
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		grid-gap: 8px;
	}

	figure, img {
		width: 100%;
		margin: 0;
	}
</style>

<h1>Photo album</h1>

<div class="photos">
	{#each photos as photo}
		<figure>
			<img src={photo.thumbnailUrl} alt={photo.title}>
			<figcaption>{photo.title}</figcaption>
		</figure>
	{:else}
		<!-- this block renders when photos.length === 0 -->
		<p>loading...</p>
	{/each}
</div>
```
通过测试发现onMount的回调函数执行顺序在onDestroy之后。并且返回不能是promise对象，也就意味着如果使用async关键字将不会在组件销毁时执行。
```
<script>
  import { onMount, onDestroy } from "svelte";

  let photos = [];
  onMount(() => {
    fetch(`https://jsonplaceholder.typicode.com/photos?_limit=20`)
      .then(res => {
        return res.json();
      })
      .then(res => {
        photos = res;
      });
    return () => {
      console.log("onMount end2");
    };
  });
  onDestroy(() => {
    console.log("onDestroy2");
  });
</script>

<style>
  .photos {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 8px;
  }

  figure,
  img {
    width: 100%;
    margin: 0;
  }
</style>

<a href="/">Photo album</a>

<div class="photos">
  {#each photos as photo}
    <figure>
      <img src={photo.thumbnailUrl} alt={photo.title} />
      <figcaption>{photo.title}</figcaption>
    </figure>
  {:else}
    <!-- this block renders when photos.length === 0 -->
    <p>loading...</p>
  {/each}
</div>

```
### onDestory
当销毁组件时调用。
```
<script>
  import { onDestroy } from "svelte";
  let seconds = 0;
  const interval = setInterval(() => (seconds += 1), 1000);
  onDestroy(() => clearInterval(interval));
</script>

<p>The page has been open for {seconds} {seconds === 1 ? 'second' : 'seconds'}</p>

```
### beforeUpdate 、afterUpdate 
beforeUpdate 在DOM更新前运行，afterUpdate 在DOM更新后运行。
```
<script>
	import Eliza from 'elizabot';
	import { beforeUpdate, afterUpdate } from 'svelte';

	let div;
	let autoscroll;

	beforeUpdate(() => {
		autoscroll = div && (div.offsetHeight + div.scrollTop) > (div.scrollHeight - 20);
	});

	afterUpdate(() => {
		if (autoscroll) div.scrollTo(0, div.scrollHeight);
	});

	const eliza = new Eliza();

	let comments = [
		{ author: 'eliza', text: eliza.getInitial() }
	];

	function handleKeydown(event) {
		if (event.which === 13) {
			const text = event.target.value;
			if (!text) return;

			comments = comments.concat({
				author: 'user',
				text
			});

			event.target.value = '';

			const reply = eliza.transform(text);

			setTimeout(() => {
				comments = comments.concat({
					author: 'eliza',
					text: '...',
					placeholder: true
				});

				setTimeout(() => {
					comments = comments.filter(comment => !comment.placeholder).concat({
						author: 'eliza',
						text: reply
					});
				}, 500 + Math.random() * 500);
			}, 200 + Math.random() * 200);
		}
	}
</script>

<style>
	.chat {
		display: flex;
		flex-direction: column;
		height: 500px;
		max-width: 320px;
    margin: 0 auto;
	}

	.scrollable {
		flex: 1 1 auto;
		border-top: 1px solid #eee;
		margin: 0 0 0.5em 0;
		overflow-y: auto;
	}

	article {
		margin: 0.5em 0;
	}
  .eliza{
    text-align: left;
  }
	.user {
		text-align: right;
	}

	span {
		padding: 0.5em 1em;
		display: inline-block;
	}

	.eliza span {
		background-color: #eee;
		border-radius: 1em 1em 1em 0;
	}

	.user span {
		background-color: #0074D9;
		color: white;
		border-radius: 1em 1em 0 1em;
	}
</style>

<div class="chat">
	<h1>Eliza</h1>

	<div class="scrollable" bind:this={div}>
		{#each comments as comment}
			<article class={comment.author}>
				<span>{comment.text}</span>
			</article>
		{/each}
	</div>

	<input on:keydown={handleKeydown}>
</div>
```
### tick
tick不同于其他生命周期，因为您可以随时调用它，而不仅是组件首次初始化时。它返回一个promise，该promise在任何pending状态的promise状态更改并应用于DOM时立刻执行resolve。如果没有pending状态的promise时，则立即执行resolve。

当您在Svelte中更改组件状态时，它不会立即更新DOM。而是等到下一个微任务，看看是否还有其他需要应用的更改，包括在其他组件中。这样做避免了不必要的工作，并使浏览器可以更有效地对事物进行批处理。

下面例子中使用tab切换大小写，如不过不适用tick，由于text 更改后，dom并没有马上更新，所以立刻设置光标位置然后dom更新后失效。使用tick可以解决这个问题。
```
<script>
	import { tick } from 'svelte';

	let text = `Select some text and hit the tab key to toggle uppercase`;

	async function handleKeydown(event) {
		if (event.which !== 9) return;

		event.preventDefault();

		const { selectionStart, selectionEnd, value } = this;
		const selection = value.slice(selectionStart, selectionEnd);

		const replacement = /[a-z]/.test(selection)
			? selection.toUpperCase()
			: selection.toLowerCase();

		text = (
			value.slice(0, selectionStart) +
			replacement +
			value.slice(selectionEnd)
		);

		await tick();
		this.selectionStart = selectionStart;
		this.selectionEnd = selectionEnd;
	}
</script>

<style>
	textarea {
		width: 100%;
		height: 200px;
	}
</style>

<textarea value={text} on:keydown={handleKeydown}></textarea>
```
本教程的所有代码均上传到github有需要的同学可以参考 [https://github.com/sullay/svelte-learn](https://github.com/sullay/svelte-learn)。