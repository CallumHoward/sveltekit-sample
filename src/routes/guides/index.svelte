<script context="module" lang="ts">
	import type { LoadEvent } from '@sveltejs/kit';
	import type { Guide } from 'src/types/guide';

	export async function load({ fetch }: LoadEvent) {
		await new Promise(resolve => setTimeout(resolve, 1000))
		const res = await fetch('https://jsonplaceholder.typicode.com/posts');
		const guides = await res.json();

		if (res.ok) {
			return {
				props: {
					guides
				}
			};
		}

		return {
			status: res.status,
			error: new Error('Could not fetch guides')
		};
	}
</script>

<script lang="ts">
	export let guides: Guide[];
</script>

<div class="guides">
	<ul>
		{#each guides as guide}
			<li>
				<a sveltekit:prefetch href={`/guides/${guide.id}`}>{guide.title}</a>
			</li>
		{/each}
	</ul>
</div>

<style>
	.guides {
		margin-top: 20px;
	}
	ul {
		list-style-type: none;
		padding: 0;
	}
	a {
		display: inline-block;
		margin-top: 10px;
		padding: 10px;
		border: 1px dotted rgba(255, 255, 255, 0.2);
	}
</style>
