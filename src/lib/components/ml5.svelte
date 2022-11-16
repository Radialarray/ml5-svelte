<script>
	import { onMount } from 'svelte';

	export let domElement = null;
	export let mlSketch = null;
	export let mlReady = null;

	const modelReady = () => {
		console.log('Model ready!');
	};

	onMount(async () => {
		if (mlReady === (undefined || null)) {
			mlReady = modelReady;
		}

		const ml5 = await import('ml5');
		console.log('ml5 version:', ml5.version);
		mlSketch(domElement, ml5, mlReady);
	});
</script>
