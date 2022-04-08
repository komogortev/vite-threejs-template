<script setup>
import { ref } from 'vue'
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

// 1. Init three tools
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(
  75, // FOV
  window.innerWidth / window.innerHeight, // aspect ratio
  0.1, // near clipping plane
  1000 // far clipping plane
);
const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );

const controls = new OrbitControls( camera, renderer.domElement );

// 2. Declare the scene setup
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
}
animate();
</script>

<template>

</template>

<style scoped>
</style>
