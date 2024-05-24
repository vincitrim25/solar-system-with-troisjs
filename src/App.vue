<template>
  <Renderer ref="rendererRef" antialias :orbit-ctrl="{ enableDamping: true }" resize="window">
    <Camera :position="{ z: 300 }" />
    <Scene ref="scene">
      <Sphere :radius="1000">
        <PhongMaterial :props="{side: 1}">
          <Texture 
            src="../src/assets/galaxy_starfield.png"
            :props="{ side: 2, shininess: 0}"
          />
        </PhongMaterial>
      </Sphere>
      <CelestialBody 
        ref="earth"
        :scale=".5" 
        :rotation="earthRotation" 
        :position="earthPosition"
        :src="'../src/assets/earth/scene.gltf'"
      />
      <CelestialBody 
        ref="moon"
        :scale=".125" 
        :rotation="moonRotation" 
        :position="moonPosition"
        :src="'../src/assets/moon/scene.gltf'"
      />
    </Scene>
  </Renderer>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { RendererPublicInterface, Renderer, Camera, Scene, Group, Sphere, SphereGeometry, StandardMaterial, Texture, PhongMaterial } from 'troisjs'
import CelestialBody from './CelestialBody.vue';
import { CircleGeometry, MeshBasicMaterial, Mesh, EllipseCurve, Line, LineBasicMaterial, BufferGeometry } from 'three'

const rendererRef = ref();
const scene = ref();
const earthRotation = ref({x: 0, y: 0, z: 0});
const earthPosition = ref({x: 0, y: 0, z: 0});
const moonRotation = ref({x: 0, y: 0, z: 0});
const moonPosition = ref({x: 100, y: 0, z: 0});

var orbitRadius = 100
var theta = 0;
var dTheta = 2 * Math.PI / 1000;


onMounted(() => {
  // var circle = new CircleGeometry(100, 32);
  // var circleMat = new MeshBasicMaterial({color: 0xffff00});
  // var circleMesh = new Mesh(circle, circleMat);
  // scene.value.add(circleMesh);
  var orbit = new EllipseCurve(
    0, 0,
    100, 100,
    0, 360 * (Math.PI/180),
    false,
    0
  )
  var points = orbit.getPoints(50);
  var circle = new Line( 
    new BufferGeometry().setFromPoints(points), 
    new LineBasicMaterial({color: '#FFFFFF', opacity: 0.2, transparent: true})
  )
  circle.geometry.rotateX(Math.PI * 0.5);
  scene.value.add( 
    circle
  )
  
  const renderer = rendererRef.value as RendererPublicInterface
  renderer.onBeforeRender(() => {
    earthRotation.value.y -= .0005
    moonRotation.value.y -= .005

    theta += dTheta;
    moonPosition.value.x = orbitRadius * Math.cos(theta);
    moonPosition.value.z = orbitRadius * Math.sin(theta);
  })
})


</script>

<style>
body,
html {
  margin: 0;
}

canvas {
  display: block;
}
</style>
