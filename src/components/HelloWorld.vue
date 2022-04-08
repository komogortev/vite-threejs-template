<script setup>
import { ref } from 'vue'
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

// 1. Init three tools
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

// 2. Declare the scene setup
// 2.1. System (dev, navigation) elements
/* define lines *material, for lines we have to use LineBasicMaterial or LineDashedMaterial. */
const axisXMaterial = new THREE.LineBasicMaterial({ color: 0xff0000 }); // create a red LineBasicMaterial
const axisYMaterial = new THREE.LineBasicMaterial({ color: 0x0000ff }); // create a blue LineBasicMaterial
const axisZMaterial = new THREE.LineBasicMaterial({ color: 0x00ff00 }); // create a green LineBasicMaterial
/* define lines *geometry with some "vertices" */
const pointsX = [];
pointsX.push(new THREE.Vector3( - 100, 0, 0 ));
pointsX.push(new THREE.Vector3( 100, 0, 0 ));
const pointsY = [];
pointsY.push(new THREE.Vector3( 0, - 100, 0 ));
pointsY.push(new THREE.Vector3( 0, 100, 0 ));
const pointsZ = [];
pointsZ.push(new THREE.Vector3( 0, 0, - 100 ));
pointsZ.push(new THREE.Vector3( 0, 0, 100 ));
/* The lines are drawn between each consecutive pair of vertices,
  but not between the first and last (the line is not closed.) */
const axisXGeometry = new THREE.BufferGeometry().setFromPoints( pointsX );
const axisYGeometry = new THREE.BufferGeometry().setFromPoints( pointsY );
const axisZGeometry = new THREE.BufferGeometry().setFromPoints( pointsZ );
/* Having points for two lines and a material, we can put them together to form a line. */
const lineX = new THREE.Line( axisXGeometry, axisXMaterial );
const lineY = new THREE.Line( axisYGeometry, axisYMaterial );
const lineZ = new THREE.Line( axisZGeometry, axisZMaterial );
scene.add( lineX, lineY, lineZ );

// 2.2. Context objects
const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );

camera.position.z = 5;

// 3. Render the scene
/* Create a loop that causes the renderer to draw the scene
  every time the screen is refreshed (on a typical screen
  this means 60 times per second). */
function animate() {
	requestAnimationFrame( animate );
	renderer.render( scene, camera );

  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
}

animate();
</script>

<template>

</template>

<style scoped>
</style>
