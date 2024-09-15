<script>
    export let onUserCreated;
    
    let username = '';
    let email = '';
  
    async function createUser() {
      const response = await fetch('http://localhost:8080/users', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ username, email })
      });
  
      if (response.ok) {
        alert('User created successfully!');
        onUserCreated(username);
      } else if (response.status === 409) {
        alert('User already exists.');
      } else {
        alert('Failed to create user');
      }
    }
  </script>
  
  <h2>Create User</h2>
  <input type="text" placeholder="Enter username" bind:value={username} />
  <input type="text" placeholder="Enter username" bind:value={email} />
  <button on:click={createUser}>Register</button>
  