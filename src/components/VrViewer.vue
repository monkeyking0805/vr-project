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
            autoRotate: true,
            autoRotateSpeed: 1, // Rotate 1 degree per second
            controlBar: false // Optional: hide control bar if not needed
          });
      },
      loadPanoramas() {
        // Load your panoramas here
        this.panoramas = [
          new PANOLENS.ImagePanorama(require('@/assets/1.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/2.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/3.jpg')),
          new PANOLENS.ImagePanorama(require('@/assets/4.jpg')),
          // Add as many panoramas as you like
        ];
      },
      initPanorama() {
        // Add the first panorama as the initial one
        if (this.panoramas.length > 0) {
          this.viewer.add(this.panoramas[0]);
          this.viewer.add(this.panoramas[1]);
          this.viewer.add(this.panoramas[2]);
          this.viewer.add(this.panoramas[3]);
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
          { position: new THREE.Vector3(2500, 400, -4000), targetPanoramaIndex: 3, panoramaIndex: 0 },
          { position: new THREE.Vector3(4000, 600, -1000), targetPanoramaIndex: 2, panoramaIndex: 0 },
          { position: new THREE.Vector3(7000, 2500, 9000), targetPanoramaIndex: 1, panoramaIndex: 0 },
          { position: new THREE.Vector3(4000, 400, -4000), targetPanoramaIndex: 0, panoramaIndex: 3 },
          { position: new THREE.Vector3(6000, 800, 2000), targetPanoramaIndex: 2, panoramaIndex: 3 },
          { position: new THREE.Vector3(5000, 1000, -500), targetPanoramaIndex: 0, panoramaIndex: 2 },
          { position: new THREE.Vector3(3000, 400, -6000), targetPanoramaIndex: 3, panoramaIndex: 2 },
          { position: new THREE.Vector3(6000, 400, 6000), targetPanoramaIndex: 1, panoramaIndex: 2 },
          { position: new THREE.Vector3(2000, 0, 1500), targetPanoramaIndex: 0, panoramaIndex: 1 },
          { position: new THREE.Vector3(3500, 200, -2000), targetPanoramaIndex: 2, panoramaIndex: 1 },
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
  