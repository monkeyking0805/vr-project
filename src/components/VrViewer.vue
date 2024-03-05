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
        currentIndex: 0,
        intervalId: null,
      };
    },
    mounted() {
      this.initViewer();
      this.loadPanoramas();
      this.initPanorama();
      this.setTimer();
    },
    beforeUnmount(){
      if (this.intervalId) {
        clearInterval(this.intervalId);
        this.intervalId = null;
      }
    },
    methods: {
      initViewer() {
        this.viewer = new PANOLENS.Viewer({ 
          container: document.querySelector('#container') ,
          autoHideInfospot: false,
          autoRotate: true,
          autoRotateSpeed: 2, // Rotate 1 degree per second
          controlBar: false, // Optional: hide control bar if not needed
          output: 'console'
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
            this.panoramas[i].addEventListener('click', () => {
              if (this.intervalId) {
                clearInterval(this.intervalId);
                this.intervalId = null;
              }
            })
          }
        }
        this.addHotspots();
      },
      changePanorama(index) {
        // Change the current panorama to the one at the given index
        this.viewer.setPanorama(this.panoramas[index]);
        this.currentIndex = index;
        if (!this.intervalId) {
          this.setTimer();
        }
      },
      addHotspots() {
        // Define hotspot configurations
        const hotspotConfigs = [
          { position: new THREE.Vector3(4522.68, -241.92, -2101.19), targetPanoramaIndex: 1, panoramaIndex: 0 },
          { position: new THREE.Vector3(-1568.18, -895.65, -4656.43), targetPanoramaIndex: 2, panoramaIndex: 0 },
          { position: new THREE.Vector3(3047.93, -349.37, 3939.18), targetPanoramaIndex: 3, panoramaIndex: 0 },          
          { position: new THREE.Vector3(2000, 0, 0), targetPanoramaIndex: 2, panoramaIndex: 1 },
          { position: new THREE.Vector3(-2000, 0, 0), targetPanoramaIndex: 7, panoramaIndex: 1 },
          { position: new THREE.Vector3(2500, 300, 0), targetPanoramaIndex: 1, panoramaIndex: 2 },
          { position: new THREE.Vector3(2632.59, 29.04, 4242.66), targetPanoramaIndex: 3, panoramaIndex: 2 },
          { position: new THREE.Vector3(-2426.53, -881.61, 4276.40), targetPanoramaIndex: 0, panoramaIndex: 2 },
          { position: new THREE.Vector3(2500, 300, -6000), targetPanoramaIndex: 9, panoramaIndex: 2 },
          { position: new THREE.Vector3(-739.14, -457.52, -4914.93), targetPanoramaIndex: 13, panoramaIndex: 2 },
          { position: new THREE.Vector3(2000, -400, -2000), targetPanoramaIndex: 0, panoramaIndex: 3 },
          { position: new THREE.Vector3(1457.39, -375.00, 4763.55), targetPanoramaIndex: 4, panoramaIndex: 3 },
          { position: new THREE.Vector3(1908.38, -1823.79, -4242.06), targetPanoramaIndex: 3, panoramaIndex: 4 },
          { position: new THREE.Vector3(1578.54, -467.42, 4716.44), targetPanoramaIndex: 5, panoramaIndex: 4 },
          { position: new THREE.Vector3(4405.50, -613.04, -2260.71), targetPanoramaIndex: 4, panoramaIndex: 5 },
          { position: new THREE.Vector3(2081.37, -957.93, 4435.25), targetPanoramaIndex: 6, panoramaIndex: 5 },
          { position: new THREE.Vector3(2068.64, -458.86, -4523.81), targetPanoramaIndex: 5, panoramaIndex: 6 },
          { position: new THREE.Vector3(232.74, -1085.55, 4865.90), targetPanoramaIndex: 7, panoramaIndex: 6 },
          { position: new THREE.Vector3(4923.19, -565.83, -605.17), targetPanoramaIndex: 1, panoramaIndex: 7 },
          { position: new THREE.Vector3(1758.58, -355.15, -4658.40), targetPanoramaIndex: 6, panoramaIndex: 7 },
          { position: new THREE.Vector3(-944.18, -862.17, 4827.52), targetPanoramaIndex: 10, panoramaIndex: 7 },
          { position: new THREE.Vector3(-1500, -400, -1000), targetPanoramaIndex: 10, panoramaIndex: 8 },
          { position: new THREE.Vector3(4667.20, -913.13, 1530.43), targetPanoramaIndex: 9, panoramaIndex: 8 },
          { position: new THREE.Vector3(2540.78, -882.21, 4206.65), targetPanoramaIndex: 11, panoramaIndex: 8 },
          { position: new THREE.Vector3(3014.06, -607.86, -3931.65), targetPanoramaIndex: 8, panoramaIndex: 9 },
          { position: new THREE.Vector3(-1265.28, -584.08, -4790.85), targetPanoramaIndex: 11, panoramaIndex: 9 },
          { position: new THREE.Vector3(-4376.94, -847.34, 2244.21), targetPanoramaIndex: 12, panoramaIndex: 9 },
          { position: new THREE.Vector3(2296.73, -1249.09, -4252.99), targetPanoramaIndex: 7, panoramaIndex: 10 },
          { position: new THREE.Vector3(4457.99, -133.09, -2237.56), targetPanoramaIndex: 1, panoramaIndex: 10 },
          { position: new THREE.Vector3(3684.05, -744.37, 3294.18), targetPanoramaIndex: 8, panoramaIndex: 10 },
          { position: new THREE.Vector3(2871.83, -1346.71, -3856.85), targetPanoramaIndex: 8, panoramaIndex: 11 },
          { position: new THREE.Vector3(537.83, -567.21, 4933.69), targetPanoramaIndex: 12, panoramaIndex: 11 },
          { position: new THREE.Vector3(1592.24, -1219.06, 4573.77), targetPanoramaIndex: 9, panoramaIndex: 11 },
          { position: new THREE.Vector3(2814.23, -446.77, 4100.04), targetPanoramaIndex: 2, panoramaIndex: 12 },
          { position: new THREE.Vector3(2941.64, -364.92, -4024.47), targetPanoramaIndex: 11, panoramaIndex: 12 },
          { position: new THREE.Vector3(-180.89, -989.10, 4887.80), targetPanoramaIndex: 13, panoramaIndex: 12 },
          { position: new THREE.Vector3(2343.84, -933.40, 4306.99), targetPanoramaIndex: 2, panoramaIndex: 13 },
          { position: new THREE.Vector3(-775.01, -728.25, -4876.85), targetPanoramaIndex: 12, panoramaIndex: 13 },
        ];

        // Loop through the configurations and create hotspots
        hotspotConfigs.forEach(({ position, targetPanoramaIndex, panoramaIndex }) => {
          this.createAndAddHotspot(position, targetPanoramaIndex, panoramaIndex);
        });
      },
      createAndAddHotspot(position, targetPanoramaIndex, panoramaIndex) {
        const hotspot = new PANOLENS.Infospot(300, PANOLENS.DataImage.Arrow);
        hotspot.position.copy(position);
        hotspot.addEventListener('click', () => {
          this.changePanorama(targetPanoramaIndex);
          console.log(targetPanoramaIndex);
        });
        this.panoramas[panoramaIndex].add(hotspot);
      },
      setTimer() {
        this.intervalId = setInterval(() => {
          console.log("timer called");
          const nextStep = (this.currentIndex + 1) % 14;
          this.changePanorama(nextStep);
        },15000);
      }
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
  