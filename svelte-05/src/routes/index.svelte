<script>
  import Inner from "../components/Inner";
  import Outer from "../components/Outer";
  let m = { x: 0, y: 0 };

  function handleMousemove(event) {
    m.x = event.clientX;
    m.y = event.clientY;
  }
  function handleClick() {
    alert("no more alerts");
  }
</script>

<style>
  div {
    margin: 20px auto;
    width: 500px;
    height: 500px;
    background: seagreen;
  }
</style>

<div on:mousemove={handleMousemove}>The mouse position is {m.x} x {m.y}</div>
<div
  on:mousemove={e => {
    m.x = e.clientX;
    m.y = e.clientY;
  }}>
  The mouse position is {m.x} x {m.y}
</div>
<button on:click|once={handleClick}>Click me</button>
<Inner on:message={e => {
    alert(e.detail.text);
  }} />
<Outer on:message={e => {
    alert(e.detail.text);
  }} />
