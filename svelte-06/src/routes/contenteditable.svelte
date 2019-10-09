<script>
  let html = "<p>Write some text!</p>";
  let text = "Write some text!";
</script>

<style>
  [contenteditable] {
    padding: 0.5em;
    border: 1px solid #eee;
    border-radius: 4px;
  }
</style>

<div contenteditable="true" bind:innerHTML={html} />

<div contenteditable="true" bind:textContent={text} />

<pre>{html}</pre>
<p>{text}</p>
