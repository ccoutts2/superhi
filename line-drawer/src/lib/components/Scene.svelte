<script lang="ts">
	import { T } from '@threlte/core';
	import { useGltf, OrbitControls, useGltfAnimations } from '@threlte/extras';

	const gltf = useGltf('/assets/LittlestTokyo.glb');
	const { actions } = useGltfAnimations<'Take 001'>(gltf);

	$effect(() => {
		$actions['Take 001']?.play();
	});
</script>

<T.PerspectiveCamera makeDefault fov={40} position={[5, 2, 8]}>
	<OrbitControls
		autoRotate
		autoRotateSpeed={0.2}
		enableDamping
		enableZoom={false}
		target={[0, 0.5, 0]}
	/>
</T.PerspectiveCamera>

{#await gltf then { scene }}
	<T is={scene} position={[1, 1, 0]} scale={0.01} />
{/await}
