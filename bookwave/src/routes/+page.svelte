<script lang="ts">
	import '../lib/styles.css';
	import { onMount } from 'svelte';

	let images: HTMLElement[];

	onMount(() => {
		// @ts-ignore
		images = document.querySelectorAll('.images');
		images?.forEach((image: HTMLElement) => {
			const originalImage = image.querySelector('img');
			const originalImageSource = originalImage?.getAttribute('src');
			image.innerHTML = '';
		});
	});
</script>

<header>
	<h1>Learn to code now</h1>
	<p>Available December</p>
</header>

<div class="images">
	<img src="assets/book1.jpg" alt="" />
</div>

<div class="images">
	<img src="assets/book2.jpg" alt="" />
</div>

<div class="images">
	<img src="assets/book3.jpg" alt="" />
</div>
