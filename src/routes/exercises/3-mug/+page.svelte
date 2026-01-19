<script lang="ts">
  import * as THREE from "three";
  import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";

  import { OrbitControls } from "three/examples/jsm/Addons.js";

  let container: HTMLDivElement; // bound to elem

  // Scene
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0xf0f0f0); // light gray

  // Renderer
  const renderer = new THREE.WebGLRenderer();
  renderer.setSize(window.innerWidth, window.innerHeight);

  renderer.outputColorSpace = THREE.SRGBColorSpace;
  renderer.toneMapping = THREE.ACESFilmicToneMapping;
  renderer.toneMappingExposure = 1.0;

  // Camera
  const camera = new THREE.PerspectiveCamera(
    75, // F?OV
    window.innerWidth / window.innerHeight, // aspect ratio
    0.1, // near/far clip
    1000,
  );
  camera.position.set(3, 2, 4);

  // Lighting
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.2);
  scene.add(ambientLight);
  const directionalLight = new THREE.DirectionalLight(0xffffff, 0.1);
  directionalLight.position.set(3, 5, 2);
  scene.add(directionalLight);

  $: if (container) {
    container.appendChild(renderer.domElement);

    // Controls
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.update();

    // Add mug
    const gltfLoader = new GLTFLoader();
    let mug: THREE.Group<THREE.Object3DEventMap>;

    gltfLoader.load(
      "/assets/mug.glb",
      function (gltf) {
        mug = gltf.scene;

        scene.add(mug);

        mug.traverse((obj) => {
          if (obj instanceof THREE.Light) {
            obj.intensity *= 0.004;
            scene.add(obj);
          }
        });
      },
      undefined,
      function (error) {
        console.error(error);
      },
    );

    renderer.setAnimationLoop(() => {
      mug?.rotateY(0.004);
      controls.update();

      renderer.render(scene, camera);
    });
  }
</script>

<div bind:this={container} id="mug-render"></div>

<style>
  #mug-render {
    height: 100%;
  }
</style>
