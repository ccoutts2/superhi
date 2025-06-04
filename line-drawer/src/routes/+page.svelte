<script lang="ts">
	import '../lib/styles.css';
	import Scene from '$lib/components/Scene.svelte';
	import { Canvas } from '@threlte/core';
	import { Sky } from '@threlte/extras';
</script>

<Canvas>
	<Sky elevation={8} />
	<Scene />
</Canvas>
