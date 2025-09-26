<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
</script>

<main>
	<div class="placeholder"></div>
	<div class="content">
		<h1>RECORDING</h1>
	</div>
	<div class="placeholder">
		<div class="placeholder__action">
            <!-- This button now dispatches a 'stop' event when clicked -->
			<button class="btn--outline" on:click={() => dispatch('stop')}>STOP</button>
		</div>
	</div>
</main>

<style>
	main {
		background: #ff3434;
		height: 100vh;
		color: white;
		display: flex;
		flex-direction: column;
	}
	.placeholder {
		flex: 1;
	}
	.content h1 {
		text-align: center;
		font-size: 1rem;
		font-weight: 700;
	}
	.placeholder:last-child {
		display: flex;
		align-items: flex-end;
	}
	.placeholder__action {
		margin: 1rem;
		width: 100%;
	}
</style>
