<script lang="ts">
	import { Canvas, InteractiveObject, OrbitControls, T } from '@threlte/core'
	import { spring } from 'svelte/motion'
	import { degToRad } from 'three/src/math/MathUtils'
	import { GLTF, Text } from '@threlte/extras'

	const scalen = spring(1), scalep = spring(1)

	const listurl = 'https://raw.githubusercontent.com/Joshimello/EversoulScrape/main/weapon_list.json'
	const request = new XMLHttpRequest()
    request.open('GET', listurl, false)
    request.send(null)

	let list = JSON.parse(request.responseText)
	let baseurl = 'https://raw.githubusercontent.com/Joshimello/EversoulScrape/main/weapons/'
	
	let index = 0
	let url = baseurl + list[index]

</script>

<span>{list[index]}</span>

<button class="prev" on:click={() => (url = baseurl + list[--index])}>{'<'}</button>
<button class="next" on:click={() => (url = baseurl + list[++index])}>{'>'}</button>

<Canvas>
	<T.PerspectiveCamera makeDefault position={[10, 5, 0]}>
		<OrbitControls autoRotate maxPolarAngle={degToRad(80)} />
	</T.PerspectiveCamera>

	<T.DirectionalLight castShadow position={[5, 10, 5]} />
	<T.DirectionalLight position={[-3, 10, -10]} intensity={0.2} />
	<T.AmbientLight intensity={0.2} />

	<T.Group rotation.y={degToRad(90)}>
		<GLTF castShadow receiveShadow url={url} scale={3} />
	</T.Group>

	<T.Mesh receiveShadow position.y={-2}>
		<T.CylinderGeometry args={[5, 5, 0.3, 72]} />
		<T.MeshStandardMaterial color="white" />
	</T.Mesh>
</Canvas>


<style>
	span {
		position: absolute;
		font-size: 2rem;
		text-transform: uppercase;
		font-family: monospace;
		padding: 1rem;
	}

	.prev {
		position: absolute;
		bottom: 0;
		left: 0;
		font-size: 5rem;
		font-family: monospace;
	}

	.next {
		position: absolute;
		bottom: 0;
		right: 0;
		font-size: 5rem;
		font-family: monospace;
	}
</style>