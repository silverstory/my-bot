<script context="module">
	
</script>

<script>
	export let component;
	export let message;
	export let id;
</script>

<h1>
	{message} {id}
</h1>

<svelte:component this={component}/>