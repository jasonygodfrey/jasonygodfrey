### Jason ⊂(◉‿◉)つ 
<strong>Software Developer Engineer</strong>

I'm a full-stack software engineer with a passion for game development. With my expertise in both front-end and back-end development, I bring a unique perspective to projects and strive to make an impact. I am driven by creating memorable experiences. I have experience in producing websites, apps, games, vr, and ai.

<h1 style="font-size:48px;"><a href="https://jasongodfrey.dev">jasongodfrey.dev</a></h1>

![Snake animation](https://github.com/jasonygodfrey/jasonygodfrey/blob/output/github-contribution-grid-snake.svg)

<p>
  <img src="https://img.icons8.com/color/48/000000/python.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/javascript.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/c-plus-plus-logo.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/c-sharp-logo.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/java-coffee-cup-logo.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/django.png" width="48" height="48">
  <img src="https://user-images.githubusercontent.com/83245646/217352002-5fda2c7e-8cd7-4192-8076-20f09c9a4502.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/bootstrap.png" width="48" height="48">
</p>
<p>
  <img src="https://img.icons8.com/color/48/000000/sql.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/mongodb.png" width="48" height="48">
  <img src="https://user-images.githubusercontent.com/83245646/217352408-21aa6d39-10f8-4e54-ba4a-4252e4090952.png" width="48" height="48">
  <img src="https://user-images.githubusercontent.com/83245646/217356559-4ed8bb7f-d71a-4f9d-a500-decfbade3290.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/google-cloud.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/android-studio.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/docker.png" width="48" height="48">
  <img src="https://img.icons8.com/color/48/000000/kubernetes.png" width="48" height="48">
</p>
<p>
  <img
<html lang="en">
	<head>
		<title>three.js webgl - animation - keyframes</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
		<link type="text/css" rel="stylesheet" href="main.css">
		<style>
			body {
				background-color: #bfe3dd;
				color: #000;
			}

			a {
				color: #2983ff;
			}
		</style>
	</head>

	<body>

		<div id="container"></div>

		<div id="info">
			<a href="https://threejs.org" target="_blank" rel="noopener">three.js</a> webgl - animation - keyframes<br/>
			Model: <a href="https://artstation.com/artwork/1AGwX" target="_blank" rel="noopener">Littlest Tokyo</a> by
			<a href="https://artstation.com/glenatron" target="_blank" rel="noopener">Glen Fox</a>, CC Attribution.
		</div>

		<!-- Import maps polyfill -->
		<!-- Remove this when import maps will be widely supported -->
		<script async src="https://unpkg.com/es-module-shims@1.3.6/dist/es-module-shims.js"></script>

		<script type="importmap">
			{
				"imports": {
					"three": "../build/three.module.js",
					"three/addons/": "./jsm/"
				}
			}
		</script>

		<script type="module">

			import * as THREE from 'three';

			import Stats from 'three/addons/libs/stats.module.js';

			import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
			import { RoomEnvironment } from 'three/addons/environments/RoomEnvironment.js';

			import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
			import { DRACOLoader } from 'three/addons/loaders/DRACOLoader.js';

			let mixer;

			const clock = new THREE.Clock();
			const container = document.getElementById( 'container' );

			const stats = new Stats();
			container.appendChild( stats.dom );

			const renderer = new THREE.WebGLRenderer( { antialias: true } );
			renderer.setPixelRatio( window.devicePixelRatio );
			renderer.setSize( window.innerWidth, window.innerHeight );
			renderer.outputEncoding = THREE.sRGBEncoding;
			container.appendChild( renderer.domElement );

			const pmremGenerator = new THREE.PMREMGenerator( renderer );

			const scene = new THREE.Scene();
			scene.background = new THREE.Color( 0xbfe3dd );
			scene.environment = pmremGenerator.fromScene( new RoomEnvironment(), 0.04 ).texture;

			const camera = new THREE.PerspectiveCamera( 40, window.innerWidth / window.innerHeight, 1, 100 );
			camera.position.set( 5, 2, 8 );

			const controls = new OrbitControls( camera, renderer.domElement );
			controls.target.set( 0, 0.5, 0 );
			controls.update();
			controls.enablePan = false;
			controls.enableDamping = true;

			const dracoLoader = new DRACOLoader();
			dracoLoader.setDecoderPath( 'jsm/libs/draco/gltf/' );

			const loader = new GLTFLoader();
			loader.setDRACOLoader( dracoLoader );
			loader.load( 'models/gltf/LittlestTokyo.glb', function ( gltf ) {

				const model = gltf.scene;
				model.position.set( 1, 1, 0 );
				model.scale.set( 0.01, 0.01, 0.01 );
				scene.add( model );

				mixer = new THREE.AnimationMixer( model );
				mixer.clipAction( gltf.animations[ 0 ] ).play();

				animate();

			}, undefined, function ( e ) {

				console.error( e );

			} );


			window.onresize = function () {

				camera.aspect = window.innerWidth / window.innerHeight;
				camera.updateProjectionMatrix();

				renderer.setSize( window.innerWidth, window.innerHeight );

			};


			function animate() {

				requestAnimationFrame( animate );

				const delta = clock.getDelta();

				mixer.update( delta );

				controls.update();

				stats.update();

				renderer.render( scene, camera );

			}


		</script>

	</body>

</html>
