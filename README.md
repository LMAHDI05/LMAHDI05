<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Boutique en ligne 3D Mehdi</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="container">
        <div id="webgl-container"></div>
        <h1 class="site-name">Mehdi</h1>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="app.js"></script>
</body>
</html>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    overflow: hidden;
}

#container {
    text-align: center;
}

#webgl-container {
    width: 100vw;
    height: 80vh;
    background-color: #333;
}

.site-name {
    color: #fff;
    font-size: 3rem;
    font-weight: bold;
    position: absolute;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
}
// app.js
let scene, camera, renderer;
let cube, textMesh;

function init() {
    // Initialisation de la scène
    scene = new THREE.Scene();
    scene.background = new THREE.Color(0xeeeeee);  // Couleur de fond gris clair

    // Création de la caméra
    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

    // Initialisation du rendu
    renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.getElementById('webgl-container').appendChild(renderer.domElement);

    // Ajouter une lumière
    let light = new THREE.AmbientLight(0x404040); // lumière ambiante
    scene.add(light);

    // Ajouter une lumière directionnelle
    let directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    directionalLight.position.set(5, 5, 5);
    scene.add(directionalLight);

    // Création d'un cube pour représenter un produit (vêtement, par exemple)
    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshBasicMaterial({ color: 0x0077ff });
    cube = new THREE.Mesh(geometry, material);
    scene.add(cube);

    // Texte 3D avec le nom "Mehdi"
    const loader = new THREE.FontLoader();
    loader.load('https://threejs.org/examples/fonts/helvetiker_regular.typeface.json', function(font) {
        const textGeometry = new THREE.TextGeometry('Mehdi', {
            font: font,
            size: 1,
            height: 0.1,
        });
        const textMaterial = new THREE.MeshBasicMaterial({ color: 0xffcc00 });
        textMesh = new THREE.Mesh(textGeometry, textMaterial);
        textMesh.position.set(-3, 2, 0);
        scene.add(textMesh);
    });

    // Positionner la caméra
    camera.position.z = 5;

    // Fonction de rendu
    animate();
}

// Animation de la scène
function animate() {
    requestAnimationFrame(animate);

    // Rotation du cube pour donner un effet 3D
    if (cube) {
        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;
    }

    // Rotation du texte "Mehdi"
    if (textMesh) {
        textMesh.rotation.x += 0.01;
        textMesh.rotation.y += 0.01;
    }

    renderer.render(scene, camera);
}

// Redimensionnement automatique du canvas
window.addEventListener('resize', function() {
    renderer.setSize(window.innerWidth, window.innerHeight);
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
});

// Initialisation de la scène
init();
