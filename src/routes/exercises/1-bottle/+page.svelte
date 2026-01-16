<script lang="ts">
  import * as THREE from "three";
  import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";

  import { OrbitControls } from "three/examples/jsm/Addons.js";

  let container: HTMLDivElement; // bound to elem

  // Scene
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0xf0f0f0); // light gray

  // Camera
  const camera = new THREE.PerspectiveCamera(
    75, // F?OV
    window.innerWidth / window.innerHeight, // aspect ratio
    0.1, // near/far clip
    1000,
  );

  camera.position.z = 3;
  camera.position.y = 1;
  camera.position.x = 0.2;

  const renderer = new THREE.WebGLRenderer();
  renderer.setSize(window.innerWidth, window.innerHeight);

  $: if (container) {
    container.appendChild(renderer.domElement);

    // Controls
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.update();

    // Add bottle
    const gltfLoader = new GLTFLoader();
    let bottle: THREE.Group<THREE.Object3DEventMap>;

    gltfLoader.load(
      "/assets/bottle.glb",
      function (gltf) {
        bottle = gltf.scene;

        gltf.scene.traverse((obj) => {
          if (obj instanceof THREE.Light) {
            obj.intensity *= 0.005;
            scene.add(obj);
          }
        });
        scene.add(bottle);
      },
      undefined,
      function (error) {
        console.error(error);
      },
    );

    renderer.setAnimationLoop(() => {
      bottle?.rotateY(0.004);
      controls.update();

      renderer.render(scene, camera);
    });
  }
</script>

<div bind:this={container} id="bottle-render"></div>

<style>
  #bottle-render {
    height: 100%;
  }
</style>
