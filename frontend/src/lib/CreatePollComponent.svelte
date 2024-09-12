<script>
    import { onMount } from 'svelte';
    let pollTitle = '';
    let pollOptions = [''];
    let validUntil = ''; // datetime-local input value
  
    // Add a new option to the poll
    function addOption() {
      pollOptions = [...pollOptions, ''];
    }
  
    // Format the date to the desired format
    function formatDateToISO(dateString) {
      const date = new Date(dateString);
      const isoString = date.toISOString(); // Format as YYYY-MM-DDTHH:mm:ss.sssZ
  
      // Adjust the format to match the required output
      const formattedString = isoString.replace('Z', 'Z'); // Ensure Z timezone
      return formattedString;
    }
  
    // Format the options and send the request to create a poll
    async function createPoll() {
      const formattedOptions = pollOptions.map(option => ({
        caption: option
      }));
  
      const formattedValidUntil = formatDateToISO(validUntil);
  
      const pollData = {
        question: pollTitle,
        options: formattedOptions,
        validUntil: formattedValidUntil
      };
  
      // Send a POST request to the backend to create a poll
      const response = await fetch('http://localhost:8080/polls', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(pollData) // Send the correctly formatted JSON
      });
  
      if (response.ok) {
        alert('Poll created successfully!');
        pollTitle = '';
        pollOptions = [''];
        validUntil = '';
      } else {
        alert('Failed to create poll');
      }
    }
  </script>
  
  <main>
    <h2>Create a New Poll</h2>
  
    <label>Poll Question:
      <input type="text" bind:value={pollTitle} placeholder="Enter poll question">
    </label>
  
    {#each pollOptions as option, index}
      <label>Option {index + 1}:
        <input type="text" bind:value={pollOptions[index]} placeholder="Enter option {index + 1}">
      </label>
    {/each}
  
    <button on:click={addOption}>Add Option</button>
  
    <label>Valid Until:
      <input type="datetime-local" bind:value={validUntil}>
    </label>
  
    <button on:click={createPoll}>Create Poll</button>
  </main>
  