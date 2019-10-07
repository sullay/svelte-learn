<script>
  let count = 0;
  let arr = [1, 2, 3];
  $: doubled = count * 2;
  $: console.log(`the count is ${count}`);
  $: {
    console.log(`I SAID THE COUNT IS ${count}`);
  }
  $: if (count >= 5) {
    console.log(`count is dangerously high!`);
  }
  function handleClick() {
    count += 1;
  }
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

<button on:click={handleClick}>
  Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
<p>{count} doubled is {doubled}</p>

<button on:click={add}>add</button>
<p>The last one is {arr[arr.length - 1]}</p>
