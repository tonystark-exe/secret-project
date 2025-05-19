<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>GitHub Theme - Real Car Lover</title>
<style>
  /* Basic reset */
  * {
    box-sizing: border-box;
  }
  body {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji";
    background-color: #0d1117;
    color: #c9d1d9;
    line-height: 1.6;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  /* Header styled like GitHub's dark header */
  header {
    background-color: #161b22;
    padding: 1rem 2rem;
    display: flex;
    align-items: center;
    border-bottom: 1px solid #30363d;
    user-select: none;
  }
  header h1 {
    color: #58a6ff;
    font-weight: 600;
    font-size: 1.5rem;
    margin: 0;
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  }
  /* Main container */
  main {
    max-width: 960px;
    margin: 2rem auto;
    padding: 0 1rem;
  }
  h2 {
    color: #58a6ff;
    font-weight: 600;
    margin-bottom: 0.5rem;
  }
  p {
    margin-bottom: 1.2rem;
    font-size: 1.1rem;
  }
  /* 3D canvas container styling */
  #car-3d-container {
    width: 100%;
    max-width: 960px;
    height: 480px;
    background: #010409;
    border-radius: 12px;
    box-shadow: 0 8px 24px rgb(21 28 42 / 0.6);
    margin: 2rem 0 4rem;
    user-select: none;
  }
  /* Footer styled like GitHub dark footer */
  footer {
    text-align: center;
    padding: 1rem 2rem;
    font-size: 0.9rem;
    color: #8b949e;
    border-top: 1px solid #30363d;
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  }
  /* Link styling */
  a {
    color: #58a6ff;
    text-decoration: none;
  }
  a:hover {
    text-decoration: underline;
  }
</style>
</head>
<body>

<header>
  <h1>Real Car Lover 🚗</h1>
</header>

<main>
  <h2>Welcome to Your Garage</h2>
  <p>
    Celebrate your passion for cars with this immersive GitHub-themed webpage. 
    Explore sleek and modern car models up close with interactive 3D visualizations.
  </p>
  <p>
    Rotate the car models by clicking and dragging the mouse or using touch gestures on mobile.
  </p>

  <div id="car-3d-container"></div>

  <h2>About Real Cars</h2>
  <p>
    Real cars are a blend of art, engineering, and technology. From the roar of an engine to the smooth curves of the chassis, 
    car enthusiasts appreciate every detail. This site showcases simple 3D car models in a minimalistic style in tune with GitHub's cool dark theme.
  </p>
</main>

<footer>
  Made with ❤️ by a real car lover &middot; Inspired by GitHub Dark Theme
</footer>

<!-- Three.js and supporting scripts -->
<script src="https://cdn.jsdelivr.net/npm/three@0.153.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.153.0/examples/js/controls/OrbitControls.min.js"></script>
<script>
  // Setup basic scene
  const container = document.getElementById('car-3d-container');
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0x010409);

  const camera = new THREE.PerspectiveCamera(45, container.clientWidth / container.clientHeight, 0.1, 1000);
  camera.position.set(5, 3, 7);

  const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false });
  renderer.setSize(container.clientWidth, container.clientHeight);
  renderer.setClearColor(0x010409, 1);
  container.appendChild(renderer.domElement);

  // Controls to rotate model (orbit style)
  const controls = new THREE.OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.dampingFactor = 0.1;
  controls.minDistance = 4;
  controls.maxDistance = 15;
  controls.maxPolarAngle = Math.PI / 2; // limit vertical rotation

  // Lighting
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
  scene.add(ambientLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
  directionalLight.position.set(5, 10, 7);
  scene.add(directionalLight);

  // Simple stylized car model as a composition of boxes and cylinders
  function createCarModel() {
    const car = new THREE.Group();

    // Main body (box)
    const bodyGeometry = new THREE.BoxGeometry(3.5, 1, 1.2);
    const bodyMaterial = new THREE.MeshStandardMaterial({ color: 0x58a6ff, metalness: 0.5, roughness: 0.4 });
    const body = new THREE.Mesh(bodyGeometry, bodyMaterial);
    body.position.y = 0.5;
    car.add(body);

    // Cabin (smaller box)
    const cabinGeometry = new THREE.BoxGeometry(1.8, 0.7, 1);
    const cabinMaterial = new THREE.MeshStandardMaterial({ color: 0x1f6feb, metalness: 0.6, roughness: 0.3 });
    const cabin = new THREE.Mesh(cabinGeometry, cabinMaterial);
    cabin.position.set(0.2, 1.05, 0);
    car.add(cabin);

    // Wheels (cylinders)
    const wheelGeometry = new THREE.CylinderGeometry(0.4, 0.4, 0.5, 24);
    const wheelMaterial = new THREE.MeshStandardMaterial({ color: 0x222222, metalness: 0.7, roughness: 0.4 });

    const wheelPositions = [
      [-1.3, 0.2, 0.7],
      [1.3, 0.2, 0.7],
      [-1.3, 0.2, -0.7],
      [1.3, 0.2, -0.7]
    ];
    wheelPositions.forEach(pos => {
      const wheel = new THREE.Mesh(wheelGeometry, wheelMaterial);
      wheel.rotation.z = Math.PI / 2;
      wheel.position.set(pos[0], pos[1], pos[2]);
      car.add(wheel);
    });

    return car;
  }

  const carModel = createCarModel();
  scene.add(carModel);

  // Animation loop
  function animate() {
    requestAnimationFrame(animate);
    controls.update();
    renderer.render(scene, camera);
  }
  animate();

  // Handle resizing
  window.addEventListener('resize', () => {
    const width = container.clientWidth;
    const height = container.clientHeight;

    camera.aspect = width / height;
    camera.updateProjectionMatrix();

    renderer.setSize(width, height);
  });
</script>
</body>
</html>

