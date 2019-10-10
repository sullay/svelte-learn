<script>
  import { onDestroy } from "svelte";
  import { count } from "../stores";
  // let count_value;
  // const unsubscribe = count.subscribe(value => {
  //   count_value = value;
  // });
  // onDestroy(unsubscribe());
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

<input bind:value={$count}>

<button on:click="{() => $count += 1}">
	+ 1
</button>