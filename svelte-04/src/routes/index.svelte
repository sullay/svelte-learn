<script>
  import Thing from "../components/Thing";
  let user = { loggedIn: false };
  function toggle() {
    user.loggedIn = !user.loggedIn;
  }
  let x = 7;
  let list = [{ name: "aaa" }, { name: "bbb" }, { name: "ccc" }];

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

{#if user.loggedIn}
  <button on:click={toggle}>Log out</button>
{:else}
  <button on:click={toggle}>Log in</button>
{/if}

{#if x > 10}
  <p>{x} is greater than 10</p>
{:else if 5 > x}
  <p>{x} is less than 5</p>
{:else}
  <p>{x} is between 5 and 10</p>
{/if}

{#each list as l}
  <p>This is {l.name}</p>
{:else}
  <p>The list is empty</p>
{/each}
<button on:click={handleClick}>Remove first thing</button>

{#each things as thing, i (thing.id)}
  <Thing current={thing.color} index={i}/>
{/each}
