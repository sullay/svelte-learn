<script>
	export let segment;
</script>

<style>
	main {
		position: relative;
		max-width: 960px;
		background-color: white;
		margin: 0 auto;
		padding-top: 32px;
		box-sizing: border-box;
	}
	/* main标签内的所有h1标签 */
	main :global(h1){
    color: red;
	}
</style>

<main>
	<slot></slot>
</main>