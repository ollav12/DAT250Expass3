<script>
  import { onMount } from 'svelte';

  export let polls = [];
  let selectedPoll = null;
  let selectedOption = null;

  // Fetch polls from backend API
  onMount(async () => {
    const response = await fetch('http://localhost:8080/polls'); // Use API endpoint
    if (response.ok) {
      polls = await response.json();
    } else {
      alert('Failed to fetch polls');
    }
  });

  // Submit a vote
  async function submitVote() {
    if (!selectedPoll || selectedOption === null) {
      alert('Please select a poll and option to vote on.');
      return;
    }

    const response = await fetch(`http://localhost:8080/polls/${selectedPoll.id}/vote`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ option: selectedOption })
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
        <option value={poll}>{poll.title}</option>
      {/each}
    </select>
  </label>

  {#if selectedPoll}
    <h3>{selectedPoll.title}</h3>

    {#each selectedPoll.options as option, index}
      <label>
        <input type="radio" name="option" value={index} bind:group={selectedOption}>
        {option}
      </label>
    {/each}
  {/if}

  <button on:click={submitVote}>Submit Vote</button>
</main>
