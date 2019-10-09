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
