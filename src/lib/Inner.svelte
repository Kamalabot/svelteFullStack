<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello from Inner.Svelte!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>