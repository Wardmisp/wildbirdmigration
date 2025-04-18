# wildbirdmigration

My personal website

Next step :

### Ajouter du texte 3D autour du globe

Voici un exemple de la façon dont vous pouvez ajouter du texte 3D cliquable autour du globe :

1. **Installation des dépendances :**
   Assurez-vous d'avoir installé Three.js et ThreeGlobe :

   ```bash
   npm install three three-globe
   ```

2. **Configuration de base avec ThreeGlobe :**
   Voici un exemple de code pour ajouter du texte 3D autour du globe :

   ```html
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <title>ThreeGlobe with 3D Text</title>
       <style>
         body {
           margin: 0;
           overflow: hidden;
         }
       </style>
     </head>
     <body>
       <script type="module">
         import { Globe } from "three-globe";
         import * as THREE from "three";
         import { FontLoader } from "three/examples/jsm/loaders/FontLoader.js";
         import { TextGeometry } from "three/examples/jsm/geometries/TextGeometry.js";

         // Configuration de la scène
         const scene = new THREE.Scene();
         const camera = new THREE.PerspectiveCamera(
           75,
           window.innerWidth / window.innerHeight,
           0.1,
           1000
         );
         const renderer = new THREE.WebGLRenderer();
         renderer.setSize(window.innerWidth, window.innerHeight);
         document.body.appendChild(renderer.domElement);

         // Création du globe
         const globe = new Globe(renderer.domElement)
           .globeImageUrl("//unpkg.com/three-globe/example/img/earth-night.jpg")
           .bumpImageUrl(
             "//unpkg.com/three-globe/example/img/earth-topology.png"
           );

         scene.add(globe);

         // Position de la caméra
         camera.position.z = 3;

         // Chargement de la police pour le texte 3D
         const loader = new FontLoader();
         loader.load(
           "https://threejs.org/examples/fonts/helvetiker_regular.typeface.json",
           function (font) {
             // Création du texte 3D
             const textGeometry = new TextGeometry("Hello World", {
               font: font,
               size: 0.1,
               height: 0.01,
             });
             const textMaterial = new THREE.MeshBasicMaterial({
               color: 0xffffff,
             });
             const textMesh = new THREE.Mesh(textGeometry, textMaterial);

             // Positionnement du texte autour du globe
             textMesh.position.set(1, 1, 1);
             textMesh.lookAt(new THREE.Vector3(0, 0, 0));

             // Ajout du texte à la scène
             scene.add(textMesh);

             // Ajout d'un événement de clic sur le texte
             const raycaster = new THREE.Raycaster();
             const mouse = new THREE.Vector2();

             function onMouseClick(event) {
               mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
               mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
               raycaster.setFromCamera(mouse, camera);
               const intersects = raycaster.intersectObject(textMesh);
               if (intersects.length > 0) {
                 console.log("Text clicked!");
                 // Ajoutez ici les actions à effectuer lors du clic
               }
             }

             window.addEventListener("click", onMouseClick, false);
           }
         );

         // Boucle d'animation
         function animate() {
           requestAnimationFrame(animate);
           globe.rotation.y += 0.005;
           renderer.render(scene, camera);
         }
         animate();
       </script>
     </body>
   </html>
   ```

### Explications

1. **Chargement de la police :**

   - Utilisez `FontLoader` pour charger une police compatible avec Three.js.
   - Créez une géométrie de texte avec `TextGeometry`.

2. **Positionnement du texte :**

   - Positionnez le texte autour du globe en utilisant `textMesh.position.set`.
   - Utilisez `textMesh.lookAt` pour orienter le texte vers le centre du globe.

3. **Interactivité :**
   - Utilisez `Raycaster` pour détecter les clics sur le texte.
   - Ajoutez un événement de clic pour exécuter des actions lorsque le texte est cliqué.

### Conseils supplémentaires

- **Personnalisation :** Vous pouvez personnaliser la taille, la couleur et la police du texte selon vos besoins.
- **Performance :** Assurez-vous que les objets 3D ajoutés n'affectent pas les performances de votre application.
- **Autres objets 3D :** Vous pouvez ajouter d'autres objets 3D de la même manière, en utilisant les classes et méthodes de Three.js.

Si vous avez des questions supplémentaires ou besoin d'aide pour une partie spécifique, n'hésitez pas à demander !
