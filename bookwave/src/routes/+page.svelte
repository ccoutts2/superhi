<script lang="ts">
	import '../lib/styles.css';
	import { onMount } from 'svelte';
	import { Application, Assets, DisplacementFilter, Sprite } from 'pixi.js';
	import { RGBSplitFilter } from 'pixi-filters';

	let images: HTMLElement[];
	let app;
	let currentX: number = $state(0);
	let aimX: number = $state(0);
	let currentY: number = $state(0);
	let aimY: number = $state(0);

	onMount(async () => {
		images = Array.from(document.querySelectorAll('.images'));

		for (const image of images) {
			const originalImage = image.querySelector('img');
			const originalImageSource = originalImage?.getAttribute('src');
			if (!originalImageSource) continue;

			image.innerHTML = '';

			app = new Application();
			await app.init({
				width: 1100,
				height: 800,
				backgroundAlpha: 0
			});

			image.appendChild(app.canvas);

			const texture = await Assets.load(originalImageSource);
			const displacementTexture = await Assets.load('assets/displacement1.jpg');
			const sprite = new Sprite(texture);

			// Displacement asset image must be 2 to power of something for web gl to be able to wrap things.
			const displacementSprite = new Sprite(displacementTexture);

			let rgbFilter = new RGBSplitFilter();

			sprite.x = 100 + 450;
			sprite.y = 100 + 300;
			sprite.width = 900;
			sprite.height = 600;
			sprite.interactive = true;

			sprite.anchor.x = 0.5;
			sprite.anchor.y = 0.5;

			displacementSprite.width = 600;
			displacementSprite.height = 600;
			displacementSprite.texture.source.wrapMode = 'repeat';

			const displacementFilter = new DisplacementFilter({
				sprite: displacementSprite,
				scale: 60
			});

			sprite.filters = [displacementFilter, rgbFilter];

			app.stage.addChild(sprite);
			app.stage.addChild(displacementSprite);

			image.addEventListener('mousemove', (event: MouseEvent) => {
				aimX = event.pageX;
				aimY = event.pageY;
			});

			const animate = () => {
				const diffX = aimX - currentX;
				const diffY = aimY - currentY;

				currentX = currentX + diffX * 0.05;
				currentY = currentY + diffY * 0.05;

				if (displacementSprite) {
					displacementSprite.x = currentX;
					displacementSprite.y = displacementSprite.y + 1 + diffY * 0.02;

					rgbFilter.red = [diffX * 0.1, 0];
					rgbFilter.green = [0, diffY * 0.1];
					rgbFilter.blue = [2, 1];
				}

				requestAnimationFrame(animate);
			};

			animate();
		}
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
