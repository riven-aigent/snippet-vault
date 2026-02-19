<script lang="ts">
	import { onMount } from 'svelte';
	import SnippetCard from './SnippetCard.svelte';

	interface Snippet {
		id: string;
		title: string;
		code: string;
		language: string;
		category: string;
		tags: string[];
		created_at: string;
	}

	let snippets: Snippet[] = [];
	let categories: string[] = [];
	let selectedCategory = '';
	let searchQuery = '';

	onMount(() => {
		loadSnippets();
	});

	function loadSnippets() {
		const stored = localStorage.getItem('snippets');
		if (stored) {
			snippets = JSON.parse(stored);
			categories = [...new Set(snippets.map(s => s.category).filter(Boolean))];
		}
	}

	$: filteredSnippets = snippets.filter(s => {
		const matchesCategory = !selectedCategory || s.category === selectedCategory;
		const matchesSearch = !searchQuery || 
			s.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
			s.code.toLowerCase().includes(searchQuery.toLowerCase()) ||
			s.tags.some(t => t.toLowerCase().includes(searchQuery.toLowerCase()));
		return matchesCategory && matchesSearch;
	});

	function deleteSnippet(id: string) {
		if (confirm('Delete this snippet?')) {
			snippets = snippets.filter(s => s.id !== id);
			localStorage.setItem('snippets', JSON.stringify(snippets));
		}
	}
</script>

<svelte:head>
	<title>Snippet Vault</title>
</svelte:head>

<div class="header">
	<h1>Your Snippets</h1>
	<p class="subtitle">{snippets.length} snippets saved locally</p>
</div>

<div class="filters">
	<input 
		type="text" 
		placeholder="Search snippets..." 
		bind:value={searchQuery}
		class="search-input"
	/>
	{#if categories.length > 0}
		<select bind:value={selectedCategory} class="category-select">
			<option value="">All Categories</option>
			{#each categories as cat}
				<option value={cat}>{cat}</option>
			{/each}
		</select>
	{/if}
</div>

{#if filteredSnippets.length === 0}
	<div class="empty">
		{#if snippets.length === 0}
			<p>No snippets yet. <a href="/new">Create your first snippet</a></p>
		{:else}
			<p>No snippets match your search.</p>
		{/if}
	</div>
{:else}
	<div class="snippets-grid">
		{#each filteredSnippets as snippet}
			<SnippetCard {snippet} {deleteSnippet} />
		{/each}
	</div>
{/if}

<style>
	.header {
		margin-bottom: 2rem;
	}

	h1 {
		margin: 0 0 0.25rem;
		font-size: 1.8rem;
	}

	.subtitle {
		color: #666;
		margin: 0;
	}

	.filters {
		display: flex;
		gap: 1rem;
		margin-bottom: 1.5rem;
		flex-wrap: wrap;
	}

	.search-input {
		flex: 1;
		min-width: 200px;
		padding: 0.75rem 1rem;
		background: #1a1a1a;
		border: 1px solid #333;
		border-radius: 8px;
		color: #fff;
		font-size: 1rem;
	}

	.search-input:focus {
		outline: none;
		border-color: #646cff;
	}

	.category-select {
		padding: 0.75rem 1rem;
		background: #1a1a1a;
		border: 1px solid #333;
		border-radius: 8px;
		color: #fff;
		font-size: 1rem;
		min-width: 150px;
	}

	.empty {
		text-align: center;
		padding: 3rem;
		color: #666;
	}

	.snippets-grid {
		display: grid;
		gap: 1rem;
	}
</style>
