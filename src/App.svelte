<script>
  import svelteLogo from './assets/svelte.svg'
  import Counter from './lib/Counter.svelte'
  import Nested from './lib/Nested.svelte';
  import Info from './lib/info.svelte' //So even if the file name has smaller letters, the imported class matters
  import Thing from './lib/Thing.svelte'
  import Awaiting from './lib/Awaiting.svelte'
  import Keypad from './lib/Keypad.svelte'

	let pin;
	$: view = pin ? pin.replace(/\d(?!$)/g, '•') : 'enter your pin';

	function handlePin() {
		alert(`submitted ${pin}`);
	}

  let name = 'New'
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
  let a;
  let yes;

  let menu = [
	'Cookies and cream',
	'Mint choc chip',
	'Raspberry ripple'
  ];
  const emojis = {
        apple: "🍎",
        banana: "🍌",
        carrot: "🥕",
        doughnut: "🍩",
        egg: "🥚"
	}

  let scoops = 1;
	let flavours = ['Mint choc chip'];

	function join(flavours) {
		if (flavours.length === 1) return flavours[0];
		return `${flavours.slice(0, -1).join(', ')} and ${flavours[flavours.length - 1]}`;
	}

  let value;

  let questions = [
		{ id: 1, text: `Where did you go to school?` },
		{ id: 2, text: `What is your mother's name?` },
		{ id: 3, text: `What is another personal fact that an attacker could easily find with Google?` }
	];

	let selected;

	let answer = '';

	function handleSubmit() {
		alert(`answered question ${selected.id} (${selected.text}) with "${answer}"`);
	}

  let todos = [
		{ done: false, text: 'finish Svelte tutorial' },
		{ done: false, text: 'build an app' },
		{ done: false, text: 'world domination' }
	];

	function add() {
		todos = todos.concat({ done: false, text: '' });
	}

	function clear() {
		todos = todos.filter(t => !t.done);
	}

	$: remaining = todos.filter(t => !t.done).length;

  	// These values are bound to properties of the video
	let time = 0;
	let duration;
	let paused = true;

	let showControls = true;
	let showControlsTimeout;

	// Used to track time of last mouse down event
	let lastMouseDown;

	function handleMove(e) {
		// Make the controls visible, but fade out after
		// 2.5 seconds of inactivity
		clearTimeout(showControlsTimeout);
		showControlsTimeout = setTimeout(() => showControls = false, 2500);
		showControls = true;

		if (!duration) return; // video not loaded yet
		if (e.type !== 'touchmove' && !(e.buttons & 1)) return; // mouse not down

		const clientX = e.type === 'touchmove' ? e.touches[0].clientX : e.clientX;
		const { left, right } = this.getBoundingClientRect();
		time = duration * (clientX - left) / (right - left);
	}

	// we can't rely on the built-in click event, because it fires
	// after a drag — we have to listen for clicks ourselves
	function handleMousedown(e) {
		lastMouseDown = new Date();
	}

	function handleMouseup(e) {
		if (new Date() - lastMouseDown < 300) {
			if (paused) e.target.play();
			else e.target.pause();
		}
	}

	function format(seconds) {
		if (isNaN(seconds)) return '...';

		const minutes = Math.floor(seconds / 60);
		seconds = Math.floor(seconds % 60);
		if (seconds < 10) seconds = '0' + seconds;

		return `${minutes}:${seconds}`;
	}

  let w;
	let h;
	let size = 42;
	let text = 'edit me';

  import { onMount } from 'svelte';

	let canvas;

	onMount(() => {
		const ctx = canvas.getContext('2d');
		let frame = requestAnimationFrame(loop);

		function loop(t) {
			frame = requestAnimationFrame(loop);

			const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
      //console.log(imageData)
			for (let p = 0; p < imageData.data.length; p += 4) {
				const i = p / 4;
				const x = i % canvas.width;
				const y = i / canvas.width >>> 0;

				const r = 64 + (128 * x / canvas.width) + (64 * Math.sin(t / 1000));
				const g = 64 + (128 * y / canvas.height) + (64 * Math.cos(t / 1000));
				const b = 128;

				imageData.data[p + 0] = r;
				imageData.data[p + 1] = g;
				imageData.data[p + 2] = b;
				imageData.data[p + 3] = 255;
			}

			ctx.putImageData(imageData, 0, 0);
		}

		return () => {
			cancelAnimationFrame(frame);
		};
	});
</script>

<main>

  <h1 style="color: {pin ? '#333' : '#ccc'}">{view}</h1>

  <Keypad bind:value={pin} on:submit={handlePin}/>

  <input type=range bind:value={size}>
  <input bind:value={text}>
  
  <p>size: {w}px x {h}px</p>

  <div bind:clientWidth={w} bind:clientHeight={h}>
    <span style="font-size: {size}px">{text}</span>
  </div>
<video
	poster="https://sveltejs.github.io/assets/caminandes-llamigos.jpg"
	src="https://sveltejs.github.io/assets/caminandes-llamigos.mp4"
	on:mousemove={handleMove}
	on:touchmove|preventDefault={handleMove}
	on:mousedown={handleMousedown}
	on:mouseup={handleMouseup}
	bind:currentTime={time}
	bind:duration
	bind:paused>
	<track kind="captions">
</video>


<h1>Todos</h1>

{#each todos as todo}
	<div class:done={todo.done}>
		<input
			type=checkbox
			checked={todo.done}
		>

		<input
			placeholder="What needs to be done?"
			value={todo.text}
		>
	</div>
{/each}

<p>{remaining} remaining</p>

<button on:click={add}>
	Add new
</button>

<button on:click={clear}>
	Clear completed
</button>


  <form on:submit|preventDefault={handleSubmit}>
    <select value={selected} on:change="{() => answer = ''}">
      {#each questions as question}
        <option value={question}>
          {question.text}
        </option>
      {/each}
    </select>

    
	<input bind:value={answer}>

	<button disabled={!answer} type=submit>
		Submit
	</button>

  <p>selected question {selected ? selected.id : '[waiting...]'}</p>

  <h2>Flavours</h2>
  <textarea bind:value></textarea>
  <select multiple bind:value={flavours}>
    {#each menu as flavour}
      <option value={flavour}>
        {flavour}
      </option>
	  {/each}
  </select>

  {#each menu as flavour}
    <label>
      <input type=radio bind:group={flavours} name="flavours" value={flavour}>
      {flavour}
    </label>
  {/each}
  <input bind:value="{name}">

  <input type=number bind:value={a} min=0 max=2>
  <input type=range bind:value={a} min=0 max=2>

  <input type=checkbox bind:checked={yes}>

  <p>
    Using the above inputs we have create {a} lines of text.We can see whether the checkbox is checked = {yes}.
    <Thing name={menu[a]}/>
  </p>

  <CustomButton buttonName={'clickety Clik'} on:click={handleCustomClick}/>

  <CustomButton buttonName={'newAdded'} on:click={newClick}/>

  <Outer on:message={handleNewMessage}/>

  <Inner on:message={handleMessage}/>
  <div>
    As a general rule, data flow in Svelte is top down — a parent component can set props on a child component, and a component can set attributes on an element, but not the other way around.
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
    This is a {name} environment to work.
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
  
  <!-- // You can use any object as the key, as Svelte uses a Map internally — in other words you could do (thing) instead of (thing.id). Using a string or number is generally safer, -->

  {#each things as thing(thing.id)} 
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
  .done {
      opacity: 0.4;
    }
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
  canvas {
		width: 100%;
		height: 100%;
		background-color: #666;
		-webkit-mask: url(https://svelte.dev/tutorial/svelte-logo-mask.svg) 50% 50% no-repeat;
		mask: url(https://svelte.dev/tutorial/svelte-logo-mask.svg) 50% 50% no-repeat;
	}
</style>
