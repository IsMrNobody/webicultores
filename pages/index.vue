<template>
  <div class="aquatic-scene-wrapper">
    <!-- Pantalla de Carga Inicial -->
    <LoadingSpinner v-if="loading" />

    <!-- Canvas Three.js -->
    <div ref="threeContainer" class="three-viewport"></div>

    <!-- Fondo de Cáusticas y Ondas Acuáticas CSS -->
    <div class="water-caustics-backdrop">
      <div class="caustic-layer layer-1"></div>
      <div class="caustic-layer layer-2"></div>
      <div class="vignette-overlay"></div>
    </div>

    <!-- Header / Brand Overlay -->
    <header class="brand-hud">
      <div class="brand-badge">
        <span class="pulse-dot"></span>
        <span class="hud-mono">SISTEMA BIO-DIGITAL ACTIVO</span>
      </div>
      <h1 class="brand-title">WEBICULTORES</h1>
      <p class="brand-tagline">
        Cultura Web · Ecosistemas Interactivos · RestoTech
      </p>
    </header>

    <!-- Indicador de Acción Central / Instrucción de Germinación -->
    <div class="interaction-prompt" :class="{ 'is-germinated': isGerminated }">
      <button class="germinate-toggle-btn" @click="toggleGermination">
        <span class="btn-glow"></span>
        <v-icon left size="18" color="#00f5d4">
          {{ isGerminated ? 'mdi-collapse-all-outline' : 'mdi-sprout-outline' }}
        </v-icon>
        <span>
          {{
            isGerminated ? 'CONTRAER ECOSISTEMA' : 'TOCA EL ORBE PARA GERMINAR'
          }}
        </span>
      </button>
    </div>

    <!-- Tarjeta Holográfica del Nodo Seleccionado / Activo en 3D -->
    <transition name="holo-fade">
      <div
        v-if="hoveredNode && isGerminated"
        class="holographic-preview-card"
        :style="{
          left: hoveredNodeScreenPos.x + 'px',
          top: hoveredNodeScreenPos.y + 'px',
        }"
      >
        <div class="card-inner" :style="{ borderColor: hoveredNode.color }">
          <div
            class="card-glow"
            :style="{ background: hoveredNode.color }"
          ></div>
          <div class="d-flex align-center justify-space-between mb-1">
            <span class="card-badge" :style="{ color: hoveredNode.color }">
              {{ hoveredNode.badge }}
            </span>
            <span class="card-icon">{{ hoveredNode.icon }}</span>
          </div>
          <h3 class="card-title">{{ hoveredNode.title }}</h3>
          <p class="card-desc">{{ hoveredNode.subtitle }}</p>
          <div class="card-action-hint">
            <span>Haz clic para explorar</span>
            <v-icon size="14" color="white">mdi-arrow-right</v-icon>
          </div>
        </div>
      </div>
    </transition>

    <!-- Barra de Navegación Rápida Inferior (Respaldo Accesible) -->
    <nav class="quick-nav-bar" :class="{ 'is-open': isGerminated }">
      <div class="quick-nav-pills">
        <button
          v-for="node in nodesConfig"
          :key="node.id"
          class="quick-pill-btn"
          :class="{ active: hoveredNode && hoveredNode.id === node.id }"
          @click="navigateNode(node)"
          @mouseenter="onPillHover(node)"
          @mouseleave="onPillLeave"
        >
          <span class="pill-dot" :style="{ background: node.color }"></span>
          <span class="pill-title">{{ node.title }}</span>
        </button>
      </div>
    </nav>
  </div>
</template>

<script>
import * as THREE from 'three'
import { gsap } from 'gsap'
import LoadingSpinner from '~/components/LoadingSpinner.vue'

export default {
  name: 'WebicultoresAquaticScene',
  components: {
    LoadingSpinner,
  },
  data() {
    return {
      loading: true,
      isGerminated: false,
      hoveredNode: null,
      hoveredNodeScreenPos: { x: 0, y: 0 },
      nodesConfig: [
        {
          id: 'restotech',
          title: 'Menús Digitales',
          subtitle:
            'Especialidad RestoTech: pedidos directos a WhatsApp, QR interactivo y 0% comisiones.',
          badge: 'CASO ESTRELLA',
          icon: '🍕',
          color: '#ff007f', // Fucsia neón
          emissive: 0xff007f,
          route: '/restaurantes',
          angle: 0,
        },
        {
          id: 'portfolio',
          title: 'Portafolio Web',
          subtitle:
            'Plataformas web a medida, interfaces de alto impacto y aplicaciones interactivas.',
          badge: 'PROYECTOS',
          icon: '🌿',
          color: '#00f5d4', // Turquesa bioluminiscente
          emissive: 0x00f5d4,
          route: '/portfolio',
          angle: Math.PI * 0.4,
        },
        {
          id: 'branding',
          title: 'Branding & Diseño',
          subtitle:
            'Identidad visual, diseño editorial, logotipos y lenguaje de marca distintivo.',
          badge: 'DISEÑO',
          icon: '🎨',
          color: '#a855f7', // Violeta eléctrico
          emissive: 0xa855f7,
          route: '/branding',
          angle: Math.PI * 0.8,
        },
        {
          id: 'video',
          title: 'Video & Motion',
          subtitle:
            'Contenido audiovisual cinemático, animación y spots publicitarios dinámicos.',
          badge: 'MULTIMEDIA',
          icon: '🎥',
          color: '#38bdf8', // Cian cielo
          emissive: 0x38bdf8,
          route: '/video',
          angle: Math.PI * 1.2,
        },
        {
          id: 'whatsapp',
          title: 'Contacto WhatsApp',
          subtitle:
            'Comunícate directamente con nuestro equipo de desarrollo y cotiza tu proyecto.',
          badge: 'EN LÍNEA',
          icon: '💬',
          color: '#22c55e', // Verde esmeralda
          emissive: 0x22c55e,
          url: 'https://wa.me/584128352365?text=%C2%A1Hola%20Webicultores!%20Deseo%20cotizar%20un%20proyecto%20web.',
          angle: Math.PI * 1.6,
        },
      ],
    }
  },
  head() {
    return {
      title: 'Webicultores - Experiencia Bio-Digital 3D',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content:
            'Ecosistema web interactivo de Webicultores. Innovación en desarrollo web, soluciones RestoTech y diseño 3D.',
        },
      ],
    }
  },
  async mounted() {
    this.initThree()
    await this.load3dModel()
    this.setupInteractions()
    this.startLoop()
  },
  beforeDestroy() {
    this.cleanupThree()
  },
  methods: {
    initThree() {
      // 1. Escena
      this.scene = new THREE.Scene()

      // 2. Cámara
      const aspect = window.innerWidth / window.innerHeight
      this.camera = new THREE.PerspectiveCamera(46, aspect, 0.1, 100)
      this.camera.position.set(0, 0, 5.8)
      this.targetCameraZ = 5.8

      // 3. Renderer optimizado con High Performance
      this.renderer = new THREE.WebGLRenderer({
        alpha: true,
        antialias: true,
        powerPreference: 'high-performance',
      })
      this.renderer.setSize(window.innerWidth, window.innerHeight)
      // Ajuste de DPR para garantizar 60 FPS en móviles sin sobrecalentamiento
      const dpr = Math.min(window.devicePixelRatio || 1, 1.75)
      this.renderer.setPixelRatio(dpr)
      this.renderer.toneMapping = THREE.ACESFilmicToneMapping
      this.renderer.toneMappingExposure = 1.15
      this.$refs.threeContainer.appendChild(this.renderer.domElement)

      // 4. Luces ambientales y puntuales bioluminiscentes
      const ambientLight = new THREE.AmbientLight(0x00f5d4, 0.45)
      this.scene.add(ambientLight)

      const topSunLight = new THREE.DirectionalLight(0xffffff, 1.8)
      topSunLight.position.set(2, 6, 4)
      this.scene.add(topSunLight)

      // Luces de acento de bioluminiscencia marina
      this.magentaLight = new THREE.PointLight(0xff007f, 3.2, 12)
      this.magentaLight.position.set(-3.5, -2, 2.5)
      this.scene.add(this.magentaLight)

      this.cyanLight = new THREE.PointLight(0x00f5d4, 3.0, 12)
      this.cyanLight.position.set(3.5, 2.5, 2.5)
      this.scene.add(this.cyanLight)

      // 5. Grupo del Núcleo Central ("Aquatic Bio-Core")
      this.coreGroup = new THREE.Group()
      this.scene.add(this.coreGroup)

      // 6. Construir Orbe de Agua Cristalina (Water Bubble Mesh)
      const orbGeometry = new THREE.SphereGeometry(1.4, 48, 48)
      this.orbMaterial = new THREE.MeshPhysicalMaterial({
        color: new THREE.Color(0xa2f5ff),
        emissive: new THREE.Color(0x003d52),
        emissiveIntensity: 0.25,
        roughness: 0.08,
        metalness: 0.05,
        transmission: 0.88,
        ior: 1.333, // Refracción del agua pura
        thickness: 0.9,
        transparent: true,
        opacity: 0.85,
        clearcoat: 1.0,
        clearcoatRoughness: 0.1,
      })
      this.waterOrb = new THREE.Mesh(orbGeometry, this.orbMaterial)
      this.waterOrb.userData = { isCore: true }
      this.coreGroup.add(this.waterOrb)

      // 7. Anillos Cibernéticos de Raíces Bioluminiscentes (Bio-Rings)
      this.rings = []
      const ringConfigs = [
        {
          radius: 1.6,
          tube: 0.02,
          color: 0x00f5d4,
          speed: 0.015,
          rot: [0.8, 0, 0],
        },
        {
          radius: 1.78,
          tube: 0.018,
          color: 0xff007f,
          speed: -0.012,
          rot: [0, 0.7, 0.4],
        },
        {
          radius: 1.95,
          tube: 0.015,
          color: 0x38bdf8,
          speed: 0.009,
          rot: [0.4, 0.3, 0.8],
        },
      ]

      ringConfigs.forEach((cfg) => {
        const ringGeo = new THREE.TorusGeometry(cfg.radius, cfg.tube, 16, 80)
        const ringMat = new THREE.MeshStandardMaterial({
          color: cfg.color,
          emissive: cfg.color,
          emissiveIntensity: 1.8,
          roughness: 0.2,
          metalness: 0.8,
          wireframe: false,
        })
        const ringMesh = new THREE.Mesh(ringGeo, ringMat)
        ringMesh.rotation.set(...cfg.rot)
        ringMesh.userData = {
          speed: cfg.speed,
          isRing: true,
          baseRadius: cfg.radius,
        }
        this.coreGroup.add(ringMesh)
        this.rings.push(ringMesh)
      })

      // 8. Campo de Partículas de Plancton Bioluminiscente
      this.createPlanktonField()

      // 9. Construir Nodos Orbitales 3D ("Frutos Digitales")
      this.createOrbitalNodes()

      // Reloj y Variables de Animación
      this.clock = new THREE.Clock()
      this.pointer = new THREE.Vector2()
      this.raycaster = new THREE.Raycaster()
      this.dragOffset = { x: 0, y: 0 }
      this.isDragging = false
      this.prevMouse = { x: 0, y: 0 }
    },

    createPlanktonField() {
      const count = 550
      const geo = new THREE.BufferGeometry()
      const positions = new Float32Array(count * 3)
      const colors = new Float32Array(count * 3)
      const scales = new Float32Array(count)

      const colorCyan = new THREE.Color(0x00f5d4)
      const colorPink = new THREE.Color(0xff007f)
      const colorBlue = new THREE.Color(0x00b4d8)

      for (let i = 0; i < count; i++) {
        const i3 = i * 3
        positions[i3] = (Math.random() - 0.5) * 16
        positions[i3 + 1] = (Math.random() - 0.5) * 12
        positions[i3 + 2] = (Math.random() - 0.5) * 12

        // Alternar colores de bioluminiscencia
        const rand = Math.random()
        let c = colorCyan
        if (rand > 0.7) c = colorPink
        else if (rand > 0.4) c = colorBlue

        colors[i3] = c.r
        colors[i3 + 1] = c.g
        colors[i3 + 2] = c.b

        scales[i] = Math.random() * 0.08 + 0.02
      }

      geo.setAttribute('position', new THREE.BufferAttribute(positions, 3))
      geo.setAttribute('color', new THREE.BufferAttribute(colors, 3))

      // Crear textura circular suave para las partículas sin assets externos
      const canvas = document.createElement('canvas')
      canvas.width = 32
      canvas.height = 32
      const ctx = canvas.getContext('2d')
      const grad = ctx.createRadialGradient(16, 16, 0, 16, 16, 16)
      grad.addColorStop(0, 'rgba(255, 255, 255, 1)')
      grad.addColorStop(0.4, 'rgba(0, 245, 212, 0.8)')
      grad.addColorStop(1, 'rgba(0, 0, 0, 0)')
      ctx.fillStyle = grad
      ctx.fillRect(0, 0, 32, 32)
      const particleTexture = new THREE.CanvasTexture(canvas)

      const mat = new THREE.PointsMaterial({
        size: 0.12,
        map: particleTexture,
        vertexColors: true,
        transparent: true,
        opacity: 0.75,
        blending: THREE.AdditiveBlending,
        depthWrite: false,
      })

      this.planktonField = new THREE.Points(geo, mat)
      this.scene.add(this.planktonField)
    },

    createOrbitalNodes() {
      this.nodesGroup = new THREE.Group()
      this.scene.add(this.nodesGroup)
      this.orbitalMeshes = []
      this.orbitalDistance = 3.3

      this.nodesConfig.forEach((cfg, index) => {
        const nodeContainer = new THREE.Group()
        nodeContainer.userData = { config: cfg, index, isNode: true }

        // 1. Núcleo esférico del nodo
        const coreGeo = new THREE.SphereGeometry(0.32, 32, 32)
        const coreMat = new THREE.MeshStandardMaterial({
          color: new THREE.Color(cfg.color),
          emissive: new THREE.Color(cfg.emissive),
          emissiveIntensity: 2.2,
          roughness: 0.15,
          metalness: 0.85,
        })
        const coreMesh = new THREE.Mesh(coreGeo, coreMat)
        coreMesh.userData = { isNode: true, config: cfg }
        nodeContainer.add(coreMesh)

        // 2. Halo de refracción acuática alrededor del nodo
        const haloGeo = new THREE.SphereGeometry(0.46, 24, 24)
        const haloMat = new THREE.MeshPhysicalMaterial({
          color: new THREE.Color(cfg.color),
          transmission: 0.8,
          roughness: 0.1,
          ior: 1.25,
          transparent: true,
          opacity: 0.45,
          wireframe: true,
        })
        const haloMesh = new THREE.Mesh(haloGeo, haloMat)
        nodeContainer.add(haloMesh)

        // 3. Mini anillo orbital del nodo
        const nodeRingGeo = new THREE.TorusGeometry(0.55, 0.015, 12, 36)
        const nodeRingMat = new THREE.MeshStandardMaterial({
          color: cfg.color,
          emissive: cfg.color,
          emissiveIntensity: 1.5,
        })
        const nodeRing = new THREE.Mesh(nodeRingGeo, nodeRingMat)
        nodeRing.rotation.x = Math.PI / 3
        nodeContainer.add(nodeRing)

        // Posición inicial: contraído dentro del núcleo
        nodeContainer.position.set(0, 0, 0)
        nodeContainer.scale.set(0.001, 0.001, 0.001)

        this.nodesGroup.add(nodeContainer)
        this.orbitalMeshes.push({
          container: nodeContainer,
          coreMesh,
          haloMesh,
          nodeRing,
          config: cfg,
          currentAngle: cfg.angle,
        })
      })
    },

    async load3dModel() {
      try {
        const { GLTFLoader } = await import(
          'three/examples/jsm/loaders/GLTFLoader.js'
        )
        const loader = new GLTFLoader()

        loader.load(
          '/drone.gltf',
          (gltf) => {
            this.droneModel = gltf.scene

            // Centrar y escalar el modelo dentro de la burbuja
            const box = new THREE.Box3().setFromObject(this.droneModel)
            const center = box.getCenter(new THREE.Vector3())
            this.droneModel.position.sub(center)

            const size = box.getSize(new THREE.Vector3())
            const maxDim = Math.max(size.x, size.y, size.z)
            const scale = 1.7 / maxDim
            this.droneModel.scale.setScalar(scale)

            // Ajuste sutil de materiales para acoplar con la atmósfera bioluminiscente
            this.droneModel.traverse((child) => {
              if (child.isMesh && child.material) {
                child.material.roughness = Math.max(
                  0.2,
                  child.material.roughness || 0.3
                )
                if (child.material.emissive) {
                  child.material.emissiveIntensity = 1.3
                }
              }
            })

            // Configurar animación GLTF si existe
            if (gltf.animations && gltf.animations.length > 0) {
              this.mixer = new THREE.AnimationMixer(this.droneModel)
              this.action = this.mixer.clipAction(gltf.animations[0])
              this.action.play()
            }

            this.coreGroup.add(this.droneModel)
            this.loading = false
          },
          undefined,
          () => {
            this.loading = false
          }
        )
      } catch (e) {
        this.loading = false
      }
    },

    setupInteractions() {
      const dom = this.renderer.domElement

      // 1. Detección de puntero para Raycasting y Parallax
      const onPointerMove = (e) => {
        let clientX = e.clientX
        let clientY = e.clientY
        if (e.touches && e.touches.length > 0) {
          clientX = e.touches[0].clientX
          clientY = e.touches[0].clientY
        }

        this.pointer.x = (clientX / window.innerWidth) * 2 - 1
        this.pointer.y = -(clientY / window.innerHeight) * 2 + 1

        // Arrastre para rotación libre del núcleo
        if (this.isDragging) {
          const deltaX = (clientX - this.prevMouse.x) / window.innerWidth
          const deltaY = (clientY - this.prevMouse.y) / window.innerHeight

          this.coreGroup.rotation.y += deltaX * Math.PI * 2
          this.coreGroup.rotation.x += deltaY * Math.PI * 2
          this.coreGroup.rotation.x = Math.max(
            -Math.PI / 3,
            Math.min(Math.PI / 3, this.coreGroup.rotation.x)
          )

          this.prevMouse = { x: clientX, y: clientY }
        }

        // Raycasting de hover sobre nodos u orbe
        this.checkRaycastHover(clientX, clientY)
      }

      const onPointerDown = (e) => {
        this.isDragging = true
        let clientX = e.clientX
        let clientY = e.clientY
        if (e.touches && e.touches.length > 0) {
          clientX = e.touches[0].clientX
          clientY = e.touches[0].clientY
        }
        this.prevMouse = { x: clientX, y: clientY }
      }

      const onPointerUp = () => {
        this.isDragging = false
      }

      // 2. Clic / Tap
      const onPointerClick = (e) => {
        let clientX = e.clientX
        let clientY = e.clientY
        if (e.touches && e.touches.length > 0) {
          clientX = e.touches[0].clientX
          clientY = e.touches[0].clientY
        }

        this.pointer.x = (clientX / window.innerWidth) * 2 - 1
        this.pointer.y = -(clientY / window.innerHeight) * 2 + 1
        this.raycaster.setFromCamera(this.pointer, this.camera)

        // Verificar intersección con nodos orbitales primero
        if (this.isGerminated && this.orbitalMeshes.length > 0) {
          const nodeObjects = this.orbitalMeshes.map((m) => m.coreMesh)
          const intersects = this.raycaster.intersectObjects(nodeObjects, true)

          if (intersects.length > 0) {
            const hitNode = intersects[0].object.userData.config
            if (hitNode) {
              this.navigateNode(hitNode)
              return
            }
          }
        }

        // Verificar intersección con el orbe central
        const coreObjects = [this.waterOrb]
        if (this.droneModel) coreObjects.push(this.droneModel)

        const coreIntersects = this.raycaster.intersectObjects(
          coreObjects,
          true
        )
        if (coreIntersects.length > 0) {
          this.toggleGermination()
        }
      }

      dom.addEventListener('mousemove', onPointerMove)
      dom.addEventListener('touchmove', onPointerMove, { passive: true })
      dom.addEventListener('mousedown', onPointerDown)
      dom.addEventListener('touchstart', onPointerDown, { passive: true })
      window.addEventListener('mouseup', onPointerUp)
      window.addEventListener('touchend', onPointerUp)
      dom.addEventListener('click', onPointerClick)

      // 3. Responsive resize
      this.onResize = () => {
        this.camera.aspect = window.innerWidth / window.innerHeight
        this.camera.updateProjectionMatrix()
        this.renderer.setSize(window.innerWidth, window.innerHeight)
      }
      window.addEventListener('resize', this.onResize)

      this._cleanupListeners = () => {
        dom.removeEventListener('mousemove', onPointerMove)
        dom.removeEventListener('touchmove', onPointerMove)
        dom.removeEventListener('mousedown', onPointerDown)
        dom.removeEventListener('touchstart', onPointerDown)
        window.removeEventListener('mouseup', onPointerUp)
        window.removeEventListener('touchend', onPointerUp)
        dom.removeEventListener('click', onPointerClick)
        window.removeEventListener('resize', this.onResize)
      }
    },

    checkRaycastHover(clientX, clientY) {
      if (!this.isGerminated) {
        // En estado inactivo, verificar si el cursor está sobre el núcleo
        this.raycaster.setFromCamera(this.pointer, this.camera)
        const intersects = this.raycaster.intersectObject(this.waterOrb, true)
        this.isCoreHovered = intersects.length > 0
        document.body.style.cursor = this.isCoreHovered ? 'pointer' : 'default'
        return
      }

      this.raycaster.setFromCamera(this.pointer, this.camera)
      const nodeObjects = this.orbitalMeshes.map((m) => m.coreMesh)
      const intersects = this.raycaster.intersectObjects(nodeObjects, true)

      if (intersects.length > 0) {
        const targetConfig = intersects[0].object.userData.config
        if (targetConfig) {
          this.hoveredNode = targetConfig
          this.hoveredNodeScreenPos = {
            x: Math.min(window.innerWidth - 260, Math.max(20, clientX - 110)),
            y: Math.max(80, clientY - 140),
          }
          document.body.style.cursor = 'pointer'

          // Escalar nodo hovered en 3D
          const meshObj = this.orbitalMeshes.find(
            (m) => m.config.id === targetConfig.id
          )
          if (meshObj) {
            gsap.to(meshObj.container.scale, {
              x: 1.35,
              y: 1.35,
              z: 1.35,
              duration: 0.35,
              overwrite: 'auto',
            })
          }
          return
        }
      }

      // Si no hay hover sobre nodos
      if (this.hoveredNode) {
        const prevId = this.hoveredNode.id
        const meshObj = this.orbitalMeshes.find((m) => m.config.id === prevId)
        if (meshObj) {
          gsap.to(meshObj.container.scale, {
            x: 1,
            y: 1,
            z: 1,
            duration: 0.35,
            overwrite: 'auto',
          })
        }
        this.hoveredNode = null
      }

      // Hover sobre el orbe para contraer
      const coreIntersects = this.raycaster.intersectObject(this.waterOrb, true)
      document.body.style.cursor =
        coreIntersects.length > 0 ? 'pointer' : 'default'
    },

    toggleGermination() {
      this.isGerminated = !this.isGerminated

      if (this.isGerminated) {
        this.animateGerminateOpen()
      } else {
        this.animateGerminateClose()
      }
    },

    animateGerminateOpen() {
      // 1. Onda de choque y expansión del orbe acuático
      gsap.to(this.waterOrb.scale, {
        x: 1.25,
        y: 1.25,
        z: 1.25,
        duration: 0.45,
        ease: 'power2.out',
        onComplete: () => {
          gsap.to(this.waterOrb.scale, {
            x: 1.05,
            y: 1.05,
            z: 1.05,
            duration: 0.8,
            ease: 'elastic.out(1, 0.4)',
          })
        },
      })

      // 2. Cambio a bioluminiscencia fucsia/magenta neón
      gsap.to(this.orbMaterial.emissive, {
        r: 1.0,
        g: 0.0,
        b: 0.5,
        duration: 0.6,
      })
      gsap.to(this.orbMaterial, {
        emissiveIntensity: 0.75,
        duration: 0.6,
      })

      // 3. Expansión de los anillos bio-cibernéticos
      this.rings.forEach((ring, idx) => {
        gsap.to(ring.scale, {
          x: 1.35 + idx * 0.1,
          y: 1.35 + idx * 0.1,
          z: 1.35 + idx * 0.1,
          duration: 0.9,
          ease: 'back.out(1.8)',
          delay: idx * 0.06,
        })
      })

      // 4. Retroceso cinemático de cámara
      gsap.to(this.camera.position, {
        z: 7.2,
        duration: 1.2,
        ease: 'power3.out',
      })

      // 5. Desprendimiento y germinación de los Frutos Digitales (Nodos Orbitales)
      const radius = window.innerWidth < 600 ? 2.5 : 3.4
      this.orbitalMeshes.forEach((item, idx) => {
        const angle = item.currentAngle
        const targetX = Math.cos(angle) * radius
        const targetY = Math.sin(angle * 2) * 0.45
        const targetZ = Math.sin(angle) * radius

        // Animación elástica de posición y escala
        gsap.to(item.container.position, {
          x: targetX,
          y: targetY,
          z: targetZ,
          duration: 1.1,
          delay: 0.15 + idx * 0.08,
          ease: 'back.out(1.7)',
        })

        gsap.to(item.container.scale, {
          x: 1,
          y: 1,
          z: 1,
          duration: 0.8,
          delay: 0.15 + idx * 0.08,
          ease: 'power3.out',
        })
      })
    },

    animateGerminateClose() {
      // 1. Restaurar orbe a estado original
      gsap.to(this.waterOrb.scale, {
        x: 1,
        y: 1,
        z: 1,
        duration: 0.6,
        ease: 'power2.inOut',
      })

      gsap.to(this.orbMaterial.emissive, {
        r: 0.0,
        g: 0.24,
        b: 0.32,
        duration: 0.6,
      })
      gsap.to(this.orbMaterial, {
        emissiveIntensity: 0.25,
        duration: 0.6,
      })

      // 2. Contraer anillos
      this.rings.forEach((ring) => {
        gsap.to(ring.scale, {
          x: 1,
          y: 1,
          z: 1,
          duration: 0.6,
          ease: 'power2.inOut',
        })
      })

      // 3. Restaurar posición de cámara
      gsap.to(this.camera.position, {
        z: 5.8,
        duration: 0.9,
        ease: 'power2.inOut',
      })

      // 4. Replegar nodos al centro
      this.orbitalMeshes.forEach((item, idx) => {
        gsap.to(item.container.position, {
          x: 0,
          y: 0,
          z: 0,
          duration: 0.5,
          delay: idx * 0.04,
          ease: 'power2.in',
        })

        gsap.to(item.container.scale, {
          x: 0.001,
          y: 0.001,
          z: 0.001,
          duration: 0.4,
          delay: idx * 0.04,
        })
      })

      this.hoveredNode = null
    },

    navigateNode(node) {
      if (node.url) {
        window.open(node.url, '_blank')
      } else if (node.route) {
        this.$router.push(node.route)
      }
    },

    onPillHover(node) {
      this.hoveredNode = node
      const meshObj = this.orbitalMeshes.find((m) => m.config.id === node.id)
      if (meshObj) {
        gsap.to(meshObj.container.scale, {
          x: 1.4,
          y: 1.4,
          z: 1.4,
          duration: 0.3,
        })
      }
    },

    onPillLeave() {
      if (this.hoveredNode) {
        const meshObj = this.orbitalMeshes.find(
          (m) => m.config.id === this.hoveredNode.id
        )
        if (meshObj) {
          gsap.to(meshObj.container.scale, {
            x: 1,
            y: 1,
            z: 1,
            duration: 0.3,
          })
        }
        this.hoveredNode = null
      }
    },

    startLoop() {
      const render = () => {
        this.animFrameId = requestAnimationFrame(render)

        const delta = this.clock.getDelta()
        const elapsedTime = this.clock.getElapsedTime()

        // 1. Animación del modelo GLTF (si existe mixer)
        if (this.mixer) {
          this.mixer.update(delta)
        }

        // 2. Respiración organo-digital del núcleo
        const breath = Math.sin(elapsedTime * 1.6) * 0.04
        if (this.droneModel) {
          this.droneModel.position.y = breath
        }

        // Flotación suave vertical del grupo completo
        if (!this.isDragging) {
          this.coreGroup.position.y = Math.sin(elapsedTime * 1.2) * 0.12
          // Rotación Idle continua
          this.coreGroup.rotation.y += delta * 0.25
        }

        // 3. Rotación diferencial de los anillos de savia lumínica
        this.rings.forEach((ring) => {
          ring.rotation.z += ring.userData.speed
          ring.rotation.x += ring.userData.speed * 0.5
        })

        // 4. Parallax de cámara interactivo con el puntero del mouse
        const parallaxTargetX = this.pointer.x * 0.45
        const parallaxTargetY = this.pointer.y * 0.3
        this.camera.position.x +=
          (parallaxTargetX - this.camera.position.x) * 0.05
        this.camera.position.y +=
          (parallaxTargetY - this.camera.position.y) * 0.05
        this.camera.lookAt(0, 0, 0)

        // 5. Órbita dinámica de los Frutos Digitales en estado germinado
        if (this.isGerminated) {
          const orbitRadius = window.innerWidth < 600 ? 2.5 : 3.4
          const speedMultiplier = 0.28

          this.orbitalMeshes.forEach((item) => {
            item.currentAngle += delta * speedMultiplier
            const angle = item.currentAngle

            // Sólo mover suavemente si no se está arrastrando bruscamente
            const targetX = Math.cos(angle) * orbitRadius
            const targetY =
              Math.sin(angle * 2) * 0.35 +
              Math.sin(elapsedTime + item.config.angle) * 0.1
            const targetZ = Math.sin(angle) * orbitRadius

            item.container.position.x +=
              (targetX - item.container.position.x) * 0.08
            item.container.position.y +=
              (targetY - item.container.position.y) * 0.08
            item.container.position.z +=
              (targetZ - item.container.position.z) * 0.08

            // Auto-rotación del fruto sobre sí mismo
            item.coreMesh.rotation.y += delta * 1.2
            item.nodeRing.rotation.z += delta * 1.5
          })
        }

        // 6. Fluctuación y flotación de las micropartículas de plancton
        if (this.planktonField) {
          this.planktonField.rotation.y = elapsedTime * 0.03
          this.planktonField.rotation.x = Math.sin(elapsedTime * 0.02) * 0.05
        }

        // 7. Luces puntuales orbitando dinámicamente
        if (this.magentaLight && this.cyanLight) {
          this.magentaLight.position.x = Math.sin(elapsedTime * 0.8) * 4.2
          this.magentaLight.position.z = Math.cos(elapsedTime * 0.8) * 3.5
          this.cyanLight.position.x = -Math.sin(elapsedTime * 0.7) * 4.2
          this.cyanLight.position.z = -Math.cos(elapsedTime * 0.7) * 3.5
        }

        // Renderizado
        this.renderer.render(this.scene, this.camera)
      }

      render()
    },

    cleanupThree() {
      if (this.animFrameId) {
        cancelAnimationFrame(this.animFrameId)
      }
      if (this._cleanupListeners) {
        this._cleanupListeners()
      }
      if (this.renderer) {
        this.renderer.dispose()
      }
      if (this.scene) {
        this.scene.clear()
      }
      if (
        this.$refs.threeContainer &&
        this.renderer &&
        this.renderer.domElement
      ) {
        this.$refs.threeContainer.removeChild(this.renderer.domElement)
      }
    },
  },
}
</script>

<style scoped>
.aquatic-scene-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background-color: #02070e;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica,
    Arial, sans-serif;
  user-select: none;
}

.three-viewport {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 2;
  cursor: grab;
}

.three-viewport:active {
  cursor: grabbing;
}

/* Fondo de Cáusticas y Ondas Acuáticas */
.water-caustics-backdrop {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  pointer-events: none;
  background: radial-gradient(
    ellipse at 50% 40%,
    #052033 0%,
    #03121f 45%,
    #01070e 100%
  );
  overflow: hidden;
}

.caustic-layer {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  opacity: 0.12;
  background: radial-gradient(
      circle at 30% 40%,
      rgba(0, 245, 212, 0.45) 0%,
      transparent 35%
    ),
    radial-gradient(
      circle at 70% 60%,
      rgba(255, 0, 127, 0.3) 0%,
      transparent 40%
    );
  filter: blur(40px);
}

.layer-1 {
  animation: waveMotion 18s ease-in-out infinite alternate;
}

.layer-2 {
  opacity: 0.08;
  animation: waveMotion 24s ease-in-out infinite alternate-reverse;
}

.vignette-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(
    circle at center,
    transparent 50%,
    rgba(1, 6, 12, 0.85) 100%
  );
}

@keyframes waveMotion {
  0% {
    transform: scale(1) rotate(0deg);
  }
  50% {
    transform: scale(1.15) rotate(4deg) translate(2%, 2%);
  }
  100% {
    transform: scale(1.05) rotate(-3deg) translate(-2%, -1%);
  }
}

/* Header & HUD */
.brand-hud {
  position: absolute;
  top: 24px;
  left: 28px;
  z-index: 10;
  pointer-events: none;
}

.brand-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(0, 245, 212, 0.08);
  border: 1px solid rgba(0, 245, 212, 0.25);
  border-radius: 9999px;
  padding: 4px 12px;
  margin-bottom: 8px;
  backdrop-filter: blur(8px);
}

.pulse-dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background-color: #00f5d4;
  box-shadow: 0 0 10px #00f5d4;
  animation: pulseGlow 2s infinite ease-in-out;
}

@keyframes pulseGlow {
  0%,
  100% {
    opacity: 0.4;
    transform: scale(0.9);
  }
  50% {
    opacity: 1;
    transform: scale(1.3);
  }
}

.hud-mono {
  font-family: monospace;
  font-size: 0.68rem;
  letter-spacing: 0.12em;
  color: #00f5d4;
  font-weight: 700;
}

.brand-title {
  font-size: clamp(1.8rem, 4vw, 2.8rem);
  font-weight: 900;
  letter-spacing: -0.02em;
  margin: 0;
  line-height: 1.1;
  background: linear-gradient(135deg, #ffffff 30%, #a0f0ed 70%, #ff70ba 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.brand-tagline {
  margin-top: 4px;
  font-size: 0.85rem;
  color: rgba(220, 240, 255, 0.65);
  letter-spacing: 0.03em;
}

/* Prompt Central de Interacción */
.interaction-prompt {
  position: absolute;
  bottom: 96px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
  transition: transform 0.4s ease, opacity 0.4s ease;
}

.germinate-toggle-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(3, 16, 28, 0.75);
  border: 1px solid rgba(0, 245, 212, 0.4);
  color: #e0fbfc;
  padding: 10px 22px;
  border-radius: 9999px;
  font-size: 0.78rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  cursor: pointer;
  backdrop-filter: blur(12px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5), 0 0 20px rgba(0, 245, 212, 0.15);
  transition: all 0.3s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.germinate-toggle-btn:hover {
  border-color: #00f5d4;
  background: rgba(0, 245, 212, 0.15);
  box-shadow: 0 10px 40px rgba(0, 245, 212, 0.3);
  transform: translateY(-2px);
}

/* Tarjeta Holográfica Flotante */
.holographic-preview-card {
  position: fixed;
  z-index: 1000;
  pointer-events: none;
  width: 250px;
  transform: translate(-50%, -100%);
  filter: drop-shadow(0 12px 28px rgba(0, 0, 0, 0.6));
}

.card-inner {
  position: relative;
  background: rgba(4, 18, 30, 0.88);
  border: 1px solid #00f5d4;
  border-radius: 14px;
  padding: 14px 16px;
  backdrop-filter: blur(16px);
  overflow: hidden;
}

.card-glow {
  position: absolute;
  top: -20px;
  right: -20px;
  width: 70px;
  height: 70px;
  border-radius: 50%;
  opacity: 0.35;
  filter: blur(20px);
}

.card-badge {
  font-size: 0.65rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  font-family: monospace;
}

.card-icon {
  font-size: 1.25rem;
}

.card-title {
  color: #ffffff;
  font-size: 1rem;
  font-weight: 800;
  margin: 4px 0 2px 0;
}

.card-desc {
  font-size: 0.74rem;
  color: rgba(220, 235, 245, 0.75);
  line-height: 1.4;
  margin-bottom: 8px;
}

.card-action-hint {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 0.7rem;
  color: #00f5d4;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.holo-fade-enter-active,
.holo-fade-leave-active {
  transition: opacity 0.25s ease, transform 0.25s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.holo-fade-enter {
  opacity: 0;
  transform: translate(-50%, -85%) scale(0.92);
}

.holo-fade-leave-to {
  opacity: 0;
  transform: translate(-50%, -105%) scale(0.95);
}

/* Barra de Navegación Rápida Inferior */
.quick-nav-bar {
  position: absolute;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
  width: 90%;
  max-width: 740px;
}

.quick-nav-pills {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 8px;
  padding: 6px;
  background: rgba(3, 14, 24, 0.65);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 9999px;
  backdrop-filter: blur(14px);
}

.quick-pill-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 14px;
  border-radius: 9999px;
  background: transparent;
  border: 1px solid transparent;
  color: #cbd5e1;
  font-size: 0.75rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.quick-pill-btn:hover,
.quick-pill-btn.active {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(0, 245, 212, 0.3);
  color: #ffffff;
  transform: translateY(-1px);
}

.pill-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
}

@media (max-width: 600px) {
  .brand-hud {
    top: 16px;
    left: 16px;
  }
  .interaction-prompt {
    bottom: 120px;
  }
  .quick-nav-bar {
    bottom: 16px;
  }
  .quick-pill-btn {
    padding: 5px 10px;
    font-size: 0.68rem;
  }
}
</style>
