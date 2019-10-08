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
