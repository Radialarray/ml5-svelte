<script>
	import { onMount } from 'svelte';
	import P5 from 'p5-svelte';

	let video;
	let handpose;
	let hands = [];
	let sketch;
	let width = 640;
	let height = 480;

	onMount(async () => {
		const ml5 = await import('ml5');
		console.log('ml5 version:', ml5.version);

		sketch = (p5) => {
			p5.setup = () => {
				p5.createCanvas(width, height);
				video = p5.createCapture(p5.VIDEO);
				video.size(width, height);
				handpose = ml5.handpose(video, modelReady);

				// This sets up an event that fills the global variable "predictions"
				// with an array every time new hand poses are detected
				handpose.on('hand', (results) => {
					hands = results;
					if (hands.length > 0) {
						console.log(hands);
					}
				});

				// Hide the video element, and just show the canvas
				video.hide();
			};

			p5.draw = () => {
				p5.image(video, 0, 0, width, height);

				// We can call both functions to draw all keypoints and the skeletons
				drawKeypoints(p5);
			};
		};
	});

	function modelReady() {
		console.log('Model ready!');
	}

	// A function to draw ellipses over the detected keypoints
	function drawKeypoints(p5) {
		for (let i = 0; i < hands.length; i += 1) {
			const hand = hands[i];
			for (let j = 0; j < hand.landmarks.length; j += 1) {
				const keypoint = hand.landmarks[j];
				p5.fill(0, 255, 0);
				p5.noStroke();
				p5.ellipse(keypoint[0], keypoint[1], 10, 10);
			}
		}
	}
</script>

{#if hands.length > 0}
	<h1>Active</h1>
{:else}
	<h1>Not Active</h1>
{/if}

{#if sketch}
	<P5 {sketch} />
{/if}

<style>
</style>
