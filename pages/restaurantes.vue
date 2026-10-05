<template>
  <v-main class="restaurants-page">
    <!-- Hero Banner Section -->
    <section class="hero-section">
      <div class="hero-bg-overlay"></div>
      <v-container class="py-12 py-md-16 position-relative">
        <v-row align="center" justify="center">
          <v-col cols="12" lg="10" class="text-center">
            <div class="d-inline-flex align-center hero-kicker mb-4 px-4 py-1">
              <v-icon small color="amber lighten-2" class="mr-2"
                >mdi-silverware-fork-knife</v-icon
              >
              <span
                class="text-caption font-weight-bold text-uppercase tracking-wider"
                >Soluciones Digitales Para Gastronomía</span
              >
            </div>

            <h1 class="text-h3 text-sm-h2 font-weight-black hero-title mb-4">
              Menús Digitales & Experiencias para Restaurantes
            </h1>

            <p class="text-subtitle-1 text-md-h6 hero-subtitle mx-auto mb-8">
              Digitalizamos la carta de tu restaurante con diseño de autor,
              carga ultra-rápida y pedidos directos a WhatsApp. Sin
              intermediarios, sin comisiones por venta y optimizado al 100% para
              celulares.
            </p>

            <!-- Quick Action CTAs -->
            <div class="d-flex flex-wrap justify-center gap-3 mb-12">
              <v-btn
                color="amber darken-2"
                dark
                large
                elevation="4"
                class="px-6 font-weight-bold cta-primary"
                @click="scrollToSection('showcase')"
              >
                <v-icon left>mdi-compass</v-icon>
                Explorar Restaurantes
              </v-btn>

              <v-btn
                color="white"
                outlined
                large
                class="px-6 font-weight-bold ml-sm-4 mt-3 mt-sm-0"
                @click="openSimulator(featuredRestaurants[0])"
              >
                <v-icon left color="amber">mdi-cellphone-link</v-icon>
                Probar Simulador en Vivo
              </v-btn>

              <v-btn
                text
                large
                class="px-6 font-weight-bold text-white ml-sm-4 mt-3 mt-sm-0"
                @click="scrollToSection('contact')"
              >
                <v-icon left>mdi-chat-outline</v-icon>
                Solicitar Propuesta
              </v-btn>
            </div>

            <!-- Key Metrics Bar -->
            <v-row class="metrics-row justify-center mt-6">
              <v-col cols="6" sm="3" class="metric-item py-4">
                <div class="metric-number text-amber">11+</div>
                <div class="metric-label">Restaurantes Activos</div>
              </v-col>
              <v-col cols="6" sm="3" class="metric-item py-4">
                <div class="metric-number text-green">0%</div>
                <div class="metric-label">Comisiones Por Pedido</div>
              </v-col>
              <v-col cols="6" sm="3" class="metric-item py-4">
                <div class="metric-number text-cyan">&lt; 1.2s</div>
                <div class="metric-label">Velocidad de Carga</div>
              </v-col>
              <v-col cols="6" sm="3" class="metric-item py-4">
                <div class="metric-number text-pink">+42%</div>
                <div class="metric-label">Ticket Promedio</div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <!-- Navigation Breadcrumbs & Flow Header -->
    <v-container class="py-4">
      <div class="d-flex align-center justify-space-between flex-wrap">
        <div class="d-flex align-center text-caption text-grey">
          <nuxt-link to="/" class="text-grey text-decoration-none hover-white"
            >Inicio</nuxt-link
          >
          <span class="mx-2">/</span>
          <nuxt-link
            to="/portfolio"
            class="text-grey text-decoration-none hover-white"
            >Portafolio</nuxt-link
          >
          <span class="mx-2">/</span>
          <span class="white--text font-weight-medium"
            >Restaurantes & Gastronomía</span
          >
        </div>

        <div class="mt-2 mt-sm-0">
          <v-chip
            small
            color="grey darken-3"
            text-color="amber lighten-2"
            class="font-weight-medium"
          >
            <v-icon left x-small>mdi-fire</v-icon>
            Especialidad Destacada Webicultores
          </v-chip>
        </div>
      </div>
    </v-container>

    <!-- Filter & Search Controls Section -->
    <section id="showcase" class="py-6">
      <v-container>
        <div class="filter-wrapper pa-4 rounded-xl mb-8">
          <v-row align="center">
            <v-col cols="12" md="6">
              <v-text-field
                v-model="searchQuery"
                prepend-inner-icon="mdi-magnify"
                label="Buscar por restaurante, tipo de cocina o ciudad..."
                hide-details
                outlined
                dense
                clearable
                dark
                class="rounded-lg"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <div class="d-flex flex-wrap align-center justify-md-end gap-2">
                <span class="text-caption text-grey mr-2 d-none d-sm-inline"
                  >Filtrar por cocina:</span
                >
                <v-btn
                  v-for="cat in categories"
                  :key="cat"
                  x-small
                  depressed
                  :color="
                    selectedCategory === cat
                      ? 'amber darken-2'
                      : 'grey darken-3'
                  "
                  :class="
                    selectedCategory === cat
                      ? 'white--text font-weight-bold'
                      : 'grey--text text--lighten-1'
                  "
                  class="ma-1 text-capitalize px-3 py-1 rounded-pill"
                  @click="selectedCategory = cat"
                >
                  {{ cat }}
                </v-btn>
              </div>
            </v-col>
          </v-row>
        </div>

        <!-- Section Title & Results Count -->
        <div class="d-flex align-center justify-space-between mb-6">
          <div>
            <h2 class="text-h4 font-weight-bold white--text mb-1">
              Portafolio Gastronómico
            </h2>
            <p class="text-body-2 text-grey mb-0">
              Explora los restaurantes desarrollados por Webicultores, sus
              características y sitios en vivo.
            </p>
          </div>
          <div class="text-caption text-grey">
            Mostrando {{ filteredRestaurants.length }} de
            {{ featuredRestaurants.length }} proyectos
          </div>
        </div>

        <!-- Restaurants Cards Grid -->
        <v-row v-if="filteredRestaurants.length > 0">
          <v-col
            v-for="restaurant in filteredRestaurants"
            :key="restaurant.id"
            cols="12"
            md="6"
            lg="4"
            class="d-flex"
          >
            <v-card
              dark
              class="restaurant-card d-flex flex-column rounded-xl flex-grow-1"
              elevation="6"
            >
              <!-- Card Image & Overlay -->
              <div class="card-media-wrapper position-relative">
                <v-img
                  :src="restaurant.image"
                  :alt="restaurant.name"
                  height="230"
                  class="card-img"
                >
                  <template #placeholder>
                    <v-row
                      class="fill-height ma-0"
                      align="center"
                      justify="center"
                    >
                      <v-progress-circular
                        indeterminate
                        color="amber"
                      ></v-progress-circular>
                    </v-row>
                  </template>
                </v-img>

                <!-- Gradient Overlay -->
                <div class="media-gradient"></div>

                <!-- Top Floating Tag -->
                <div class="top-tag-container">
                  <span class="category-pill">
                    {{ restaurant.category }}
                  </span>
                </div>

                <!-- Location Pin -->
                <div class="location-badge">
                  <v-icon x-small color="amber" class="mr-1"
                    >mdi-map-marker</v-icon
                  >
                  <span>{{ restaurant.location }}</span>
                </div>
              </div>

              <!-- Card Body -->
              <v-card-text class="pa-5 flex-grow-1 d-flex flex-column">
                <div class="d-flex align-center justify-space-between mb-2">
                  <h3
                    class="text-h5 font-weight-bold white--text restaurant-title"
                  >
                    {{ restaurant.name }}
                  </h3>
                  <v-tooltip bottom>
                    <template #activator="{ on, attrs }">
                      <v-icon
                        small
                        color="green accent-3"
                        v-bind="attrs"
                        v-on="on"
                      >
                        mdi-check-decagram
                      </v-icon>
                    </template>
                    <span>En producción activa</span>
                  </v-tooltip>
                </div>

                <div
                  class="restaurant-slogan text-caption text-amber lighten-3 font-italic mb-3"
                >
                  "{{ restaurant.slogan }}"
                </div>

                <p class="text-body-2 text-grey lighten-1 mb-4 flex-grow-1">
                  {{ restaurant.shortDescription }}
                </p>

                <!-- Highlights List -->
                <div class="features-list mb-4">
                  <div
                    v-for="(feature, idx) in restaurant.features.slice(0, 3)"
                    :key="idx"
                    class="d-flex align-center mb-1 text-caption text-grey lighten-2"
                  >
                    <v-icon x-small color="amber darken-1" class="mr-2"
                      >mdi-check-circle</v-icon
                    >
                    <span>{{ feature }}</span>
                  </div>
                </div>

                <!-- Tech Badges -->
                <div class="d-flex flex-wrap gap-1 mb-4">
                  <span
                    v-for="(tech, tIdx) in restaurant.technologies"
                    :key="tIdx"
                    class="tech-tag"
                  >
                    {{ tech }}
                  </span>
                </div>

                <v-divider class="my-2 border-grey"></v-divider>

                <!-- Card Actions -->
                <div class="d-flex align-center justify-space-between pt-2">
                  <v-btn
                    small
                    color="amber darken-2"
                    depressed
                    class="font-weight-bold text-caption px-3"
                    @click="openSimulator(restaurant)"
                  >
                    <v-icon left x-small>mdi-cellphone-play</v-icon>
                    Simulador
                  </v-btn>

                  <v-btn
                    small
                    outlined
                    color="grey lighten-1"
                    class="text-caption px-3"
                    @click="openCaseStudy(restaurant)"
                  >
                    <v-icon left x-small>mdi-text-box-search-outline</v-icon>
                    Detalles
                  </v-btn>

                  <v-btn
                    icon
                    small
                    color="white"
                    :href="restaurant.websiteUrl"
                    target="_blank"
                    rel="noopener noreferrer"
                    title="Abrir sitio web oficial"
                  >
                    <v-icon small>mdi-open-in-new</v-icon>
                  </v-btn>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>

        <!-- Empty State -->
        <v-card
          v-else
          dark
          class="pa-12 text-center rounded-xl bg-dark-card my-8"
        >
          <v-icon size="64" color="amber darken-2" class="mb-4"
            >mdi-food-off</v-icon
          >
          <h3 class="text-h5 font-weight-bold white--text mb-2">
            No se encontraron restaurantes
          </h3>
          <p class="text-body-2 text-grey mb-4">
            No hay restaurantes que coincidan con la búsqueda "{{
              searchQuery
            }}" o categoría "{{ selectedCategory }}".
          </p>
          <v-btn color="amber darken-2" dark depressed @click="resetFilters">
            Restablecer Filtros
          </v-btn>
        </v-card>
      </v-container>
    </section>

    <!-- Why Choose Webicultores / Ecosystem Section -->
    <section class="ecosystem-section py-16">
      <v-container>
        <div class="text-center mb-12">
          <div
            class="text-caption text-amber font-weight-bold text-uppercase tracking-wider mb-2"
          >
            La Ventaja Webicultores
          </div>
          <h2 class="text-h4 text-sm-h3 font-weight-bold white--text mb-4">
            ¿Por Qué los Restaurantes Prefieren Nuestros Menús Digitales?
          </h2>
          <p class="text-subtitle-1 text-grey max-w-700 mx-auto">
            Desarrollamos soluciones hechas a la medida de la cocina y el
            cliente. Sin apps pesadas, sin registros engorrosos y directo al
            grano.
          </p>
        </div>

        <v-row>
          <v-col
            v-for="(pillar, pIdx) in pillars"
            :key="pIdx"
            cols="12"
            sm="6"
            lg="3"
          >
            <v-card
              dark
              class="pillar-card pa-6 rounded-xl fill-height d-flex flex-column"
              elevation="4"
            >
              <div class="pillar-icon-box mb-4">
                <v-icon size="32" :color="pillar.color">{{
                  pillar.icon
                }}</v-icon>
              </div>
              <h3 class="text-h6 font-weight-bold white--text mb-2">
                {{ pillar.title }}
              </h3>
              <p class="text-body-2 text-grey lighten-1 mb-0 flex-grow-1">
                {{ pillar.description }}
              </p>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <!-- Consultation & Quick Contact Section -->
    <section id="contact" class="py-16 contact-section">
      <v-container>
        <v-row justify="center">
          <v-col cols="12" md="10" lg="8">
            <v-card
              dark
              class="pa-8 pa-md-10 rounded-2xl contact-card"
              elevation="8"
            >
              <div class="text-center mb-8">
                <v-icon size="48" color="amber darken-2" class="mb-3"
                  >mdi-chef-hat</v-icon
                >
                <h2 class="text-h4 font-weight-bold white--text mb-2">
                  ¿Listo Para Digitalizar Tu Restaurante?
                </h2>
                <p class="text-body-1 text-grey mb-0">
                  Cuéntanos sobre tu negocio y te enviaremos una propuesta con
                  demo personalizada en menos de 24 horas.
                </p>
              </div>

              <v-form
                ref="contactForm"
                v-model="formValid"
                @submit.prevent="submitRestaurantInquiry"
              >
                <v-row>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="form.restaurantName"
                      label="Nombre del Restaurante *"
                      outlined
                      dense
                      required
                      :rules="[(v) => !!v || 'Campo requerido']"
                    ></v-text-field>
                  </v-col>

                  <v-col cols="12" sm="6">
                    <v-select
                      v-model="form.cuisineType"
                      :items="cuisineOptions"
                      label="Tipo de Gastronomía *"
                      outlined
                      dense
                      required
                      :rules="[(v) => !!v || 'Campo requerido']"
                    ></v-select>
                  </v-col>

                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="form.city"
                      label="Ciudad / País *"
                      outlined
                      dense
                      required
                      :rules="[(v) => !!v || 'Campo requerido']"
                    ></v-text-field>
                  </v-col>

                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="form.whatsappNumber"
                      label="Teléfono WhatsApp *"
                      outlined
                      dense
                      required
                      placeholder="+58 412..."
                      :rules="[(v) => !!v || 'Campo requerido']"
                    ></v-text-field>
                  </v-col>

                  <v-col cols="12">
                    <v-textarea
                      v-model="form.message"
                      label="¿Qué necesitas en tu menú digital? (Opcional)"
                      outlined
                      dense
                      rows="3"
                      placeholder="Ej: Tenemos 40 platos, delivery propio y queremos código QR para mesas..."
                    ></v-textarea>
                  </v-col>

                  <v-col cols="12" class="text-center pt-2">
                    <v-btn
                      type="submit"
                      large
                      color="amber darken-2"
                      dark
                      elevation="4"
                      class="px-8 font-weight-bold"
                      :disabled="!formValid"
                    >
                      Enviar
                    </v-btn>
                    <div class="text-caption text-grey mt-3">
                      Respuesta directa con el equipo técnico de Webicultores
                    </div>
                  </v-col>
                </v-row>
              </v-form>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <!-- Interactive Live Device Simulator Modal -->
    <v-dialog
      v-model="simulatorDialog"
      max-width="1100"
      scrollable
      transition="dialog-bottom-transition"
    >
      <v-card
        v-if="activeRestaurant"
        dark
        class="simulator-dialog-card rounded-xl"
      >
        <!-- Dialog Header -->
        <v-card-title
          class="pa-4 grey darken-4 d-flex align-center justify-space-between"
        >
          <div class="d-flex align-center">
            <v-icon color="amber" class="mr-2">mdi-cellphone-link</v-icon>
            <div>
              <div class="text-h6 font-weight-bold white--text leading-tight">
                {{ activeRestaurant.name }}
              </div>
              <div class="text-caption text-grey">
                Simulador Interactivo en Vivo ·
                {{ activeRestaurant.websiteUrl }}
              </div>
            </div>
          </div>

          <!-- Controls in Header -->
          <div class="d-flex align-center gap-2">
            <!-- Viewport Switcher -->
            <v-btn-toggle
              v-model="simulatorDevice"
              mandatory
              dense
              class="mr-3 d-none d-sm-flex"
            >
              <v-btn value="mobile" small title="Vista Móvil (390px)">
                <v-icon small>mdi-cellphone</v-icon>
              </v-btn>
              <v-btn value="tablet" small title="Vista Tablet (768px)">
                <v-icon small>mdi-tablet</v-icon>
              </v-btn>
              <v-btn value="desktop" small title="Vista Completa">
                <v-icon small>mdi-monitor</v-icon>
              </v-btn>
            </v-btn-toggle>

            <!-- Reload Button -->
            <v-btn icon small title="Recargar iframe" @click="reloadIframe">
              <v-icon small>mdi-refresh</v-icon>
            </v-btn>

            <!-- Open in Netlify External Tab -->
            <v-btn
              icon
              small
              :href="activeRestaurant.websiteUrl"
              target="_blank"
              rel="noopener noreferrer"
              title="Abrir en pestaña nueva"
            >
              <v-icon small>mdi-open-in-new</v-icon>
            </v-btn>

            <!-- Close Dialog -->
            <v-btn icon small @click="simulatorDialog = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </div>
        </v-card-title>

        <v-divider></v-divider>

        <!-- Dialog Content: Device Stage -->
        <v-card-text
          class="pa-4 pa-md-6 simulator-stage d-flex flex-column align-center justify-center"
        >
          <!-- Restaurant Switcher Chips -->
          <div
            class="restaurant-switch-bar mb-4 d-flex flex-wrap justify-center gap-1"
          >
            <v-chip
              v-for="r in featuredRestaurants"
              :key="r.id"
              small
              filter
              :color="
                activeRestaurant.id === r.id
                  ? 'amber darken-2'
                  : 'grey darken-3'
              "
              :class="
                activeRestaurant.id === r.id ? 'white--text' : 'grey--text'
              "
              class="ma-1 font-weight-medium cursor-pointer"
              @click="switchSimulatorRestaurant(r)"
            >
              {{ r.name }}
            </v-chip>
          </div>

          <!-- Device Container Frame -->
          <div
            class="device-frame-container"
            :class="['device-' + simulatorDevice]"
          >
            <!-- Mobile Notch / Header bar -->
            <div v-if="simulatorDevice === 'mobile'" class="device-notch">
              <span class="notch-speaker"></span>
              <span class="notch-camera"></span>
            </div>

            <!-- The Live Web Iframe -->
            <iframe
              ref="simulatorIframe"
              :src="activeRestaurant.websiteUrl"
              class="simulator-iframe"
              title="Simulador de restaurante"
              loading="lazy"
            ></iframe>
          </div>

          <div class="mt-4 text-center">
            <span class="text-caption text-grey">
              ¿No carga el menú interactivo dentro del simulador?
              <a
                :href="activeRestaurant.websiteUrl"
                target="_blank"
                rel="noopener noreferrer"
                class="amber--text text-decoration-none font-weight-bold"
              >
                Haz clic aquí para abrirlo directamente en Netlify
              </a>
            </span>
          </div>
        </v-card-text>
      </v-card>
    </v-dialog>

    <!-- Case Study Details Modal -->
    <v-dialog v-model="caseStudyDialog" max-width="700">
      <v-card
        v-if="selectedCaseStudy"
        dark
        class="rounded-xl pa-6 bg-dark-card"
      >
        <div class="d-flex align-start justify-space-between mb-4">
          <div>
            <div
              class="text-caption text-amber font-weight-bold text-uppercase"
            >
              Ficha Técnica & Caso de Éxito
            </div>
            <h3 class="text-h4 font-weight-bold white--text">
              {{ selectedCaseStudy.name }}
            </h3>
            <div class="text-caption text-grey">
              {{ selectedCaseStudy.location }} ·
              {{ selectedCaseStudy.category }}
            </div>
          </div>
          <v-btn icon small @click="caseStudyDialog = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </div>

        <v-img
          :src="selectedCaseStudy.image"
          height="200"
          class="rounded-lg mb-4"
        ></v-img>

        <div class="mb-4">
          <h4 class="text-subtitle-1 font-weight-bold text-amber mb-1">
            El Concepto & Reto
          </h4>
          <p class="text-body-2 text-grey lighten-1">
            {{ selectedCaseStudy.story }}
          </p>
        </div>

        <div class="mb-4">
          <h4 class="text-subtitle-1 font-weight-bold text-amber mb-1">
            La Solución Desarrollada por Webicultores
          </h4>
          <ul class="text-body-2 text-grey lighten-1 pl-4">
            <li
              v-for="(feat, fIdx) in selectedCaseStudy.features"
              :key="fIdx"
              class="mb-1"
            >
              {{ feat }}
            </li>
          </ul>
        </div>

        <div class="mb-6">
          <h4 class="text-subtitle-1 font-weight-bold text-amber mb-2">
            Tecnologías Empleadas
          </h4>
          <div class="d-flex flex-wrap gap-2">
            <v-chip
              v-for="(tech, tIdx) in selectedCaseStudy.technologies"
              :key="tIdx"
              small
              color="grey darken-3"
              text-color="white"
            >
              {{ tech }}
            </v-chip>
          </div>
        </div>

        <div class="d-flex justify-end gap-2 pt-2">
          <v-btn
            color="amber darken-2"
            dark
            depressed
            class="font-weight-bold"
            @click="openSimulatorFromDetails"
          >
            <v-icon left small>mdi-cellphone-play</v-icon>
            Probar en Simulador
          </v-btn>

          <v-btn
            outlined
            color="white"
            :href="selectedCaseStudy.websiteUrl"
            target="_blank"
            rel="noopener noreferrer"
            class="ml-2 font-weight-bold"
          >
            <v-icon left small>mdi-open-in-new</v-icon>
            Visitar Sitio Web
          </v-btn>
        </div>
      </v-card>
    </v-dialog>
  </v-main>
</template>

<script>
export default {
  name: 'RestaurantesPage',
  layout: 'project',
  data() {
    return {
      searchQuery: '',
      selectedCategory: 'Todos',
      categories: [
        'Todos',
        'Parrillas & Cortes',
        'Italiana & Mariscos',
        'Pizzas & Street Food',
        'Gastrobar & Coctelería',
        'Burgers & Casual',
        'Cafetería & Panadería',
        'Asiática & Wok',
      ],

      // Dialog states
      simulatorDialog: false,
      simulatorDevice: 'mobile',
      activeRestaurant: null,

      caseStudyDialog: false,
      selectedCaseStudy: null,

      // Form state
      formValid: true,
      form: {
        restaurantName: '',
        cuisineType: '',
        city: '',
        whatsappNumber: '',
        message: '',
      },
      cuisineOptions: [
        'Parrilla & Carnes',
        'Comida Italiana / Pastas',
        'Pizzería & Comida Rápida',
        'Gastrobar / Coctelería / Tapas',
        'Hamburguesería / Pepitos',
        'Cafetería / Brunch / Repostería',
        'Comida Asiática / Sushi',
        'Otro tipo de restaurante',
      ],

      // Value pillars
      pillars: [
        {
          icon: 'mdi-lightning-bolt',
          color: 'amber',
          title: 'Carga Instantánea (< 1.2s)',
          description:
            'Menús hiper-optimizados que cargan en un parpadeo en cualquier teléfono con conexión 4G o WiFi sin consumir datos.',
        },
        {
          icon: 'mdi-cash-remove',
          color: 'green accent-3',
          title: '0% Comisiones por Venta',
          description:
            'Olvídate de entregar el 20% al 25% de cada plato a intermediarios. Todo el margen y el ticket completo va a tu bolsillo.',
        },
        {
          icon: 'mdi-whatsapp',
          color: 'light-blue accent-3',
          title: 'Comandas Listas a WhatsApp',
          description:
            'El pedido llega organizado con nombre del cliente, dirección, método de pago, platos y notas de preparación listo para cocina.',
        },
        {
          icon: 'mdi-qrcode-scan',
          color: 'pink accent-2',
          title: 'QR en Mesa & Delivery',
          description:
            'Una sola herramienta sirve para comensales en salón escaneando el QR, pick-up en tienda y delivery a domicilio.',
        },
      ],

      // Real 5 restaurants portfolio
      featuredRestaurants: [
        {
          id: 'mantel-rojo',
          name: 'Mantel Rojo: Parrillas & Más',
          slogan: 'Cortes al rojo vivo & Parrillas premium',
          category: 'Parrillas & Cortes',
          location: 'Lechería / Anzoátegui',
          websiteUrl: 'https://mantelrojo.netlify.app/',
          image: '/images/restaurants/mantel_rojo_grill_1791063663812.jpg',
          shortDescription:
            'Asistente virtual y menú digital de alta gama para cortes a la parrilla, carrusel de llamas y personalización de términos de cocción.',
          story:
            'Mantel Rojo necesitaba una herramienta visual atractiva para que sus clientes pudieran seleccionar el término de sus cortes (término medio, 3/4, bien cocido), armar sus parrillas familiares y enviar el pedido listo a WhatsApp sin fricción.',
          features: [
            'Carrusel de Llamas interactivo para cortes premium',
            'Selector de término de cocción y guarniciones personalizadas',
            'Comanda automática estructurada a WhatsApp',
            'Diseño oscuro elegante con contrastes de brasas ardientes',
          ],
          technologies: [
            'Vue.js',
            'Tailwind CSS',
            'Netlify Edge',
            'WhatsApp API',
          ],
        },
        {
          id: 'saporito',
          name: 'Saporito — Pastas y Mariscos',
          slogan: 'Cocina italiana & frutos del mar con estilo bohemio',
          category: 'Italiana & Mariscos',
          location: 'Anzoátegui (By Il Pomodoro)',
          websiteUrl: 'https://saporito.netlify.app/',
          image: '/images/restaurants/saporito_pasta_seafood_1791063673167.jpg',
          shortDescription:
            'Menú digital bohemio y fresco enfocado en la mejor tradición culinaria italiana de pastas artesanales y frutos del mar frescos.',
          story:
            'El restaurante Saporito (creado por Il Pomodoro) requería una carta digital que transmitiera la frescura de sus mariscos y la calidez de sus pastas artesanales, facilitando el pedido a domicilio con fotos en alta definición.',
          features: [
            'Carta sensorial con fotografía de alta fidelidad',
            'Sección dedicada a pastichos tradicionales y cazuelas de mariscos',
            'Cálculo automático de cuenta y adiciones en tiempo real',
            'Diseño mediterráneo moderno con paleta salina y cálida',
          ],
          technologies: ['React', 'Vite', 'Tailwind CSS', 'WhatsApp Business'],
        },
        {
          id: 'doro-restaurant',
          name: "D'Oro Restaurant",
          slogan: 'Pizzas 33x33cm & Auténtico Sabor Maracucho',
          category: 'Pizzas & Street Food',
          location: 'Alta Vista, Puerto Ordaz',
          websiteUrl: 'https://dororest.netlify.app/',
          image: '/images/restaurants/doro_artisan_pizza_1791063682637.jpg',
          shortDescription:
            'Auténtico sabor maracucho, pizzas cuadradas gigantes de 33x33cm, smash burgers jugosas, patacones y coctelería de autor.',
          story:
            "D'Oro Restaurant en Puerto Ordaz destaca por sus pizzas de formato cuadrado artesanal de 33cm y su street food gourmet zuliano. Desarrollamos un menú interactivo rápido y vibrante que refleja la energía del local.",
          features: [
            'Configurador de pizzas cuadradas 33x33cm con bordes rellenos',
            'Catálogo interactivo de smash burgers y patacones maracuchos',
            'Sección de coctelería artesanal y promociones del día',
            'Tipografía de autor Cabinet Grotesk y fluidez en móviles',
          ],
          technologies: ['Vue.js', 'Vite', 'Cabinet Grotesk', 'Netlify Edge'],
        },
        {
          id: '11-11-foodie-bar',
          name: '11:11 Foodie Bar',
          slogan: 'Pide un deseo — Experiencia gastronómica y coctelería',
          category: 'Gastrobar & Coctelería',
          location: 'Av. Principal de Lechería',
          websiteUrl: 'https://11foodiebar11.netlify.app/',
          image:
            '/images/restaurants/restaurant_hero_culinary_1791063653662.jpg',
          shortDescription:
            'Menú digital interactivo para gastrobar cosmopolita. Brunch, tapas gourmet, platos de autor y coctelería experimental.',
          story:
            '11:11 Foodie Bar necesitaba un menú ágil que se adaptara tanto al ritmo de los desayunos y brunchs matutinos como a la atmósfera nocturna de cocteles y cenas en Lechería, con reserva de mesas y pedidos.',
          features: [
            'Carta interactiva con turnos dinámicos y coctelería experimental',
            'Módulo de reservas directas y consultas de mesa',
            'Diseño minimalista premium con micro-animaciones fluidas',
            'Acceso rápido vía código QR colocado en cada mesa',
          ],
          technologies: ['React', 'Vite', 'Tailwind CSS', 'WhatsApp Direct'],
        },
        {
          id: 'garage-chilling',
          name: 'Garage Chilling',
          slogan: 'El sabor más chilling de la ciudad',
          category: 'Burgers & Casual',
          location: 'El Tigre / Anzoátegui',
          websiteUrl: 'https://garagechilling.netlify.app/',
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1777319241/garage/bg_mltzlk.png',
          shortDescription:
            'La experiencia casual y urbana para disfrutar de hamburguesas premium, pepitos monumentales, almuerzos y bebidas heladas.',
          story:
            'Garage Chilling buscaba simplificar el proceso de pedido para sus comensales jóvenes y familias, permitiendo personalizar salsas, extras y bebidas en un entorno gráfico relajado ("chilling") y veloz.',
          features: [
            'Selector de hamburguesas clásicas, ocean, chicken y especial',
            'Personalización de pepitos de lomito y pollo',
            'Video background y estética visual de garaje urbano',
            'Envío de pedido estructurado para delivery o consumo en local',
          ],
          technologies: ['React', 'Cloudinary Video', 'Vite', 'Tailwind CSS'],
        },
        {
          id: 'mola-restaurant',
          name: 'Mola Restaurant',
          slogan: 'Gastrobar, Tapas & Coctelería Mediterránea',
          category: 'Gastrobar & Coctelería',
          location: 'Lechería / Anzoátegui',
          websiteUrl: 'https://molarest.netlify.app/menu',
          image: '/images/restaurants/mola_tapas_gastrobar_1791163566120.jpg',
          shortDescription:
            'Experiencia gastronómica moderna con carta digital de tapas de autor, cocina mediterránea, vinos y coctelería.',
          story:
            'Mola Restaurant requería una carta digital sofisticada con categorías fluidas que permitiese a sus comensales consultar tapas, entradas, platos principales y cocteles desde su smartphone con máxima velocidad.',
          features: [
            'Menú interactivo categorizado por tapas, platos y coctelería',
            'Diseño estético mediterráneo con contrastes cálidos',
            'Optimización total para visualización rápida mediante QR',
            'Acceso directo a pedidos y reservas',
          ],
          technologies: ['Vue.js', 'Vuetify', 'Netlify Edge', 'Responsive UI'],
        },
        {
          id: 'rey-cochino',
          name: 'Rey Cochino',
          slogan: 'Comida Rápida, Chino Frito & Auténtico Sabor Venezolano',
          category: 'Pizzas & Street Food',
          location: 'Anzoátegui',
          websiteUrl: 'https://rey-cochino.netlify.app/',
          image: '/images/restaurants/rey_cochino_frito_1791163576435.jpg',
          shortDescription:
            'El rey del cochino frito, chicharrón crujiente, arroz chino especial y combos de comida rápida venezolana.',
          story:
            'Rey Cochino requería un menú dinámico de alta conversión para delivery que facilitara a los clientes armar sus combos de cochino frito con tostones, ensaladas y arroz especial sin demoras.',
          features: [
            'Catálogo visual de raciones de cochino frito y combos familiares',
            'Selector de acompañantes, salsas y extras',
            'Botón de pedido directo a WhatsApp con comanda clara',
            'Carga ultrarrápida adaptada a conexiones móviles',
          ],
          technologies: [
            'Vue.js',
            'Tailwind CSS',
            'WhatsApp Ordering',
            'Netlify',
          ],
        },
        {
          id: 'mc-racuchos',
          name: 'Mc Racuchos',
          slogan: 'Comida Rápida Maracucha & Street Food Zuliano',
          category: 'Burgers & Casual',
          location: 'Venezuela',
          websiteUrl: 'https://mc-racuchos.netlify.app/',
          image: '/images/restaurants/mc_racuchos_food_1791163588956.jpg',
          shortDescription:
            'Patacones gigantes, hamburguesas monumentales, pepitos, tumbarranchos y las mejores salsas zulianas a domicilio.',
          story:
            'Mc Racuchos llevó la auténtica sazón maracucha al entorno digital con un menú visualmente apetitoso y enfocado en maximizar los pedidos directos por delivery a WhatsApp.',
          features: [
            'Personalización completa de patacones (plátano verde o maduro)',
            'Módulo de combos de comida rápida maracucha y tequeños',
            'Cálculo de cuenta y envío de comanda con un solo tap',
            'Diseño vibrante urbano con alta tasa de conversión',
          ],
          technologies: ['React', 'Vite', 'Tailwind CSS', 'WhatsApp Business'],
        },
        {
          id: 'mila-cafe',
          name: 'Mila Café',
          slogan: 'Cafetería de Especialidad, Brunch & Repostería de Autor',
          category: 'Cafetería & Panadería',
          location: 'Lechería / Anzoátegui',
          websiteUrl: 'https://milacafe.netlify.app/',
          image: '/images/restaurants/mila_cafe_brunch_1791163607522.jpg',
          shortDescription:
            'El punto de encuentro para amantes del café de especialidad, desayunos gourmet, brunchs y pastelería fina.',
          story:
            'Mila Café buscaba reflejar su ambiente cálido y minimalista en una carta digital limpia, facilitando a los clientes explorar las notas de café, la bollería recién horneada y los platos salados.',
          features: [
            'Carta digital de bebidas frías, calientes y café de especialidad',
            'Sección de brunch, tostadas gourmet y repostería artesanal',
            'Diseño estético y delicado en tonos neutros y pastel',
            'Escaneo rápido en mesa mediante código QR en acrílico',
          ],
          technologies: ['Vue.js', 'Vuetify', 'Netlify Edge', 'Mobile First'],
        },
        {
          id: 'pan-arte',
          name: 'PanArte',
          slogan: 'Panadería Artesanal, Pastelería & Desayunos',
          category: 'Cafetería & Panadería',
          location: 'Venezuela',
          websiteUrl: 'https://pan-arte.netlify.app/',
          image: '/images/restaurants/pan_arte_panaderia_1791163618975.jpg',
          shortDescription:
            'Panes de masa madre, hojaldres crujientes, cachitos tradicionales, pastelería y desayunos recién salidos del horno.',
          story:
            'PanArte combinó la tradición panadera europea con el calor venezolano en un menú digital interactivo para encargos anticipados de panes, bandejas de pasapalos y desayunos diarios.',
          features: [
            'Catálogo de panes de corteza, hojaldres y bollería artesanal',
            'Sección de desayunos, jugos naturales y combos matutinos',
            'Pedidos rápidos para retiro en tienda (pick-up) o delivery',
            'Fotografía culinaria apetecible y estructura intuitiva',
          ],
          technologies: ['Vue.js', 'Tailwind CSS', 'WhatsApp API', 'Netlify'],
        },
        {
          id: 'arroz-frito-anaco',
          name: 'Arroz Frito Anaco',
          slogan: 'El Auténtico Arroz Frito y Gastronomía Asiática en Anaco',
          category: 'Asiática & Wok',
          location: 'Anaco / Anzoátegui',
          websiteUrl: 'https://arrozfritoanaco.netlify.app/',
          image: '/images/restaurants/arroz_frito_wok_1791163631889.jpg',
          shortDescription:
            'Especialistas en arroz chino frito al wok, lumpias crujientes, pollo agridulce y combos de comida cantonesa.',
          story:
            'Arroz Frito Anaco digitalizó su servicio de despacho y pedidos para la ciudad de Anaco, eliminando errores en comandas telefónicas mediante un sistema interactivo de selección de raciones.',
          features: [
            'Selector de porciones individuales, medianas y familiares de arroz frito',
            'Adición de extras: lumpias, costillitas sal y pimienta y salsas',
            'Direccionamiento automático de pedidos con datos de entrega',
            'Funcionamiento ágil incluso en zonas con conexión móvil limitada',
          ],
          technologies: [
            'Vue.js',
            'Vuetify',
            'WhatsApp Orders',
            'Netlify Edge',
          ],
        },
      ],
    }
  },
  head() {
    return {
      title: 'Menús Digitales para Restaurantes | Webicultores',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content:
            "Portafolio especializado en desarrollo de menús digitales y aplicaciones gastronómicas para restaurantes. Diseños para Mantel Rojo, Saporito, D'Oro, 11:11 Foodie Bar, Garage Chilling, Mola, Rey Cochino, Mc Racuchos, Mila Café, PanArte y Arroz Frito Anaco.",
        },
        {
          property: 'og:title',
          content: 'Menús Digitales para Restaurantes | Webicultores',
        },
        {
          property: 'og:description',
          content:
            'Descubre cómo Webicultores transforma restaurantes con menús interactivos, pedidos por WhatsApp sin comisiones y experiencias culinarias digitales.',
        },
      ],
    }
  },
  computed: {
    filteredRestaurants() {
      return this.featuredRestaurants.filter((rest) => {
        const query = this.searchQuery ? this.searchQuery.toLowerCase() : ''
        const matchesQuery =
          !query ||
          rest.name.toLowerCase().includes(query) ||
          rest.category.toLowerCase().includes(query) ||
          rest.location.toLowerCase().includes(query) ||
          rest.shortDescription.toLowerCase().includes(query)

        const matchesCategory =
          this.selectedCategory === 'Todos' ||
          rest.category === this.selectedCategory

        return matchesQuery && matchesCategory
      })
    },
  },
  methods: {
    scrollToSection(sectionId) {
      const el = document.getElementById(sectionId)
      if (el) {
        el.scrollIntoView({ behavior: 'smooth' })
      }
    },

    resetFilters() {
      this.searchQuery = ''
      this.selectedCategory = 'Todos'
    },

    openSimulator(restaurant) {
      this.activeRestaurant = restaurant
      this.simulatorDialog = true
    },

    switchSimulatorRestaurant(restaurant) {
      this.activeRestaurant = restaurant
    },

    reloadIframe() {
      if (this.$refs.simulatorIframe) {
        const currentSrc = this.$refs.simulatorIframe.src
        this.$refs.simulatorIframe.src = ''
        this.$nextTick(() => {
          this.$refs.simulatorIframe.src = currentSrc
        })
      }
    },

    openCaseStudy(restaurant) {
      this.selectedCaseStudy = restaurant
      this.caseStudyDialog = true
    },

    openSimulatorFromDetails() {
      this.caseStudyDialog = false
      if (this.selectedCaseStudy) {
        this.openSimulator(this.selectedCaseStudy)
      }
    },

    submitRestaurantInquiry() {
      const text = `¡Hola Webicultores! Me interesa digitalizar mi restaurante.%0A%0A*Restaurante:* ${encodeURIComponent(
        this.form.restaurantName
      )}%0A*Tipo de Cocina:* ${encodeURIComponent(
        this.form.cuisineType
      )}%0A*Ciudad:* ${encodeURIComponent(
        this.form.city
      )}%0A*Teléfono:* ${encodeURIComponent(
        this.form.whatsappNumber
      )}%0A*Detalles:* ${encodeURIComponent(
        this.form.message || 'Sin mensaje adicional'
      )}`

      // Open WhatsApp with Webicultores team contact
      window.open(`https://wa.me/584128352365?text=${text}`, '_blank')
    },
  },
}
</script>

<style scoped>
.restaurants-page {
  background-color: #0b0c10;
  color: #f5f5f7;
  min-height: 100vh;
}

/* Hero Section */
.hero-section {
  position: relative;
  background: radial-gradient(circle at 50% 20%, #1f1b18 0%, #0b0c10 75%);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  overflow: hidden;
}

.hero-bg-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('/images/restaurants/restaurant_hero_culinary_1791063653662.jpg');
  background-size: cover;
  background-position: center;
  opacity: 0.12;
  filter: blur(2px);
  pointer-events: none;
}

.hero-kicker {
  background: rgba(255, 179, 0, 0.12);
  border: 1px solid rgba(255, 179, 0, 0.3);
  border-radius: 9999px;
  color: #ffb300;
}

.hero-title {
  color: #ffffff;
  letter-spacing: -0.02em;
  line-height: 1.15;
}

.hero-subtitle {
  color: #c4c4cc;
  max-width: 780px;
  line-height: 1.6;
}

.metrics-row {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 16px;
  backdrop-filter: blur(12px);
}

.metric-item {
  border-right: 1px solid rgba(255, 255, 255, 0.06);
}

.metric-item:last-child {
  border-right: none;
}

.metric-number {
  font-size: 2rem;
  font-weight: 900;
  line-height: 1;
  margin-bottom: 4px;
}

.metric-label {
  font-size: 0.8rem;
  color: #a0a0ab;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

/* Filter controls */
.filter-wrapper {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
}

/* Restaurant Cards */
.restaurant-card {
  background: #14161d;
  border: 1px solid rgba(255, 255, 255, 0.08);
  transition: transform 0.25s ease, box-shadow 0.25s ease,
    border-color 0.25s ease;
  overflow: hidden;
}

.restaurant-card:hover {
  transform: translateY(-4px);
  border-color: rgba(255, 179, 0, 0.35);
  box-shadow: 0 16px 32px rgba(0, 0, 0, 0.45);
}

.card-media-wrapper {
  position: relative;
  overflow: hidden;
}

.card-img {
  transition: transform 0.4s ease;
}

.restaurant-card:hover .card-img {
  transform: scale(1.04);
}

.media-gradient {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60%;
  background: linear-gradient(to top, rgba(20, 22, 29, 0.95), transparent);
  pointer-events: none;
}

.top-tag-container {
  position: absolute;
  top: 12px;
  left: 12px;
  z-index: 2;
}

.category-pill {
  background: rgba(15, 15, 20, 0.85);
  color: #ffc107;
  font-size: 0.75rem;
  font-weight: 700;
  padding: 4px 10px;
  border-radius: 6px;
  border: 1px solid rgba(255, 193, 7, 0.3);
  backdrop-filter: blur(8px);
}

.location-badge {
  position: absolute;
  bottom: 10px;
  left: 16px;
  z-index: 2;
  font-size: 0.8rem;
  color: #e4e4e7;
  display: flex;
  align-items: center;
}

.restaurant-title {
  font-size: 1.35rem;
  line-height: 1.25;
}

.tech-tag {
  background: rgba(255, 255, 255, 0.06);
  color: #a1a1aa;
  font-size: 0.7rem;
  padding: 2px 8px;
  border-radius: 4px;
}

.border-grey {
  border-color: rgba(255, 255, 255, 0.08) !important;
}

/* Ecosystem Pillars */
.ecosystem-section {
  background: #0f1015;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.pillar-card {
  background: #151720;
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: transform 0.2s ease, border-color 0.2s ease;
}

.pillar-card:hover {
  transform: translateY(-3px);
  border-color: rgba(255, 179, 0, 0.3);
}

.pillar-icon-box {
  width: 52px;
  height: 52px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.04);
  border-radius: 12px;
}

/* Contact Section */
.contact-card {
  background: #141720;
  border: 1px solid rgba(255, 255, 255, 0.08);
}

/* Simulator Modal Styles */
.simulator-dialog-card {
  background: #0f1118;
}

.simulator-stage {
  background: #090a0f;
  min-height: 580px;
}

.device-frame-container {
  background: #000;
  border: 8px solid #27272a;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7);
  overflow: hidden;
  transition: width 0.3s ease;
  position: relative;
}

.device-mobile {
  width: 380px;
  max-width: 95vw;
  height: 640px;
  border-radius: 36px;
  border-width: 10px;
}

.device-tablet {
  width: 720px;
  max-width: 95vw;
  height: 640px;
  border-radius: 24px;
}

.device-desktop {
  width: 100%;
  max-width: 1000px;
  height: 600px;
  border-radius: 12px;
}

.device-notch {
  width: 120px;
  height: 18px;
  background: #27272a;
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.notch-speaker {
  width: 35px;
  height: 3px;
  background: #3f3f46;
  border-radius: 3px;
}

.notch-camera {
  width: 6px;
  height: 6px;
  background: #18181b;
  border-radius: 50%;
}

.simulator-iframe {
  width: 100%;
  height: 100%;
  border: none;
  background: #121212;
}

/* Helpers */
.max-w-700 {
  max-width: 700px;
}

.bg-dark-card {
  background: #141620 !important;
}

.hover-white:hover {
  color: #fff !important;
}
</style>
