<script>
  let questions = [
    { id: 1, text: `Where did you go to school?` },
    { id: 2, text: `What is your mother's name?` },
    {
      id: 3,
      text: `What is another personal fact that an attacker could easily find with Google?`
    }
  ];

	let selected;
</script>

<select bind:value={selected}>
  {#each questions as question}
    <option value={question} label={question.text}></option>
  {/each}
</select>

<!-- <p>The question is "{selected?questions.filter(q => q.id === selected)[0].text:null}"</p> -->
<p>The question is "{selected?selected.text:''}"</p>
