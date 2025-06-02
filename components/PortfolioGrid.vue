<template>
  <v-container class="portfolio-container pa-4 pa-md-8">
    <!-- Header section with parallax effect -->
    <v-parallax
      src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040942/webicultores/ban_zejeog.png"
      height="300"
      class="rounded-xl mb-8 contain"
    >
      <div
        class="d-flex flex-column fill-height justify-center align-center text-white"
      >
        <h1 class="text-h2 font-weight-bold mb-2 text-shadow">
          Portafolio Web
        </h1>
        <div class="text-subtitle-1 mb-4 text-center px-4 text-shadow">
          Una colección de mis proyectos de aplicaciones web y sitios web
        </div>
        <v-chip-group>
          <v-chip
            v-for="(category, i) in categories"
            :key="i"
            :color="getCategoryColor(i)"
            filter
            variant="elevated"
            class="ma-1"
            @click="filterByCategory(category)"
          >
            {{ category }}
          </v-chip>
        </v-chip-group>
      </div>
    </v-parallax>

    <!-- Filter and sort controls -->
    <!-- <v-card flat class="mb-8 pa-4 rounded-xl bg-surface-variant">
      <v-row align="center">
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="search"
            prepend-inner-icon="mdi-magnify"
            label="Buscar proyectos"
            hide-details
            variant="outlined"
            density="compact"
            bg-color="surface"
            class="rounded-pill"
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6" class="d-flex justify-end">
          <v-btn-toggle
            v-model="viewMode"
            color="primary"
            density="comfortable"
            rounded="pill"
          >
            <v-btn value="grid" prepend-icon="mdi-view-grid">
              Cuadrícula
            </v-btn>
            <v-btn value="list" prepend-icon="mdi-view-list"> Lista </v-btn>
          </v-btn-toggle>
        </v-col>
      </v-row>
    </v-card> -->

    <!-- Projects grid or list -->
    <v-fade-transition mode="out-in">
      <v-row v-if="viewMode === 'grid'">
        <v-col
          v-for="(project, index) in filteredProjects"
          :key="index"
          cols="12"
          sm="4"
          lg="4"
        >
          <ProjectCard :project="project" />
        </v-col>
      </v-row>
      <v-list v-else class="bg-transparent">
        <v-list-item
          v-for="(project, index) in filteredProjects"
          :key="index"
          class="mb-4 rounded-lg"
        >
          <ProjectCard :project="project" />
        </v-list-item>
      </v-list>
    </v-fade-transition>

    <!-- Empty state -->
    <v-card v-if="filteredProjects.length === 0" class="pa-8 text-center" flat>
      <v-icon
        icon="mdi-file-search-outline"
        size="64"
        class="mb-4 text-medium-emphasis"
      ></v-icon>
      <h3 class="text-h5 mb-2">No se encontraron proyectos</h3>
      <p class="text-body-1 text-medium-emphasis">
        Intenta con otra búsqueda o categoría
      </p>
      <v-btn
        color="primary"
        class="mt-4"
        prepend-icon="mdi-refresh"
        @click="resetFilters"
      >
        Reiniciar filtros
      </v-btn>
    </v-card>
  </v-container>
</template>

<script>
import ProjectCard from './ProjectCard.vue'

export default {
  name: 'PortfolioGrid',
  components: {
    ProjectCard,
  },
  props: {
    projects: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      search: '',
      viewMode: 'grid',
      selectedCategory: null,
      colors: ['primary', 'secondary', 'info', 'success', 'warning', 'error'],
    }
  },
  computed: {
    // Extraer todas las categorías únicas de las tecnologías de los proyectos
    categories() {
      const allTechnologies = this.projects.flatMap(
        (project) => project.technologies || []
      )
      const uniqueCategories = [...new Set(allTechnologies)]
      return ['Todos', ...uniqueCategories]
    },

    // Filtrar proyectos según la búsqueda y categoría seleccionada
    filteredProjects() {
      return this.projects.filter((project) => {
        // Filtrar por texto de búsqueda
        const searchMatch =
          this.search === '' ||
          project.name.toLowerCase().includes(this.search.toLowerCase()) ||
          (project.description &&
            project.description
              .toLowerCase()
              .includes(this.search.toLowerCase()))

        // Filtrar por categoría seleccionada
        const categoryMatch =
          !this.selectedCategory ||
          this.selectedCategory === 'Todos' ||
          (project.technologies &&
            project.technologies.includes(this.selectedCategory))

        return searchMatch && categoryMatch
      })
    },
  },
  methods: {
    // Obtener color para cada categoría
    getCategoryColor(index) {
      return this.colors[index % this.colors.length]
    },

    // Filtrar por categoría
    filterByCategory(category) {
      this.selectedCategory = category === 'Todos' ? null : category
    },

    // Reiniciar todos los filtros
    resetFilters() {
      this.search = ''
      this.selectedCategory = null
    },
  },
}
</script>

<style scoped>
.portfolio-container {
  min-height: 100vh;
}

.text-shadow {
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5);
}

/* Animación para los chips de categoría */
.v-chip {
  transition: all 0.3s ease;
}

.v-chip:hover {
  transform: translateY(-3px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

/* Estilo para la sección de búsqueda */
.v-card.bg-surface-variant {
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

/* Animación para las tarjetas de proyectos */
.v-col,
.v-list-item {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.v-col:hover,
.v-list-item:hover {
  z-index: 1;
}
</style>
