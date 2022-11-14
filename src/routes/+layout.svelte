<script>
	import { getPages } from '$lib/utility'; // Path to the utility function

	const { url } = import.meta;
	const modules = import.meta.glob('./**/*.svelte'); // Include subfolder
	// const modules = import.meta.glob('./**.svelte'); // Current folder only
	const pages = getPages(url, modules);

	const capitalize = (word) => {
		if (word.length === 0) {
			return '';
		} else {
			return word[0].toUpperCase() + word.slice(1);
		}
	};

	const navigation = pages.map((page) => {
		const name = capitalize(page.replace(/\//gm, ''));

		return { name: name, link: page };
	});
</script>

<nav>
	<ul>
		{#each navigation as element}
			<li>
				{#if element.link === '/'}
					<a href={element.link}>🤖</a>
				{:else}
					<a href={element.link}>{element.name}</a>
				{/if}
			</li>
		{/each}
	</ul>
</nav>

<main>
	<slot><!-- optional fallback --></slot>
</main>

<style type="postcss">
	nav ul {
		display: flex;
		gap: 16px;
		list-style: none;
		padding: 0;
		flex-wrap: wrap;
		align-items: center;
	}

	a {
		text-decoration: none;
	}
	:global(body) {
		max-width: 80%;
		margin-left: auto;
		margin-right: auto;
		font-family: sans-serif;
	}

	@media (max-width: 560px) {
		:global(body) {
			max-width: 100%;
			padding: 0 20px;
		}
	}
</style>
