<script>
  let w;
  let h;
  let w2;
  let h2;
  let size = 42;
  let text = "edit me";
</script>

<style>
  input {
    display: block;
    margin: 20px auto;
  }
  div {
    display: block;
  }
  .inline {
    display: inline;
  }
</style>

<input type="range" bind:value={size} />
<input bind:value={text} />

<p>size: {w}px x {h}px</p>

<div
  bind:clientWidth={w}
  bind:clientHeight={h}>
  <span style="font-size: {size}px">{text}</span>
</div>

<p>line size: {w2}px x {h2}px</p>
<div
  bind:clientWidth={w2}
  bind:clientHeight={h2}
  class="inline">
  <span style="font-size: {size}px">
    {text}
  </span>
</div>
