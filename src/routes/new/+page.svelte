<script lang="ts">
	import { goto } from '$app/navigation';
	import hljs from 'highlight.js';

	let title = '';
	let code = '';
	let language = 'javascript';
	let category = '';
	let tagsInput = '';

	const languages = [
		'javascript', 'typescript', 'python', 'rust', 'go', 'java', 'c', 'cpp',
		'csharp', 'ruby', 'php', 'swift', 'kotlin', 'html', 'css', 'sql',
		'bash', 'json', 'yaml', 'markdown', 'gdscript', 'svelte', 'vue'
	];

	function saveSnippet() {
		if (!title.trim() || !code.trim()) {
			alert('Title and code are required');
			return;
		}

		const snippet = {
			id: crypto.randomUUID(),
			title: title.trim(),
			code: code.trim(),
			language,
			category: category.trim(),
			tags: tagsInput.split(',').map(t => t.trim()).filter(Boolean),
			created_at: new Date().toISOString()
		};

		const stored = localStorage.getItem('snippets');
		const snippets = stored ? JSON.parse(stored) : [];
		snippets.unshift(snippet);
		localStorage.setItem('snippets', JSON.stringify(snippets));

		goto('/');
	}

	function detectLanguage() {
		const detected = hljs.highlightAuto(code);
		if (detected.language && languages.includes(detected.language)) {
			language = detected.language;
		}
	}
</script>

<svelte:head>
	<title>New Snippet | Snippet Vault</title>
</svelte:head>

<div class="header">
	<h1>New Snippet</h1>
	<a href="/" class="back-link">← Back</a>
</div>

<form on:submit|preventDefault={saveSnippet} class="form">
	<div class="form-group">
		<label for="title">Title *</label>
		<input type="text" id="title" bind:value={title} placeholder="e.g., Fetch with timeout" required />
	</div>

	<div class="form-row">
		<div class="form-group">
			<label for="language">Language</label>
			<select id="language" bind:value={language}>
				{#each languages as lang}
					<option value={lang}>{lang}</option>
				{/each}
			</select>
		</div>

		<div class="form-group">
			<label for="category">Category</label>
			<input type="text" id="category" bind:value={category} placeholder="e.g., Utils, API, DOM" />
		</div>
	</div>

	<div class="form-group">
		<label for="code">Code *</label>
		<textarea 
			id="code" 
			bind:value={code} 
			placeholder="Paste your code here..."
			rows="12"
			required
		></textarea>
		<button type="button" on:click={detectLanguage} class="detect-btn">
			Auto-detect language
		</button>
	</div>

	<div class="form-group">
		<label for="tags">Tags (comma-separated)</label>
		<input type="text" id="tags" bind:value={tagsInput} placeholder="e.g., async, fetch, timeout" />
	</div>

	<div class="actions">
		<button type="submit" class="save-btn">Save Snippet</button>
		<a href="/" class="cancel-btn">Cancel</a>
	</div>
</form>

<style>
	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 2rem;
	}

	h1 {
		margin: 0;
		font-size: 1.8rem;
	}

	.back-link {
		color: #888;
		text-decoration: none;
	}

	.back-link:hover {
		color: #fff;
	}

	.form {
		max-width: 700px;
	}

	.form-group {
		margin-bottom: 1.25rem;
	}

	.form-row {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 1rem;
	}

	label {
		display: block;
		margin-bottom: 0.5rem;
		font-weight: 500;
		color: #aaa;
	}

	input, select, textarea {
		width: 100%;
		padding: 0.75rem 1rem;
		background: #1a1a1a;
		border: 1px solid #333;
		border-radius: 8px;
		color: #fff;
		font-size: 1rem;
		font-family: inherit;
	}

	textarea {
		font-family: 'Fira Code', 'JetBrains Mono', 'SF Mono', Consolas, monospace;
		resize: vertical;
	}

	input:focus, select:focus, textarea:focus {
		outline: none;
		border-color: #646cff;
	}

	.detect-btn {
		margin-top: 0.5rem;
		padding: 0.5rem 1rem;
		background: transparent;
		border: 1px solid #444;
		border-radius: 6px;
		color: #888;
		cursor: pointer;
		font-size: 0.85rem;
	}

	.detect-btn:hover {
		border-color: #646cff;
		color: #646cff;
	}

	.actions {
		display: flex;
		gap: 1rem;
		margin-top: 1.5rem;
	}

	.save-btn {
		padding: 0.75rem 2rem;
		background: #646cff;
		border: none;
		border-radius: 8px;
		color: #fff;
		font-size: 1rem;
		font-weight: 500;
		cursor: pointer;
	}

	.save-btn:hover {
		background: #535bf2;
	}

	.cancel-btn {
		padding: 0.75rem 1.5rem;
		background: transparent;
		border: 1px solid #444;
		border-radius: 8px;
		color: #888;
		text-decoration: none;
		display: flex;
		align-items: center;
	}

	.cancel-btn:hover {
		border-color: #666;
		color: #fff;
	}

	@media (max-width: 600px) {
		.form-row {
			grid-template-columns: 1fr;
		}
	}
</style>
