<script>
  let count = 0
  $: doubled = count * 2;
  const increment = () => {
    count += 5
  }
</script>

<button on:click={increment}>
  count is {count}
</button>
<div class="flex">
  <p class="w-50 mr2 bg-green tc"> 
    This is new counter : {count}. The anoter is doubled counter : {doubled}
    The above counter data is coming from the Counter.svelte while the App is only
    rendering it
  </p>

  <p class="w-50 mr2 bg-gold tc"> 
    Para in the left side has been rendered by using the tachyons Css imported at the 
    top
  </p>
</div>
