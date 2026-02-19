<script lang="ts">
	import hljs from 'highlight.js';
	import { onMount } from 'svelte';

	export let snippet: {
		id: string;
		title: string;
		code: string;
		language: string;
		category: string;
		tags: string[];
		created_at: string;
	};
	export let deleteSnippet: (id: string) => void;

	let codeRef: HTMLElement;
	let copied = false;

	onMount(() => {
		if (codeRef) {
			hljs.highlightElement(codeRef);
		}
	});

	function copyCode() {
		navigator.clipboard.writeText(snippet.code);
		copied = true;
		setTimeout(() => copied = false, 2000);
	}
</script>

<article class="snippet-card">
	<header>
		<div class="title-row">
			<h3>{snippet.title}</h3>
			<div class="actions">
				<button on:click={copyCode} class="copy-btn">
					{copied ? '✓ Copied' : 'Copy'}
				</button>
				<button on:click={() => deleteSnippet(snippet.id)} class="delete-btn">×</button>
			</div>
		</div>
		<div class="meta">
			<span class="language">{snippet.language}</span>
			{#if snippet.category}
				<span class="category">{snippet.category}</span>
			{/if}
		</div>
	</header>

	<pre><code bind:this={codeRef} class="language-{snippet.language}">{snippet.code}</code></pre>

	{#if snippet.tags.length > 0}
		<footer class="tags">
			{#each snippet.tags as tag}
				<span class="tag">{tag}</span>
			{/each}
		</footer>
	{/if}
</article>

<style>
	.snippet-card {
		background: #1a1a1a;
		border: 1px solid #222;
		border-radius: 12px;
		overflow: hidden;
	}

	header {
		padding: 1rem;
		border-bottom: 1px solid #222;
	}

	.title-row {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		gap: 1rem;
	}

	h3 {
		margin: 0;
		font-size: 1.1rem;
	}

	.actions {
		display: flex;
		gap: 0.5rem;
	}

	.copy-btn, .delete-btn {
		padding: 0.4rem 0.8rem;
		background: #333;
		border: none;
		border-radius: 6px;
		color: #fff;
		cursor: pointer;
		font-size: 0.85rem;
		transition: background 0.2s;
	}

	.copy-btn:hover {
		background: #646cff;
	}

	.delete-btn {
		padding: 0.4rem 0.6rem;
		background: transparent;
		color: #666;
		font-size: 1.2rem;
	}

	.delete-btn:hover {
		color: #ff4444;
		background: rgba(255, 68, 68, 0.1);
	}

	.meta {
		display: flex;
		gap: 0.5rem;
		margin-top: 0.5rem;
	}

	.language, .category {
		font-size: 0.75rem;
		padding: 0.2rem 0.6rem;
		border-radius: 4px;
		background: #333;
		color: #aaa;
	}

	.category {
		background: rgba(100, 108, 255, 0.2);
		color: #646cff;
	}

	pre {
		margin: 0;
		padding: 1rem;
		overflow-x: auto;
	}

	.tags {
		padding: 0.75rem 1rem;
		display: flex;
		gap: 0.5rem;
		flex-wrap: wrap;
		border-top: 1px solid #222;
	}

	.tag {
		font-size: 0.75rem;
		padding: 0.2rem 0.5rem;
		background: #222;
		border-radius: 4px;
		color: #888;
	}
</style>
