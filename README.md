
# Joseph Potapenko
>I love **pizza**, don't take it _personally._
- [LinkedIn Profile](https://www.linkedin.com/in/joseph-potapenko-1788a7316/)

- [Github Profile](https://github.com/JosephPotapenko)

## Skills
- **3+ years of Customer Retention and Experience Services**
    - Retail Associate
    - Customer Experience Colleague
- **2+ years of Teaching and Tutoring Experience**
    - Teacher at a local nonprofit
    - Teaching assistant at Eastern Washington University
- **Bilingual** 
    - Fluent in Russian (Reading, Writing, Interpreting)
    - Intermediate communicaiton in Ukranian
- **Event Organization (2 years of volunteer at a local nonprofit)**
- **3 years of experience in Programming**
    - Python
    - Java
    - C
    - SQL
    - Assembly
- **3 year of Computer Related Skills**
    - Algorithms
    - Software Development with Agile
    - Data Structures
    - Networking
    - Cybersecurity
    - Design Patterns
    - Hardware Interfacing
    - Github
    - Linux
    - Ubuntu and Virtual Machines
    - Command Line Experience
    

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Globe</title>
    <style>
        body { margin: 0; }
        canvas { display: block; }
        #info {
            position: absolute;
            top: 10px;
            left: 10px;
            background: rgba(255, 255, 255, 0.8);
            padding: 10px;
            border-radius: 10px;
            backdrop-filter: blur(10px);
            display: none;
        }
    </style>
</head>
<body>
    <div id="info"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://d3js.org/d3.v6.min.js"></script>
    <script>
        // Set up the scene, camera, and renderer
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Create the globe
        const geometry = new THREE.SphereGeometry(5, 32, 32);
        const material = new THREE.MeshBasicMaterial({
            map: new THREE.TextureLoader().load('https://threejs.org/examples/textures/earth.jpg'),
            transparent: true,
            opacity: 0.7
        });
        const globe = new THREE.Mesh(geometry, material);
        scene.add(globe);

        // Position the camera
        camera.position.z = 10;

        // Load GeoJSON data and create clickable countries
        d3.json('https://raw.githubusercontent.com/johan/world.geo.json/master/countries.geo.json').then(data => {
            data.features.forEach(feature => {
                const country = new THREE.Shape();
                feature.geometry.coordinates[0].forEach(coord => {
                    country.moveTo(coord[0], coord[1]);
                });
                const countryGeometry = new THREE.ShapeGeometry(country);
                const countryMaterial = new THREE.MeshBasicMaterial({
                    color: 0xffffff,
                    transparent: true,
                    opacity: 0.5
                });
                const countryMesh = new THREE.Mesh(countryGeometry, countryMaterial);
                countryMesh.userData = { name: feature.properties.name };
                scene.add(countryMesh);

                countryMesh.on('click', () => {
                    document.getElementById('info').innerHTML = `Country: ${feature.properties.name}`;
                    document.getElementById('info').style.display = 'block';
                });
            });
        });

        // Render loop
        function animate() {
            requestAnimationFrame(animate);
            globe.rotation.y += 0.001;
            renderer.render(scene, camera);
        }
        animate();

        // Handle window resize
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>