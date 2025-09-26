<script>
	import { onMount, onDestroy } from 'svelte';
	const speed = 0.05;
	let rotation = 0;
	let frame;

	onMount(() => {
		const animate = () => {
			rotation += speed;
			frame = requestAnimationFrame(animate); // Create a continuous loop
		};
		
		animate(); // Start the loop
	});

	onDestroy(() => {
		cancelAnimationFrame(frame); // Stop the animation loop
	});
</script>

<main>
	<div class="img" style:transform="translateY(-50%) rotate({rotation}deg)">
		<img src="/mishran-high.png" alt="" />
	</div>
</main>

<style>
	.img {
		position: absolute;
		top: 0;
		width: 100vw;
		transform: translateY(-50%);
	}
	img {
		width: 100%;
		height: 100%;
	}
</style>