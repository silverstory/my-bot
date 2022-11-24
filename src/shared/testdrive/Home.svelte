<script>
	import RedThing from './RedThing.svelte';
	import GreenThing from './GreenThing.svelte';
	import BlueThing from './BlueThing.svelte';
// 	import Gago, { component } from './Gago.svelte';
	import Super from './Super.svelte';

	const options = [
		{ color: 'red',   component: RedThing   },
		{ color: 'green', component: GreenThing },
		{ color: 'blue',  component: BlueThing  },
	];
	
	let ka = [{id: Math.random(), message: 'ka', component: RedThing},];

	let selected = options[0];
	
		function add() {
			ka = ka.concat({id: Math.random(), message: 'hello', component: selected.component });
	}
	
</script>

<select bind:value={selected}>
	{#each options as option}
		<option value={option}>{option.color}</option>
	{/each}
</select>

<button on:click={add}>add
</button>

{#each ka as a (a.id) }
	<Super component={a.component} message={a.message} id={a.id} />
{/each}

