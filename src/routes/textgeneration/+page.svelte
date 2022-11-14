<script>
	import P5 from 'p5-svelte';
	import Ml5 from '$lib/ml5.svelte';
	let charRNN;
	let textInput;
	let tempSlider;
	let lengthSlider;
	let runningInference = true;
	let modelLoaded = 'Loading Model!';

	let generated = false;
	let last;

	let original;

	let sketch = (p5) => {
		p5.setup = () => {
			p5.noCanvas();
		};
	};

	const mlSketch = (domElement, ml5, modelReady) => {
		// Create the LSTM Generator passing it the model directory
		charRNN = ml5.charRNN('models/bolaño/', modelReady);
	};

	function modelReady() {
		modelLoaded = 'Model is loaded.';
		runningInference = false;
	}

	// Has 500 milliseconds passed since the last time a change was made?
	function checkGenerate() {
		const passed = millis() - last;
		if (passed > 500 && !generated) {
			generate();
			generated = true;
		}
	}

	// Update the time
	function changing() {
		generated = false;
		last = millis();
	}

	// Generate new text!
	function generate() {
		// Grab the original text
		original = textInput.value();
		// Make it to lower case
		const txt = original.toLowerCase();

		// prevent starting inference if we've already started another instance
		// or if there is no prompt
		// TODO: is there better JS way of doing this?
		if (!runningInference && txt.length > 0) {
			runningInference = true;

			// Update the status log
			select('#status').html('Generating...');

			// Update the length and temperature span elements
			select('#length').html(lengthSlider.value());
			select('#temperature').html(tempSlider.value());

			// Here is the data for the LSTM generator
			const data = {
				seed: txt,
				temperature: tempSlider.value(),
				length: lengthSlider.value()
			};

			// Generate text with the charRNN
			charRNN.generate(data, gotData);
		}
	}

	// Update the DOM elements with typed and generated text
	function gotData(err, result) {
		runningInference = false;
		if (err) {
			console.error(err);
			return;
		}
		select('#status').html('Ready!');
		select('#original').html(original);
		select('#prediction').html(result.sample);
	}
</script>

<h1>Interactive CharRNN Text Generation Example using p5.js</h1>
<h2>
	This example uses a pre-trained model on a corpus of <a
		href="https://en.wikipedia.org/wiki/Virginia_Woolf">Virginia Woolf</a
	>
</h2>
<textarea
	bind:value={textInput}
	id="textInput"
	style="width: 300px; height: 50px;"
	placeholder="type here"
/>
<br /> length:
<input bind:value={lengthSlider} id="lenSlider" type="range" min="1" max="100" />
<span id="length">20</span>
<br /> temperature:
<input bind:value={tempSlider} id="tempSlider" type="range" min="0.01" max="1" step="0.01" /><span
	id="temperature">0.5</span
>
<p id="status">{modelLoaded}</p>
<p id="result">
	<span style="font-weight:bold" id="original" /><span id="prediction" />
</p>

{#if sketch}
	<P5 {sketch} />
	<Ml5 {mlSketch} mlReady={modelReady} />
{/if}
