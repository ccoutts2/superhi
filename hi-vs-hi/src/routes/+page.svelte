<script lang="ts">
	import '../lib/styles.css';
	import { onMount } from 'svelte';
	import Matter from 'matter-js';
	import pkg from 'matter-js';
	const { Runner } = pkg;

	let container: HTMLElement;
	let width: number;
	let height: number;

	const { Engine, Render, Bodies, World, Mouse, MouseConstraint, Composites, Query } = Matter;

	onMount(() => {
		container = document.querySelector('.container') as HTMLElement;

		width = window.innerWidth;
		height = window.innerHeight;

		const engine = Engine.create();
		const renderer = Render.create({
			element: container,
			engine: engine,
			options: {
				height: height,
				width: width,
				background: '#000000',
				wireframes: false,
				pixelRatio: window.devicePixelRatio
			}
		});

		const bigBall = Bodies.circle(width / 2, height / 2, Math.min(width / 4, height / 4), {
			isStatic: true,
			render: {
				fillStyle: '#ffffff'
			}
		});

		const wallOptions = {
			render: {
				visible: false
			},
			isStatic: true
		};

		const wallThickness = 100;

		const ground = Bodies.rectangle(width / 2, height, width + 100, 50, wallOptions);
		const ceiling = Bodies.rectangle(width / 2, -50, width + 100, 50, wallOptions);
		const leftWall = Bodies.rectangle(
			-wallThickness / 2,
			height / 2,
			wallThickness,
			height,
			wallOptions
		);
		const rightWall = Bodies.rectangle(
			width + wallThickness / 2,
			height / 2,
			wallThickness,
			height,
			wallOptions
		);

		const mouse = Mouse.create(container);
		const mouseControl = MouseConstraint.create(engine, {
			mouse: mouse,
			constraint: {
				render: {
					visible: false
				}
			}
		});

		const initialShapes = Composites.stack(50, 50, 15, 5, 40, 40, (x: number, y: number) => {
			return createShape(x, y);
		});

		World.add(engine.world, [
			bigBall,
			ground,
			ceiling,
			leftWall,
			rightWall,
			mouseControl,
			initialShapes
		]);

		document.addEventListener('click', (event: MouseEvent) => {
			const shape = createShape(event.pageX, event.pageY);
			initialShapes.bodies.push(shape);
			World.add(engine.world, shape);
		});

		Render.run(renderer);

		const runner = Runner.create();

		Runner.run(runner, engine);
	});

	const createShape = (x: number, y: number) => {
		const randomNum = Math.random();

		if (randomNum > 0.5) {
			return Bodies.rectangle(x, y, 38, 50, {
				render: {
					sprite: {
						texture: 'outline-2x.png',
						xScale: 0.5,
						yScale: 0.5
					}
				}
			});
		} else {
			return Bodies.circle(x, y, 25, {
				render: {
					sprite: {
						texture: 'ball.png',
						xScale: 0.5,
						yScale: 0.5
					}
				}
			});
		}
	};
</script>

<div class="container" bind:this={container}></div>
<div class="split"></div>
