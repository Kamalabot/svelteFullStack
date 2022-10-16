<script>
  import svelteLogo from './assets/svelte.svg'
  import Counter from './lib/Counter.svelte'
  import Nested from './lib/Nested.svelte';
  import Info from './lib/info.svelte' //So even if the file name has smaller letters, the imported class matters
  import Thing from './lib/Thing.svelte'
  import Awaiting from './lib/Awaiting.svelte'

  let numbers = [1, 2, 3, 4];
  
  function addNumber() {
    numbers.push(numbers.length + 1);
    numbers = numbers
  }

  $: sum = numbers.reduce((t, n) => t + n, 0);
  $: x = numbers.length
  const pkg = {
		name: 'svelte',
		version: 3,
		speed: 'blazing',
		website: 'https://svelte.dev'
	};
  let user = {loggedIn : false};
  let userStatus;

  function toggle(){
    user.loggedIn = !user.loggedIn;
    if (user.loggedIn){
    userStatus = `He has logged In`
    } else {
      userStatus = `He has logged Out`
    }
  }

  let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' },
	];

  function handleClick() {
		things = things.slice(1);
	}

  function lastClick() {
		things.pop()
    things = things
	}
  let m = { x: 0, y: 0 };

  function handleMousemove(event) {
    m.x = event.clientX;
    m.y = event.clientY;
  }

  import Inner from './lib/Inner.svelte';

  function handleMessage(event) {
    alert(event.detail.text);
  }

  import Outer from './lib/Outer.svelte';

  function handleNewMessage(event) {
    alert(event.detail.text);
  }

  import CustomButton from './lib/CustomButton.svelte';

  function handleCustomClick() {
    alert('Button Clicked in the Main Page...');
  }

  function newClick() {
    alert('Reusing the button for New Click...');
  }
</script>

<main>
  <CustomButton buttonName={'clickety Clik'} on:click={handleCustomClick}/>

  <CustomButton buttonName={'newAdded'} on:click={newClick}/>

  <Outer on:message={handleNewMessage}/>

  <Inner on:message={handleMessage}/>
  <div>
    <a href="https://vitejs.dev" target="_blank"> 
      <img src="/vite.svg" class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank"> 
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>

  <Awaiting/>

  <div on:mousemove={handleMousemove}>
    The mouse position is {m.x} , {m.y}
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

  {#if user.loggedIn} //#Logic beginner 
	<button on:click={toggle}>
		Log out
	</button>
  {:else} //: is the continuation marker
    <button on:click={toggle}>
      Log in
    </button>
  {/if}
  <p>Is the user logged in? {userStatus}</p>

  <p>{numbers.join(' + ')} = {sum}</p>

  {#if x < 5}
    <p> Elements are increasing in size and length</p>
  {:else if x < 10}
    <p> Hey stop clicking that button... will ya?</p>
  {:else if x < 15}
    <p>Thats it the world ends in 3 days...</p>
  {:else}
    <p>Thats it the world ends</p>
  {/if}

  {#each numbers as num }
    <li>This is the {num} number</li>
  {/each}

  <button on:click={addNumber}>
    Add a number
  </button>

  <p>
   <strong> A simple rule of thumb:</strong> the updated variable must directly appear on the left hand side of the assignment.
  </p>
  
  {#each things as thing(thing.id)} // You can use any object as the key, as Svelte uses a Map internally — in other words you could do (thing) instead of (thing.id). Using a string or number is generally safer,
	  <Thing name={thing.name}/>
  {/each}

  <button on:click={handleClick}>
    Remove first thing
  </button>

  <button on:click={lastClick}>
    Remove last thing
  </button>

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
