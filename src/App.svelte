<script>
    let count = 0;
    function handleCountClick (event) {
        console.log(event);
        count++;
    }
</script>

<div>
    <button on:click={handleCountClick}>Count {count}</button>
    <button on:click={(event) => handleCountClick(event)}>Count {count}</button>
</div>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	.inactive {
        opacity: 0.5;
    }

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>