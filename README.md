<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive 3D Model</title>
    
    <!-- Load Three.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/GLTFLoader.js"></script>

    <style>
        body { margin: 0; overflow: hidden; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script>
        let scene, camera, renderer;

        function init() {
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(50, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 1, 5);

            renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            document.body.appendChild(renderer.domElement);

            // Lighting
            const ambientLight = new THREE.AmbientLight(0xffffff, 1.2);
            scene.add(ambientLight);

            const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
            directionalLight.position.set(5, 5, 5).normalize();
            scene.add(directionalLight);

            // Load 3D Model
            const loader = new THREE.GLTFLoader();
            loader.load('https://cdn.glitch.global/0383431f-93bc-43e7-85f3-3239bd533ff3/volvo_240.glb?v=1741513418170', function (gltf) {
                scene.add(gltf.scene);
                animate();
            }, undefined, function (error) {
                console.error('Error loading 3D model:', error);
            });

            // Handle Window Resize
            window.addEventListener('resize', () => {
                renderer.setSize(window.innerWidth, window.innerHeight);
                camera.aspect = window.innerWidth / window.innerHeight;
                camera.updateProjectionMatrix();
            });
        }

        function animate() {
            requestAnimationFrame(animate);
            renderer.render(scene, camera);
        }

        init();
    </script>
</body>
</html>

