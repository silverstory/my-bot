<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function popAssistance() {
		dispatch('introassistance', {
			text: 'Hello! Assistance'
		});
	}
	function popComplaint() {
		dispatch('introcomplaint', {
			text: 'Hello! Complaint'
		});
	}
</script>
<div class="content-between flex space-x-4 justify-center items-center h-20">
    <button on:click={popAssistance} class="relative inline-flex items-center justify-start inlin\e-block px-5 py-3 overflow-hidden font-medium transition-all bg-blue-600 rounded-full hover:bg-white group">
        <span class="absolute inset-0 border-0 group-hover:border-[25px] ease-linear duration-100 transition-all border-white rounded-full"></span>
        <span class="relative w-full text-left text-white transition-colors duration-200 ease-in-out group-hover:text-blue-600">Assistance</span>
    </button>

    <button on:click={popComplaint} class="relative inline-flex items-center justify-start inline-block px-5 py-3 overflow-hidden font-bold rounded-full group">
        <span class="w-32 h-32 rotate-45 translate-x-12 -translate-y-2 absolute left-0 top-0 bg-white opacity-[3%]"></span>
        <span class="absolute top-0 left-0 w-48 h-48 -mt-1 transition-all duration-500 ease-in-out rotate-45 -translate-x-56 -translate-y-24 bg-white opacity-100 group-hover:-translate-x-8"></span>
        <span class="relative w-full text-left text-black/70 transition-colors duration-200 ease-in-out group-hover:text-gray-900">Complaint</span>
        <span class="absolute inset-0 border-2 border-black/20 rounded-full"></span>
    </button>
</div>