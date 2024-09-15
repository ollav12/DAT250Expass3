<script>
  import { onMount } from 'svelte';

  export let username = '';
  let polls = [];
  let selectedPoll = null;
  let selectedOption = null;

  // Fetch polls from backend API
  onMount(async () => {
    try {
      const response = await fetch('http://localhost:8080/polls');
      if (response.ok) {
        const data = await response.json();
        // Convert the object to an array of polls
        polls = Object.values(data);
      } else {
        alert('Failed to fetch polls');
      }
    } catch (error) {
      console.error('Error fetching polls:', error);
      alert('An error occurred while fetching polls');
    }
  });

  // Submit a vote
  async function submitVote() {
    if (!selectedPoll || selectedOption === null) {
      alert('Please select a poll and option to vote on.');
      return;
    }

    const selectedOptionData = selectedPoll.options[selectedOption];

    const response = await fetch(`http://localhost:8080/users/${username}/votes`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        option: selectedOptionData
      })
    });

    if (response.ok) {
      alert('Vote submitted successfully!');
    } else {
      alert('Failed to submit vote');
    }
  }
</script>

<main>
  <h2>Vote on a Poll</h2>

  <label>Select a Poll:
    <select bind:value={selectedPoll}>
      <option value="" disabled selected>Select a poll</option>
      {#each polls as poll}
        <option value={poll}>{poll.question}</option>
      {/each}
    </select>
  </label>

  {#if selectedPoll}
    <h3>{selectedPoll.question}</h3>

    {#each selectedPoll.options as option, index}
      <label>
        <input type="radio" name="option" value={index} bind:group={selectedOption}>
        {option.caption}
      </label>
    {/each}
  {/if}

  <button on:click={submitVote}>Submit Vote</button>
</main>
