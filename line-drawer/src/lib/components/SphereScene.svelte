<script lang="ts">
	import '../styles.css';
	import { T, useTask } from '@threlte/core';
	import { Spring } from 'svelte/motion';
	import { interactivity, OrbitControls } from '@threlte/extras';

	interactivity();
	const scale = new Spring(1);
	let rotation = 0;

	useTask((delta) => {
		rotation += delta * 0.5;
	});
</script>

<T.PerspectiveCamera makeDefault position={[14, 10, 0]}>
	<OrbitControls autoRotate enableDamping />
</T.PerspectiveCamera>

<T.DirectionalLight position={[10, 20, 10]} intensity={2} castShadow />
<T.AmbientLight color="#d63fff" />

<T.Mesh
	scale={scale.current}
	onpointerenter={() => (scale.target = 0.5)}
	onpointerleave={() => (scale.target = 1)}
	rotation.y={rotation}
>
	<T.SphereGeometry args={[2, 32, 32]} />
	<T.MeshStandardMaterial color="hotpink" wireframe />
</T.Mesh>
