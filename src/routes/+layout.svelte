<script>
	import { getPages } from '$lib/utility'; // Path to the utility function

	const { url } = import.meta;
	const modules = import.meta.glob('./**/*.svelte'); // Include subfolder
	// const modules = import.meta.glob('./**.svelte'); // Current folder only

	const pages = getPages(url, modules);
</script>

<nav>
	<ul>
		{#each pages as page}
			<li>
				<a href={page}>{page}</a>
			</li>
		{/each}
	</ul>
</nav>

<main>
	<slot><!-- optional fallback --></slot>
</main>

<style>
	nav ul {
		display: flex;
		gap: 12px;
		list-style: none;
	}
</style>
