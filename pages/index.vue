template
<template>
  <div style="width: 100vw; height: 100vh; position: fixed; top: 0; left: 0">
    <LoadingSpinner v-if="loading" />
    <div
      ref="threeContainer"
      style="width: 100vw; height: 100vh; position: fixed; top: 0; left: 0"
    ></div>
    <div
      v-if="tooltip.visible"
      :style="{
        position: 'fixed',
        left: tooltip.x + 'px',
        top: tooltip.y + 'px',
        background: 'rgba(30,30,30,0.95)',
        color: '#fff',
        padding: '8px 16px',
        borderRadius: '8px',
        pointerEvents: 'auto',
        zIndex: 10000,
        fontSize: '1.1em',
        boxShadow: '0 2px 12px rgba(0,0,0,0.2)',
      }"
    >
      <TooltipMenu @navigate="handleTooltipNavigate" />
    </div>
  </div>
</template>

<script>
import * as THREE from 'three'

import TooltipMenu from '~/components/TooltipMenu.vue'
import LoadingSpinner from '~/components/LoadingSpinner.vue'

export default {
  name: 'ThreeDroneScene',
  components: {
    TooltipMenu,
    LoadingSpinner,
  },
  data() {
    return {
      tooltip: {
        visible: false,
        x: 0,
        y: 0,
        timeout: null,
      },
      loading: true,
    }
  },
  methods: {
    handleTooltipNavigate(path) {
      this.tooltip.visible = false
      this.$router.push(path)
    },
  },
  async mounted() {
    // Variables
    this.scene = new THREE.Scene()
    this.scene.background = null // Fondo transparente

    this.camera = new THREE.PerspectiveCamera(
      45,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    )
    this.camera.position.set(0, 2, 5)

    this.renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true })
    this.renderer.setClearColor(0x000000, 0) // Transparente
    this.renderer.setSize(window.innerWidth, window.innerHeight)
    this.renderer.setPixelRatio(window.devicePixelRatio)
    this.$refs.threeContainer.appendChild(this.renderer.domElement)
    this.renderer.domElement.style.width = '100vw'
    this.renderer.domElement.style.height = '100vh'

    // Luces
    const ambientLight = new THREE.AmbientLight(0xffffff, 1.2)
    this.scene.add(ambientLight)
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1.2)
    directionalLight.position.set(5, 10, 7.5)
    this.scene.add(directionalLight)

    // Cargar modelo GLTF dinámicamente
    const { GLTFLoader } = await import(
      'three/examples/jsm/loaders/GLTFLoader.js'
    )
    const loader = new GLTFLoader()
    loader.load('/drone.gltf', (gltf) => {
      this.loading = false
      this.model = gltf.scene
      // Centrar el modelo en el origen
      const box = new THREE.Box3().setFromObject(this.model)
      const center = box.getCenter(new THREE.Vector3())
      this.model.position.sub(center)
      // Escalar el modelo para que sea más grande
      const size = box.getSize(new THREE.Vector3())
      const maxDim = Math.max(size.x, size.y, size.z)
      let scale = 2.5 / maxDim // Ajusta este valor para más o menos tamaño
      // Ajuste responsivo para móvil
      if (window.innerWidth < 600) {
        scale = 1.4 / maxDim
      }
      this.model.scale.setScalar(scale)
      // Opcional: ajustar la cámara para encuadrar mejor
      this.camera.position.set(0, 0, 5)
      this.camera.lookAt(0, 0, 0)
      this.scene.add(this.model)
      // Animación GLTF
      if (gltf.animations && gltf.animations.length > 0) {
        this.mixer = new THREE.AnimationMixer(this.model)
        this.action = this.mixer.clipAction(gltf.animations[0])
        this.action.paused = true
        this.action.play()
      }
    })

    // Animación
    this.clock = new THREE.Clock()
    this.isModelHovered = false
    this.animate = () => {
      requestAnimationFrame(this.animate)
      // Actualiza animación solo si está en hover
      if (this.mixer && this.isModelHovered) {
        const delta = this.clock.getDelta()
        this.mixer.update(delta)
      } else if (this.clock) {
        this.clock.getDelta() // Mantén el reloj actualizado
      }
      this.renderer.render(this.scene, this.camera)
    }
    this.animate()

    // Interacción con mouse y touch
    this.isDragging = false
    this.prevPointer = { x: 0, y: 0 }
    this.rotation = { x: 0, y: 0 }

    // Raycaster para detectar hover sobre el modelo
    this.raycaster = new THREE.Raycaster()
    this.pointer = new THREE.Vector2()

    const getPointer = (e) => {
      if (e.touches && e.touches.length > 0) {
        return { x: e.touches[0].clientX, y: e.touches[0].clientY }
      } else {
        return { x: e.clientX, y: e.clientY }
      }
    }

    const checkModelHover = (clientX, clientY) => {
      // Normaliza coordenadas de pantalla a -1 a 1
      this.pointer.x = (clientX / window.innerWidth) * 2 - 1
      this.pointer.y = -(clientY / window.innerHeight) * 2 + 1
      this.raycaster.setFromCamera(this.pointer, this.camera)
      if (this.model) {
        const intersects = this.raycaster.intersectObject(this.model, true)
        this.isModelHovered = intersects.length > 0
        // Si hay animación, pausa o reanuda
        if (this.action) this.action.paused = !this.isModelHovered
      }
    }

    const onPointerDown = (e) => {
      this.isDragging = true
      const p = getPointer(e)
      this.prevPointer = { x: p.x, y: p.y }
    }
    const onPointerUp = () => {
      this.isDragging = false
    }
    const onPointerMove = (e) => {
      if (!this.isDragging || !this.model) return
      const p = getPointer(e)
      const dx = (p.x - this.prevPointer.x) / window.innerWidth
      const dy = (p.y - this.prevPointer.y) / window.innerHeight
      this.rotation.y += dx * Math.PI * 2
      this.rotation.x += dy * Math.PI * 2
      // Limita la rotación en X para no voltear
      this.rotation.x = Math.max(
        -Math.PI / 2,
        Math.min(Math.PI / 2, this.rotation.x)
      )
      this.model.rotation.y = this.rotation.y
      this.model.rotation.x = this.rotation.x
      this.prevPointer = { x: p.x, y: p.y }
      // Detecta hover mientras se arrastra
      checkModelHover(p.x, p.y)
    }
    // Detecta hover con mouse y touch
    this.renderer.domElement.addEventListener('mousemove', (e) =>
      checkModelHover(e.clientX, e.clientY)
    )
    this.renderer.domElement.addEventListener('touchmove', (e) => {
      if (e.touches && e.touches.length > 0)
        checkModelHover(e.touches[0].clientX, e.touches[0].clientY)
    })
    // Click sobre el modelo para mostrar tooltip
    const onModelClick = (e) => {
      let clientX, clientY
      if (e.touches && e.touches.length > 0) {
        clientX = e.touches[0].clientX
        clientY = e.touches[0].clientY
      } else {
        clientX = e.clientX
        clientY = e.clientY
      }
      // Raycast para verificar si se hizo click sobre el modelo
      this.pointer.x = (clientX / window.innerWidth) * 2 - 1
      this.pointer.y = -(clientY / window.innerHeight) * 2 + 1
      this.raycaster.setFromCamera(this.pointer, this.camera)
      if (this.model) {
        const intersects = this.raycaster.intersectObject(this.model, true)
        if (intersects.length > 0) {
          // Muestra el tooltip
          this.tooltip.visible = true
          this.tooltip.x = clientX + 10
          this.tooltip.y = clientY - 10
          if (this.tooltip.timeout) clearTimeout(this.tooltip.timeout)
          this.tooltip.timeout = setTimeout(() => {
            this.tooltip.visible = false
          }, 4500)
        }
      }
    }
    this.renderer.domElement.addEventListener('click', onModelClick)
    this.renderer.domElement.addEventListener('touchend', onModelClick)

    this.renderer.domElement.addEventListener('mousedown', onPointerDown)
    this.renderer.domElement.addEventListener('touchstart', onPointerDown)
    window.addEventListener('mousemove', onPointerMove)
    window.addEventListener('touchmove', onPointerMove)
    window.addEventListener('mouseup', onPointerUp)
    window.addEventListener('touchend', onPointerUp)

    // Limpieza de eventos en beforeDestroy
    this._remove3dEvents = () => {
      this.renderer.domElement.removeEventListener('mousedown', onPointerDown)
      this.renderer.domElement.removeEventListener('touchstart', onPointerDown)
      window.removeEventListener('mousemove', onPointerMove)
      window.removeEventListener('touchmove', onPointerMove)
      window.removeEventListener('mouseup', onPointerUp)
      window.removeEventListener('touchend', onPointerUp)
      this.renderer.domElement.removeEventListener('click', onModelClick)
      this.renderer.domElement.removeEventListener('touchend', onModelClick)
      if (this.tooltip.timeout) clearTimeout(this.tooltip.timeout)
    }

    // Responsive
    this.onWindowResize = () => {
      this.camera.aspect = window.innerWidth / window.innerHeight
      this.camera.updateProjectionMatrix()
      this.renderer.setSize(window.innerWidth, window.innerHeight)
    }
    window.addEventListener('resize', this.onWindowResize)
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.onWindowResize)
    if (this._remove3dEvents) this._remove3dEvents()
    if (this.renderer) {
      this.renderer.dispose()
    }
    if (this.scene) {
      this.scene.clear()
    }
    if (this.$refs.threeContainer && this.renderer) {
      this.$refs.threeContainer.removeChild(this.renderer.domElement)
    }
  },
}
</script>

<style scoped>
div[ref='threeContainer'] {
  overflow: hidden;
}
</style>
