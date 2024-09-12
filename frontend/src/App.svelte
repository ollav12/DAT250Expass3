<script>
  import CreatePollComponent from './lib/CreatePollComponent.svelte';
  import VoteComponent from './lib/VoteComponent.svelte';

  // Track which component to display: createPoll or vote
  let currentComponent = '';  // default to CreatePollComponent
  let polls = [];

  // Fetch polls for the VoteComponent
  async function fetchPolls() {
    const response = await fetch('http://localhost:8080/polls');
    if (response.ok) {
      polls = await response.json();
    } else {
      alert('Failed to fetch polls');
    }
  }

  // Switch to VoteComponent
  function showVoteComponent() {
    currentComponent = 'vote';
  }

  // Switch to CreatePollComponent
  function showCreatePollComponent() {
    currentComponent = 'createPoll';
  }
</script>

<nav>
  <button on:click={showCreatePollComponent}>Create Poll</button>
  <button on:click={showVoteComponent}>Vote</button>
</nav>

<main>
  {#if currentComponent === 'createPoll'}
    <CreatePollComponent />
  {/if}

  {#if currentComponent === 'vote'}
    <VoteComponent />
  {/if}
</main>
