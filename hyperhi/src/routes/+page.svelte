<script lang="ts">
	import '../lib/styles.css';
	import { onMount } from 'svelte';

	let cursor: HTMLElement | null;
	let canvasIn: HTMLElement | null;
	let canvasOut: HTMLElement | null;
	let isMouseDown: boolean = $state(false);

	onMount(() => {
		cursor = document.querySelector('div.cursor');
		canvasIn = document.querySelector('canvas.in');
		canvasOut = document.querySelector('canvas.out');

		document.addEventListener('mousedown', (event) => {
			isMouseDown = true;
			growCursor();
			startDraw(canvasIn, event.pageX, event.pageY);
			startDraw(canvasOut, event.pageX, event.pageY);
		});

		document.addEventListener('mouseup', () => {
			isMouseDown = false;
			shrinkCursor();
		});

		document.addEventListener('mousemove', (event) => {
			moveCursor(event.pageX, event.pageY);
			moveDraw(canvasIn, event.pageX, event.pageY);
			moveDraw(canvasOut, event.pageX, event.pageY);
		});

		if (canvasIn && canvasOut) {
			setUpCanvas(canvasIn);
			setUpCanvas(canvasOut);
		}

		window.addEventListener('resize', () => {
			setUpCanvas(canvasIn);
			setUpCanvas(canvasOut);
		});
	});

	const growCursor = () => {
		cursor?.classList.add('is-down');
	};

	const shrinkCursor = () => {
		cursor?.classList.remove('is-down');
	};

	const moveCursor = (x: number, y: number) => {
		cursor!.style.left = x + 'px';
		cursor!.style.top = y + 'px';
	};

	const setUpCanvas = (canvas: any) => {
		const w = window.innerWidth;
		const h = window.innerHeight;
		const dpi = window.devicePixelRatio;

		canvas.width = w * dpi;
		canvas.height = h * dpi;

		canvas.style.width = w + 'px';
		canvas.style.height = h + 'px';

		const context = canvas.getContext('2d');
		context.scale(dpi, dpi);

		if (canvas.classList.contains('out')) {
			context.fillStyle = '#ffffff';
			context.strokeStyle = '000000';
		} else {
			context.fillStyle = '#0000000';
			context.strokeStyle = 'ffffff';
		}

		context.lineWidth = 100;
		context.lineCap = 'round';
		context.lineJoin = 'round';

		context.shadowBlur = 20;
		context.shadowColor = context.strokeStyle;

		context.rect(0, 0, w, h);
		context.fill();
	};

	const startDraw = (canvas: any, x: number, y: number) => {
		const context = canvas.getContext('2d');

		context.moveTo(x, y);
		context.beginPath();
	};

	const moveDraw = (canvas: any, x: number, y: number) => {
		const context = canvas.getContext('2d');

		if (isMouseDown) {
			context.lineTo(x, y);
			context.stroke();
		}
	};
</script>

<div class="scratched-in">
	<div class="pattern"></div>
	<canvas class="in"></canvas>
</div>

<div class="scratched-out">
	<div class="line-up">
		<img src="/lineup.svg" alt="" />
	</div>
	<canvas class="out"></canvas>
</div>

<div class="cursor"></div>
