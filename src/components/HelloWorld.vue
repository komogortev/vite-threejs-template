<script setup>
import { ref } from 'vue'
import useWorldStore from "../store/world";
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
const { solarSystem } = useWorldStore();
// = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =
// 1. Init three tools
// 1.1. Fundamental scene setup
const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );

const camera = new THREE.PerspectiveCamera(
  75, // FOV
  window.innerWidth / window.innerHeight, // aspect ratio
  0.1, // near clipping plane
  1000 // far clipping plane
);
camera.position.set(0, 0, 100);
camera.lookAt(0, 0, 0);

const scene = new THREE.Scene();
const controls = new OrbitControls( camera, renderer.domElement );
const loader = new GLTFLoader();

const ambientLight = new THREE.AmbientLight(0xffffff, .25)
scene.add(ambientLight);

const color = 0xFFFFFF;
const intensity = 3;
const light = new THREE.PointLight(color, intensity);
scene.add(light);

// 1.1. System (dev, navigation) elements


// = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =
// 2. Declare the scene content
// 2.2 An array of objects whose rotation to update
const objects = [];
// 2.2 Use just one sphere for everything
const radius = 1;
const widthSegments = 6;
const heightSegments = 6;
const sphereGeometry = new THREE.SphereGeometry(
    radius, widthSegments, heightSegments);

const parsedSolar = JSON.parse(JSON.stringify(solarSystem.value))
Object.keys(parsedSolar).forEach(objectName => {
  createPlanetoid(
    null, {[objectName]: parsedSolar[objectName]}
  );
})

function createPlanetoid (parent = null, planetoidInfo = {}) {
  const planetoidName = Object.keys(planetoidInfo)[0]
  const _sphere = new THREE.Object3D();
  const _material = new THREE.MeshPhongMaterial({
    emissive: planetoidInfo[planetoidName].emissive,
    color: planetoidInfo[planetoidName].color
  });
  const _mesh = new THREE.Mesh(sphereGeometry, _material);
  scene.add(_sphere);
  objects.push(_sphere);

  Object.keys(planetoidInfo[planetoidName]).forEach(prop => {
    switch (prop) {
      case 'scale':
        _mesh.scale.set(
          planetoidInfo[planetoidName].scale,
          planetoidInfo[planetoidName].scale,
          planetoidInfo[planetoidName].scale,
        );
        break;
      case 'distance':
        _sphere.position.x = planetoidInfo[planetoidName].distance * 100
        break;
    }

    if (planetoidInfo[planetoidName].children) {
      Object.keys(planetoidInfo[planetoidName].children).forEach(childName => {
        createPlanetoid(
          planetoidName,
          { [childName]: planetoidInfo[planetoidName].children[childName] }
        )
      })
    }
  })

  _sphere.add(_mesh);
  objects.push(_mesh);
}

// add an AxesHelper to each node
objects.forEach((node) => {
  const axes = new THREE.AxesHelper();
  axes.material.depthTest = false; // will not check to see if they are drawing behind something else
  axes.renderOrder = 1; // (the default is 0) so that axes get drawn after all the spheres
  node.add(axes);
});

// * Load 3D model
loader.load( '/public/models/toon-cat/toon-cat.gltf', ( gltf ) => {
  gltf.animations; // Array<THREE.AnimationClip>
  gltf.scene; // THREE.Group
  gltf.scenes; // Array<THREE.Group>
  gltf.cameras; // Array<THREE.Camera>
  gltf.asset; // Object
  // *1. detailed option: downsize the model scale
  // const box = new THREE.Box3().setFromObject(gltf.scene);
  // const size = new THREE.Vector3();
  // var center = new THREE.Vector3();
  // box.getCenter(center);
  // var scale = .05;
  // gltf.scene.scale.setScalar(scale);
  // gltf.scene.position.sub(center.multiplyScalar(scale));
  // *2. general scale: alter model scale (less recommended)
  gltf.scene.scale.setScalar(.025);
  scene.add( gltf.scene );
}, undefined, ( error ) => {
  console.error( error );
} );

// = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =
// 3. Render the scene
/* Create a loop that causes the renderer to draw the scene
  every time the screen is refreshed (on a typical screen
  this means 60 times per second). */
function animate() {
	requestAnimationFrame( animate );
	renderer.render( scene, camera );

  objects.forEach((obj) => {
    obj.rotation.y += 0.01;
  });
}

animate();
</script>

<template>
  <div id="info">Threejs basic template</div>
  <pre>
    clue1
  </pre>
  <Camera></Camera>
</template>

<style scoped>
#info {
	position: absolute;
	top: 10px;
	width: 100%;
	text-align: center;
	z-index: 100;
	display:block;
}
</style>
