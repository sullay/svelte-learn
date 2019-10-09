<script>
  let name = 'world';
  let scoops=1
  let flavours = ['Mint choc chip'];
  let menu = [
		'Cookies and cream',
		'Mint choc chip',
		'Raspberry ripple'
	];
</script>

<input bind:value={name}>

<h1>Hello {name}!</h1>

<label>
	<input type=radio bind:group={scoops} value={1}>
	One scoop
</label>

<label>
	<input type=radio bind:group={scoops} value={2}>
	Two scoops
</label>

<label>
	<input type=radio bind:group={scoops} value={3}>
	Three scoops
</label>
<div>
{#each menu as flavour}
	<label>
		<input type=checkbox bind:group={flavours} value={flavour}>
		{flavour}
	</label>
{/each}
</div>