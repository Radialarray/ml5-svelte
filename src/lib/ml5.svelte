<script>
	import { onMount } from 'svelte';

	export let domElement;
	export let mlSketch;

	function modelReady() {
		console.log('Model ready!');
	}

	onMount(async () => {
		const ml5 = await import('ml5');
		console.log('ml5 version:', ml5.version);

		mlSketch(domElement, ml5, modelReady);
	});
</script>
