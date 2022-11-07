<script>
	import { onMount } from 'svelte';
	let active = false;

	let canvas;
	let video;
	let ctx;
	let webcamWidth;
	let webcamHeight;
	onMount(async () => {
		const p5 = await import('p5');
		const ml5 = await import('ml5');
		console.log('ml5 version:', ml5.version);

		ctx = canvas.getContext('2d');

		if (navigator.getUserMedia) {
			navigator.getUserMedia(
				{
					video: true,
					audio: false
				},

				function (stream) {
					webcamWidth = stream.getVideoTracks()[0].getSettings().width;
					webcamHeight = stream.getVideoTracks()[0].getSettings().height;
					canvas.setAttribute('width', webcamWidth);
					canvas.setAttribute('height', webcamHeight);

					// video.src = window.URL.createObjectURL(localMediaStream);
					video.srcObject = stream;
				},

				function (err) {
					console.log(err);
				}
			);
		} else {
			console.log('getUserMedia not supported by your browser');
		}

		const handpose = ml5.handpose(video, modelReady);

		// This sets up an event that fills the global variable "predictions"
		// with an array every time new hand poses are detected
		handpose.on('hand', (results) => {
			const hands = results;
			if (hands.length > 0) {
				console.log(hands);
			}
		});
	});

	function modelReady() {
		console.log('Model ready!');
	}
</script>

{#if active}
	<h1>Active</h1>
{:else}
	<h1>Not Active</h1>
{/if}

<canvas bind:this={canvas} />
<!-- svelte-ignore a11y-media-has-caption -->
<video bind:this={video} autoplay />

<style>
</style>
