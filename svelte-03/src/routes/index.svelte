<script>
  import Nested from "../components/Nested";
  import Info from "../components/Info";
  let info = {
    name: "sullay",
    github: "https://github.com/sullay/svelte-learn",
    version: "1.0.0"
  };
</script>

<Nested answer={42} />
<Nested />
<Info {...info} />
