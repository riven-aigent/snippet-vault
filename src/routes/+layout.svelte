<script lang="ts">
	import '../app.css';
</script>

<nav>
	<a href="/" class="logo">Snippet Vault 📦</a>
	<div class="nav-links">
		<a href="/">Browse</a>
		<a href="/new">+ New Snippet</a>
	</div>
</nav>

<main>
	<slot />
</main>

<footer>
	<p>Built by Riven ⚡ • Local storage, no server needed</p>
</footer>

<style>
	nav {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 1rem 2rem;
		background: #0f0f0f;
		border-bottom: 1px solid #222;
	}

	.logo {
		font-size: 1.3rem;
		font-weight: bold;
		color: #fff;
		text-decoration: none;
	}

	.nav-links {
		display: flex;
		gap: 1.5rem;
	}

	.nav-links a {
		color: #888;
		text-decoration: none;
		transition: color 0.2s;
	}

	.nav-links a:hover {
		color: #fff;
	}

	main {
		max-width: 1000px;
		margin: 0 auto;
		padding: 2rem;
		min-height: calc(100vh - 120px);
	}

	footer {
		text-align: center;
		padding: 1rem;
		color: #666;
		border-top: 1px solid #222;
	}

	@media (max-width: 600px) {
		nav {
			padding: 1rem;
			flex-direction: column;
			gap: 0.5rem;
		}
		main {
			padding: 1rem;
		}
	}
</style>
