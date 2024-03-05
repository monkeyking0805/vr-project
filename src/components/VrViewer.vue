<template>
    <div id="container"></div>
  </template>
  
  <script>
  import * as PANOLENS from 'panolens';
  import * as THREE from 'three';
  
  export default {
    name: 'VrViewer',
    data() {
      return {
        viewer: null,
        panoramas: [],
      };
    },
    mounted() {
      this.initViewer();
      this.loadPanoramas();
      this.initPanorama();
    },
    methods: {
      initViewer() {
        this.viewer = new PANOLENS.Viewer({ 
            container: document.querySelector('#container') ,
            autoHideInfospot: false,
            autoRotate: true,
            autoRotateSpeed: 1, // Rotate 1 degree per second
            controlBar: false // Optional: hide control bar if not needed
          });
          this.viewer.addEventListener('click', (event) => {
            console.log('Viewer clicked', event);
            // You can implement any logic here that should run when the viewer is clicked
          });
      },
      loadPanoramas() {
        // Load your panoramas here
        this.panoramas = [
          new PANOLENS.ImagePanorama(require('@/assets/1.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/2.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/3.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/4.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/5.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/6.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/7.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/8.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/9.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/10.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/11.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/12.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/13.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/14.jpg')),
          // Add as many panoramas as you like
        ];
      },
      initPanorama() {
        // Add the first panorama as the initial one
        if (this.panoramas.length > 0) {
          for (let i = 0; i < this.panoramas.length; i++) {
            this.viewer.add(this.panoramas[i]);
          }
        }
        this.addHotspots();
      },
      changePanorama(index) {
        // Change the current panorama to the one at the given index
        this.viewer.setPanorama(this.panoramas[index]);
      },
      addHotspots() {
        // Define hotspot configurations
        const hotspotConfigs = [
          { position: new THREE.Vector3(6000, 400, -3000), targetPanoramaIndex: 1, panoramaIndex: 0 },
          { position: new THREE.Vector3(-300, 0, -4000), targetPanoramaIndex: 2, panoramaIndex: 0 },
          { position: new THREE.Vector3(3000, -400, 3000), targetPanoramaIndex: 3, panoramaIndex: 0 },          
          { position: new THREE.Vector3(2000, 0, 0), targetPanoramaIndex: 2, panoramaIndex: 1 },
          { position: new THREE.Vector3(-2000, 0, 0), targetPanoramaIndex: 7, panoramaIndex: 1 },
          { position: new THREE.Vector3(2500, 300, 0), targetPanoramaIndex: 1, panoramaIndex: 2 },
          { position: new THREE.Vector3(2500, 700, 4000), targetPanoramaIndex: 3, panoramaIndex: 2 },
          { position: new THREE.Vector3(2500, 300, -6000), targetPanoramaIndex: 9, panoramaIndex: 2 },
          { position: new THREE.Vector3(2000, -400, -2000), targetPanoramaIndex: 0, panoramaIndex: 3 },
          { position: new THREE.Vector3(3000, 400, 4000), targetPanoramaIndex: 4, panoramaIndex: 3 },
          { position: new THREE.Vector3(2000, -400, -2000), targetPanoramaIndex: 3, panoramaIndex: 4 },
          { position: new THREE.Vector3(3000, 400, 4000), targetPanoramaIndex: 5, panoramaIndex: 4 },
          { position: new THREE.Vector3(1000, -400, -2000), targetPanoramaIndex: 4, panoramaIndex: 5 },
          { position: new THREE.Vector3(3000, 400, 4000), targetPanoramaIndex: 6, panoramaIndex: 5 },
          { position: new THREE.Vector3(1000, -400, -2000), targetPanoramaIndex: 5, panoramaIndex: 6 },
          { position: new THREE.Vector3(3000, 400, 4000), targetPanoramaIndex: 7, panoramaIndex: 6 },
          { position: new THREE.Vector3(2000, 0, 0), targetPanoramaIndex: 1, panoramaIndex: 7 },
          { position: new THREE.Vector3(1000, -400, -2000), targetPanoramaIndex: 6, panoramaIndex: 7 },
          { position: new THREE.Vector3(-500, 0, 8000), targetPanoramaIndex: 10, panoramaIndex: 7 },
          { position: new THREE.Vector3(-1500, -400, -1000), targetPanoramaIndex: 10, panoramaIndex: 8 },
          { position: new THREE.Vector3(8000, -500, 2500), targetPanoramaIndex: 9, panoramaIndex: 8 },
          { position: new THREE.Vector3(1500, -400, 1000), targetPanoramaIndex: 11, panoramaIndex: 8 },
          { position: new THREE.Vector3(2000, 300, 1000), targetPanoramaIndex: 2, panoramaIndex: 9 },
          { position: new THREE.Vector3(2500, 300, -3000), targetPanoramaIndex: 8, panoramaIndex: 9 },
          { position: new THREE.Vector3(-8000, 300, -3000), targetPanoramaIndex: 12, panoramaIndex: 9 },
          { position: new THREE.Vector3(5000, 0, 4000), targetPanoramaIndex: 8, panoramaIndex: 10 },
          { position: new THREE.Vector3(0, 0, -3000), targetPanoramaIndex: 7, panoramaIndex: 10 },
          { position: new THREE.Vector3(2500, 0, -1000), targetPanoramaIndex: 1, panoramaIndex: 10 },
          { position: new THREE.Vector3(5000, 0, 4000), targetPanoramaIndex: 8, panoramaIndex: 10 },
          { position: new THREE.Vector3(0, 0, -3000), targetPanoramaIndex: 8, panoramaIndex: 11 },
          { position: new THREE.Vector3(0, 0, 3000), targetPanoramaIndex: 12, panoramaIndex: 11 },
          { position: new THREE.Vector3(1000, -500, 2000), targetPanoramaIndex: 9, panoramaIndex: 11 },
          { position: new THREE.Vector3(2000, 0, 3000), targetPanoramaIndex: 2, panoramaIndex: 12 },
          { position: new THREE.Vector3(1000, 0, -2000), targetPanoramaIndex: 11, panoramaIndex: 12 },
        ];

        // Loop through the configurations and create hotspots
        hotspotConfigs.forEach(({ position, targetPanoramaIndex, panoramaIndex }) => {
          this.createAndAddHotspot(position, targetPanoramaIndex, panoramaIndex);
        });
      },
      createAndAddHotspot(position, targetPanoramaIndex, panoramaIndex) {
        const hotspot = new PANOLENS.Infospot(500, PANOLENS.DataImage.Arrow);
        hotspot.position.copy(position);
        hotspot.addEventListener('click', () => {
          this.changePanorama(targetPanoramaIndex);
          console.log(targetPanoramaIndex);
        });

        // Adjust size for specific hotspots if needed
        if (position.equals(new THREE.Vector3(7000, 2500, 9000))) {
          hotspot.scale.set(1000);
        } else if (position.equals(new THREE.Vector3(2000, 0, 1500))) {
          hotspot.scale.set(250);
        }

        this.panoramas[panoramaIndex].add(hotspot);
      },
    }
  };

  </script>
  
  <style scoped>
  #container{
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
  }
  </style>
  