<script>
	import { onMount } from 'svelte';
	import { request, gql } from 'graphql-request';

	const endpoint = 'https://rickandmortyapi.com/graphql';

	/**
	 * @type { import('../Character').default[] }
	 */
	let characters = [];

	const fetchCharacters = async () => {
		const query = gql`
			{
				characters {
					results {
						id
						name
						image
					}
				}
			}
		`;

		const data = await request(endpoint, query);
		characters = data.characters.results;
	};

	onMount(fetchCharacters);
</script>

<div class="grid">
	{#each characters as character}
		<div class="card">
			<img src={character.image} alt={character.name} />
			<h2>{character.name}</h2>
		</div>
	{/each}
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
		gap: 1rem;
		padding: 1rem;
	}

	.card {
		background-color: #fff;
		border-radius: 8px;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
		display: flex;
		flex-direction: column;
		overflow: hidden;
	}

	.card img {
		height: 250px;
		object-fit: cover;
		object-position: center;
	}

	.card h2 {
		font-size: 1.5rem;
		margin: 1rem;
	}
</style>
