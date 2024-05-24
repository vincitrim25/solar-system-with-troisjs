<template>
    <Group>
      <AmbientLight></AmbientLight>
      <GltfModel 
        ref="model" 
        :src="src" 
        :scale='{ "x": scale, "y": scale, "z": scale }' 
        :rotation="rotation"
        :position="new Vector3(position.x, position.y, position.z)"
      />
    </Group>
  </template>
  
  <script setup lang="ts">
  import { Vector3 } from 'three';
  import { GltfModel, AmbientLight, Group } from 'troisjs'
  
  const props = defineProps({
    speed: { type: Number, default: 1 },
    scale: { type: Number, default: 1 },
    rotation: {type: Object, default: {x: 0, y: 0, z: 0}},
    position: {type: Object, default: {x: 0, y: 0, z: 0}},
    src: {type: String, default : ''}
  });
  
  </script>
  
  <style></style>