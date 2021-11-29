<template>
  <v-container>
    <v-col>
<div class="max-width">
  <vgl-renderer name="rederer" :antialias=antialias camera="camera" scene="scene">
    <vgl-sphere-geometry name="sphere" width-segments=32 height-segments=16></vgl-sphere-geometry>
    <vgl-box-geometry name="box"></vgl-box-geometry>
    <vgl-scene name="scene">
      <!-- Re-rendering will be triggered when zCoordinate changes. -->
      <vgl-mesh :position="`0 0 ${zCoordinate}`" geometry="sphere"></vgl-mesh>
      <vgl-mesh :position="`5 5 ${zCoordinate}`" geometry="box"></vgl-mesh>
    </vgl-scene>
    <vgl-orthographic-camera name="camera" orbit-position="10 1 0" :zoom=zoom></vgl-orthographic-camera>
  </vgl-renderer>
  <!-- User can handles zCoordinate via this range input. -->
  <input type="range" v-model="zCoordinate" min="-5" max="5">
  <input type="range" v-model="zoom" min="1" max="50">
</div>
    </v-col>
  </v-container>
</template>

<script>
import {VglRenderer, VglScene, VglBoxGeometry, VglSphereGeometry, VglMesh, VglOrthographicCamera} from "vue-gl";
//import { OrbitControls } from "https://threejs.org/examples/jsm/controls/OrbitControls.js";

export default {
  name: "three-example",
  display: "Three",
  order: 8,
  components: {
    VglRenderer,
    VglScene,
    VglBoxGeometry,
    VglSphereGeometry,
    VglMesh,
    VglOrthographicCamera,
  },
  data() {
    return {
      zCoordinate: 0,
      antialias: true,
      zoom: 10,
    };
  },
  methods: {
  },
};
</script>
<style scoped>
.max-width {
  max-width: 400px;
}
</style>