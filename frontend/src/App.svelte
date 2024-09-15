<script>
  import CreatePollComponent from './lib/CreatePollComponent.svelte';
  import CreateUserComponent from './lib/CreateUserComponent.svelte';
  import VoteComponent from './lib/VoteComponent.svelte';

  let username = '';
  let userCreated = false;
  let showCreatePoll = false;
  let showVotePoll = false;

  function handleUserCreated(createdUsername) {
    username = createdUsername;
    userCreated = true;
  }

  // Show create poll form
  function showCreatePollForm() {
    showCreatePoll = true;
    showVotePoll = false;
  }

  // Show vote poll options
  function showVotePollOptions() {
    showCreatePoll = false;
    showVotePoll = true;
  }
</script>

<main>
  <!-- User Registration -->
  {#if !userCreated}
    <CreateUserComponent onUserCreated={handleUserCreated} />
  {/if}

  <!-- Options After Registration -->
  {#if userCreated}
    <h2>Welcome, {username}!</h2>
    <button on:click={showCreatePollForm}>Create a Poll</button>
    <button on:click={showVotePollOptions}>Vote on a Poll</button>

    <!-- Create Poll Form -->
    {#if showCreatePoll}
      <CreatePollComponent/>
    {/if}

    <!-- Vote on a Poll -->
    {#if showVotePoll}
      <VoteComponent username={username} />
    {/if}
  {/if}
</main>
