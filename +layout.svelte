<script>
	import Header from './src/routes/Header.svelte';
	import './styles.css';
</script>



<div class="app">
	<Header />

	<main>
		<slot />
	</main>

	<!-- <footer>
	
	</footer> -->
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		position: sticky;
		top: 0;
		z-index: 100;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 64rem;
		margin: 0 auto;
		box-sizing: border-box;
		z-index: 0;
	}

	

	

</style>
