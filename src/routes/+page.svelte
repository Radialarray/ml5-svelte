<h1>ML5 with P5 in Svelte 🎉</h1>

<p>The navigation shows some examples on how to use ML5 models with Svelte.</p>
<p>
	The ml5 library is extracted into a component which defers the import until the component is
	mounted to the dom and window is available. The <i>ml5</i> sketch can be loaded to a mlSketch prop
	as well as the dom-element to which it should refer and an optional callback, which is executed when
	the model is loaded.
</p>
