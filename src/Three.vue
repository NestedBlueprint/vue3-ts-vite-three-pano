<template>
	<div class="container" ref="container"></div>
</template>

<script setup lang="ts">
	import * as THREE from "three";
	import { TextureLoader } from "three";
	import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
	// import { RGBELoader } from 'three/examples/jsm/loaders/RGBELoader'
	import { ref, onMounted } from "vue";
	// 初始化场景
	const scene = new THREE.Scene();
	// 初始化透视相机
	const camera = new THREE.PerspectiveCamera(
		60,
		window.innerWidth / window.innerHeight,
		1,
		1000
	);
	// camera.position.z = 0.1;
	camera.position.set(0, 0, 0);
	camera.lookAt(0, 0, 0);
	//初始化渲染器
	const renderer = new THREE.WebGL1Renderer();
	renderer.setSize(window.innerWidth, window.innerHeight);
	const container = ref();

	const render = () => {
		renderer.render(scene, camera);
		// window.requestAnimationFrame() 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行
		requestAnimationFrame(render);
	};

	// 添加立方体
	const geometry = new THREE.BoxGeometry(10, 10, 10);
	// const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
	// const cube = new THREE.Mesh(geometry, material);
	// scene.add(cube);

	// 4_b,
	let arr = ["4_l", "4_r", "4_u", "4_d", "4_b", "4_f"];
	let boxMaterials: any[] = [];
	const loader = new TextureLoader();

	let leftTexture = loader.load("../public/imgs/panos/left.jpg");
	let rightTexture = loader.load("../public/imgs/panos/right.jpg");
	let upTexture = loader.load("../public/imgs/panos/up.jpg");
	// upTexture.rotation = Math.PI;
	// upTexture.center = new THREE.Vector2(0.5, 0.5);
	let downTexture = loader.load("../public/imgs/panos/bottom.jpg");
	// downTexture.rotation = Math.PI;
	// downTexture.center = new THREE.Vector2(0.5, 0.5);
	let frontTexture = loader.load("../public/imgs/panos/front.jpg");
	let backTexture = loader.load("../public/imgs/panos/back.jpg");

	let textures = [];
	textures.push(new THREE.MeshBasicMaterial({ map: rightTexture }));
	textures.push(new THREE.MeshBasicMaterial({ map: leftTexture }));
	textures.push(new THREE.MeshBasicMaterial({ map: upTexture }));
	textures.push(new THREE.MeshBasicMaterial({ map: downTexture }));
	textures.push(new THREE.MeshBasicMaterial({ map: frontTexture }));
	textures.push(new THREE.MeshBasicMaterial({ map: backTexture }));

	// arr.forEach((item) => {
	// 	// 纹理加载
	// 	let texture = new THREE.TextureLoader().load(`./imgs/living/${item}.jpg`);
	// 	// 创建材质
	// 	if (item === "4_u" || item === "4_d") {
	// 		texture.rotation = Math.PI;
	// 		texture.center = new THREE.Vector2(0.5, 0.5);
	// 		textures.push(new THREE.MeshBasicMaterial({ map: texture }));
	// 	} else {
	// 		textures.push(new THREE.MeshBasicMaterial({ map: texture }));
	// 	}
	// });
	const cube = new THREE.Mesh(geometry, textures);
	cube.geometry.scale(1, 1, -1);
	scene.add(cube);

	// 添加球
	// const geometry = new THREE.SphereGeometry(
	// 	/*半径*/ 5,
	// 	/*垂直节点数量*/ 32,
	// 	/*水平节点数量*/ 32
	// ); //节点数量越大，需要计算的三角形就越多，影响性能
	// const loader = new TextureLoader(); // 贴全景图
	// loader.load("./imgs/car/images/textures/skymap2.jpg", (texture) => {
	// 	// const loader = new RGBELoader() // 载入模型
	// 	// loader.load('./imgs/hdr/10xiangxi-1.hdr', (texture) => {
	// 	const material = new THREE.MeshBasicMaterial({ map: texture }); // 材质  map: texture 颜色贴图
	// 	const sphere = new THREE.Mesh(geometry, material); // Mesh(geometry(几何体): Geometry, material(材质): Material ) 网格、构造器
	// 	sphere.geometry.scale(1, 1, -1);
	// 	scene.add(sphere);
	// });

	onMounted(() => {
		// 添加控制器
		document.body.appendChild(renderer.domElement)
		const controls = new OrbitControls(camera, renderer.domElement);
		controls.enableDamping = true;
		// container.value.appendChild(renderer.domElement);
		controls.minDistance = 0.5;
		controls.maxDistance = 1000;
	
		render();
	});
</script>

<style>
	* {
		margin: 0;
		padding: 0;
	}
	.container {
		height: 100vh;
		width: 100vw;
		background-color: #f0f0f0;
	}
</style>
