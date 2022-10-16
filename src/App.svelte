<script>
  import svelteLogo from './assets/svelte.svg'
  import Counter from './lib/Counter.svelte'
  import Nested from './lib/Nested.svelte';
  import Info from './lib/info.svelte' //So even if the file name has smaller letters, the imported class matters
  let numbers = [1, 2, 3, 4];

  function addNumber() {
    numbers.push(numbers.length + 1);
    numbers = numbers
  }

  $: sum = numbers.reduce((t, n) => t + n, 0);

  const pkg = {
		name: 'svelte',
		version: 3,
		speed: 'blazing',
		website: 'https://svelte.dev'
	};

</script>

<main>
  <div>
    <a href="https://vitejs.dev" target="_blank"> 
      <img src="/vite.svg" class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank"> 
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>
  <h1>Vite + Svelte</h1>

  <div class="card">
    <Counter />
  </div>

  <p>Here is a complete para component has been imported along with a prop.</p>
  <Nested answer={47752}/>
  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme" target="_blank">SvelteKit</a>, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">
    What you are seeing above is imported from a different file and just rendered 
    here. Click on the Vite and Svelte logos to get ahead
  </p>

  <p>{numbers.join(' + ')} = {sum}</p>

  <button on:click={addNumber}>
    Add a number
  </button>

  <p>
   <strong> A simple rule of thumb:</strong> the updated variable must directly appear on the left hand side of the assignment.
  </p>

  <p>Making new components.</p>
  <Nested/>
  <p>Note the data that has been initiated below comes from the object that is declared in this main App</p>
  <Info name={pkg.name} version={pkg.version} speed={pkg.speed} website={pkg.website}/>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
