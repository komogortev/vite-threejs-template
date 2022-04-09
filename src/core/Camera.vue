<script setup>
import { ref } from 'vue'
import { PerspectiveCamera } from 'three';

const camera = new PerspectiveCamera(
  75, // FOV
  window.innerWidth / window.innerHeight, // aspect ratio
  0.1, // near clipping plane
  1000 // far clipping plane
);
camera.position.set(0, 0, 100);
camera.lookAt(0, 0, 0);
</script>

<template>

</template>

<style scoped>
</style>
