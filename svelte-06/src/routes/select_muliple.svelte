<script>
  let questions = [
    { id: 1, text: `Where did you go to school?` },
    { id: 2, text: `What is your mother's name?` },
    {
      id: 3,
      text: `What is another personal fact that an attacker could easily find with Google?`
    }
  ];

  let selectedList = [];
</script>


<select bind:value={selectedList} multiple>
  {#each questions as question}
    <option value={question} label={question.text} />
  {/each}
</select>

{#each selectedList as selected}
   <p>The question is "{selected.text}"</p>
{/each}
