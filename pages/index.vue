<template>
  <div class="banner-3d">
    <canvas ref="threeCanvas" class="three-canvas"></canvas>
    <!-- <div class="banner-title">¡Bienvenido a Webicultores!</div> -->
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import * as THREE from 'three'

// Import dinámico para GLTFLoader
let GLTFLoader

const threeCanvas = ref(null)
let renderer, scene, camera, animationId, astronaut, mixer

onMounted(async () => {
  const width = threeCanvas.value.clientWidth || window.innerWidth
  const height = 350

  // Importa GLTFLoader dinámicamente
  const module = await import('three/examples/jsm/loaders/GLTFLoader.js')
  GLTFLoader = module.GLTFLoader

  // Renderer
  renderer = new THREE.WebGLRenderer({
    canvas: threeCanvas.value,
    alpha: true,
    antialias: true,
  })
  renderer.setSize(width, height)
  renderer.setClearColor(0x000000, 1)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  renderer.shadowMap.enabled = true

  // Scene
  scene = new THREE.Scene()
  scene.background = new THREE.Color('black')

  // Camera
  camera = new THREE.PerspectiveCamera(45, width / height, 0.1, 100)
  camera.position.set(0, 1.5, 7)

  // Luces
  setupLights()

  // Cargar modelo GLB
  loadModel()

  // Interactividad hover, zoom y rotación con click
  let isHovering = false
  let isDragging = false
  let lastMouseX = 0

  // Animation loop
  function animate() {
    animationId = requestAnimationFrame(animate)
    if (mixer) mixer.update(0.016) // ~60fps
    if (astronaut) {
      if (isHovering) {
        // astronaut.rotation.y += 0.02 // rotación más rápida al hacer hover
      }
    }
    renderer.render(scene, camera)
  }
  animate()

  // Hover events
  threeCanvas.value.addEventListener('mouseenter', () => {
    isHovering = true
  })
  threeCanvas.value.addEventListener('mouseleave', () => {
    isHovering = false
  })

  // Rotación o movimiento con click y arrastre
  let lastMouseY = 0
  threeCanvas.value.addEventListener('mousedown', (event) => {
    isDragging = true
    lastMouseX = event.clientX
    lastMouseY = event.clientY
  })
  window.addEventListener('mouseup', () => {
    isDragging = false
  })
  window.addEventListener('mousemove', (event) => {
    if (!isDragging || !astronaut) return
    const deltaX = event.clientX - lastMouseX
    const deltaY = event.clientY - lastMouseY
    if (event.shiftKey) {
      // Mover posición X/Y
      astronaut.position.x += deltaX * 0.01
      astronaut.position.y -= deltaY * 0.01
    } else {
      // Rotar
      astronaut.rotation.y += deltaX * 0.01 // sensibilidad
    }
    lastMouseX = event.clientX
    lastMouseY = event.clientY
  })

  // Zoom con scroll
  threeCanvas.value.addEventListener(
    'wheel',
    (event) => {
      if (!astronaut) return
      event.preventDefault()
      const scaleStep = 0.1
      let newScale =
        astronaut.scale.x + (event.deltaY < 0 ? scaleStep : -scaleStep)
      newScale = Math.max(0.5, Math.min(5, newScale)) // Limita el zoom
      astronaut.scale.set(newScale, newScale, newScale)
    },
    { passive: false }
  )

  // Responsive resize
  const handleResize = () => {
    const newWidth = threeCanvas.value.clientWidth || window.innerWidth
    camera.aspect = newWidth / height
    camera.updateProjectionMatrix()
    renderer.setSize(newWidth, height)
  }
  window.addEventListener('resize', handleResize)

  onBeforeUnmount(() => {
    cancelAnimationFrame(animationId)
    renderer.dispose()
    window.removeEventListener('resize', handleResize)

    // Limpiar recursos
    if (astronaut) {
      scene.remove(astronaut)
      disposeScene(astronaut)
    }
    disposeScene(scene)
    if (mixer) {
      mixer.stopAllAction()
      mixer.uncacheRoot(astronaut)
      mixer = null
    }
  })

  function setupLights() {
    // Luz ambiental suave
    const ambientLight = new THREE.AmbientLight(0x404040, 0.5)
    scene.add(ambientLight)

    // Luz blanca intensa
    const whiteLight = new THREE.PointLight(0xffffff, 2, 100)
    whiteLight.position.set(0, 5, 5)
    scene.add(whiteLight)

    // Luz direccional principal
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1)
    directionalLight.position.set(5, 10, 7.5)
    directionalLight.castShadow = true
    scene.add(directionalLight)

    // Luz de relleno
    const fillLight = new THREE.DirectionalLight(0x1e90ff, 0.5)
    fillLight.position.set(-3, 3, 2)
    scene.add(fillLight)
  }

  async function loadModel() {
    try {
      const loader = new GLTFLoader()

      // Mostrar indicador de carga
      console.log('Cargando modelo...')

      const glb = await new Promise((resolve, reject) => {
        loader.load(
          '/drone.gltf',
          (gltf) => resolve(gltf),
          undefined,
          (err) => reject(err)
        )
      })

      if (!glb) {
        throw new Error('No se pudo cargar el modelo')
      }

      astronaut = glb.scene

      // Ajustar posición y escala
      astronaut.position.set(0, 0, 0)
      astronaut.scale.set(2.5, 2.5, 2.5)

      // Aplicar material dorado
      // const goldMaterial = new THREE.MeshStandardMaterial({
      //   color: 0xffd700, // dorado
      //   metalness: 1,
      //   roughness: 0.2,
      //   envMapIntensity: 1,
      // })

      astronaut.traverse((child) => {
        if (child.isMesh) {
          // child.material = goldMaterial
          // Mejorar sombreado
          child.castShadow = true
          child.receiveShadow = true
        }
      })

      scene.add(astronaut)
      console.log('Modelo cargado correctamente:', astronaut)

      // Animaciones GLTF
      if (glb.animations && glb.animations.length > 0) {
        mixer = new THREE.AnimationMixer(astronaut)
        glb.animations.forEach((clip) => {
          mixer.clipAction(clip).play()
        })
      }

      // Ajustar cámara para encuadrar el modelo
      const box = new THREE.Box3().setFromObject(astronaut)
      const center = box.getCenter(new THREE.Vector3())
      const size = box.getSize(new THREE.Vector3())

      camera.position.z = Math.max(size.x, size.y, size.z) * 2
      camera.lookAt(center)
    } catch (error) {
      console.error('Error al cargar el modelo:', error)
      // Mostrar un cubo de error si el modelo no se carga
      showErrorCube()
    }
  }

  function showErrorCube() {
    const geometry = new THREE.BoxGeometry(1, 1, 1)
    const material = new THREE.MeshBasicMaterial({ color: 0xff0000 })
    const cube = new THREE.Mesh(geometry, material)
    scene.add(cube)
    console.warn('Mostrando cubo de error en lugar del modelo')
  }

  function disposeScene(object) {
    if (!object) return

    // Liberar geometrías
    if (object.geometry) {
      object.geometry.dispose()
    }

    // Liberar materiales
    if (object.material) {
      if (Array.isArray(object.material)) {
        object.material.forEach((material) => material.dispose())
      } else {
        object.material.dispose()
      }
    }

    // Liberar texturas
    if (object.texture) {
      object.texture.dispose()
    }

    // Recorrer hijos
    if (object.children) {
      while (object.children.length > 0) {
        disposeScene(object.children[0])
        object.remove(object.children[0])
      }
    }
  }
})
</script>

<style scoped>
.banner-3d {
  position: relative;
  width: 100%;
  height: 100vh;
  background: #000;
  overflow: hidden;
  border-radius: 20px;
  box-shadow: 0 4px 24px 0 rgba(0, 0, 0, 0.12);
  margin-bottom: 36px;
}
.three-canvas {
  width: 100%;
  height: 100%;
  display: block;
}
.banner-title {
  position: absolute;
  left: 32px;
  top: 32px;
  font-size: 2.2rem;
  font-weight: bold;
  color: #fff;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.8);
  z-index: 2;
  font-family: 'Arial', sans-serif;
}

/* Ajustes responsivos */
@media (max-width: 768px) {
  .banner-title {
    font-size: 1.8rem;
    left: 16px;
    top: 20px;
  }
}
</style>
