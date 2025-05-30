<template>
  <v-container class="branding-container pa-4 pa-md-8">
    <!-- Header section with parallax effect -->
    <v-parallax
      src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040942/webicultores/ban_zejeog.png"
      height="300"
      class="rounded-xl mb-8"
    >
      <div
        class="d-flex flex-column fill-height justify-center align-center text-white"
      >
        <h1 class="text-h2 font-weight-bold mb-2 text-shadow">
          Diseño de Marca
        </h1>
        <div class="text-subtitle-1 mb-4 text-center px-4 text-shadow">
          Creación de identidades visuales únicas y memorables
        </div>
      </div>
    </v-parallax>

    <!-- Filter and search -->
    <v-row class="mb-6">
      <v-col cols="12" md="8">
        <!-- Mobile slider for categories -->
        <v-slide-group
          v-model="selectedCategory"
          show-arrows
          class="d-md-none mb-4"
          active-class="primary"
        >
          <v-slide-item
            v-for="category in categories"
            :key="category"
            v-slot="{ active, toggle }"
            :value="category"
          >
            <v-chip
              class="ma-2"
              :color="active ? 'primary' : 'grey darken-2'"
              @click="
                filterByCategory(category)
                toggle()
              "
            >
              {{ category }}
            </v-chip>
          </v-slide-item>
        </v-slide-group>

        <!-- Desktop grid for categories -->
        <div class="d-none d-md-flex flex-wrap">
          <v-chip
            v-for="category in categories"
            :key="category"
            class="mr-2 mb-2"
            :color="selectedCategory === category ? 'primary' : 'grey darken-2'"
            @click="filterByCategory(category)"
          >
            {{ category }}
          </v-chip>
        </div>
      </v-col>
      <!-- <v-col cols="12" md="4">
        <v-text-field
          v-model="search"
          label="Buscar proyectos"
          prepend-inner-icon="mdi-magnify"
          outlined
          dense
          hide-details
          clearable
        ></v-text-field>
      </v-col> -->
    </v-row>

    <!-- Projects grid -->
    <v-row dense>
      <v-col
        v-for="(project, index) in filteredProjects"
        :key="project.name"
        cols="12"
        sm="6"
        md="4"
        lg="4"
        class="d-flex"
      >
        <BrandingCard
          :project="project"
          :style="{ transitionDelay: `${index * 50}ms` }"
          class="fade-in"
        />
      </v-col>
    </v-row>

    <!-- Empty state -->
    <v-row v-if="filteredProjects.length === 0" class="text-center py-12">
      <v-col cols="12">
        <v-icon size="64" class="mb-4">mdi-magnify-remove-outline</v-icon>
        <h3 class="text-h5 mb-2">No se encontraron proyectos</h3>
        <p class="text-body-1 text--secondary">
          Intenta con otros términos de búsqueda o categorías
        </p>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import BrandingCard from '~/components/BrandingCard.vue'

export default {
  name: 'BrandingGrid',
  components: {
    BrandingCard,
  },
  props: {
    projects: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      search: '',
      selectedCategory: 'Todos',
    }
  },
  computed: {
    categories() {
      const categories = new Set(this.projects.map((p) => p.category))
      return ['Todos', ...categories]
    },
    filteredProjects() {
      let filtered = [...this.projects]

      // Filter by search term
      if (this.search) {
        const searchTerm = this.search.toLowerCase()
        filtered = filtered.filter(
          (project) =>
            project.name.toLowerCase().includes(searchTerm) ||
            project.description.toLowerCase().includes(searchTerm) ||
            project.delivers.some((d) => d.toLowerCase().includes(searchTerm))
        )
      }

      // Filter by category
      if (this.selectedCategory !== 'Todos') {
        filtered = filtered.filter(
          (project) => project.category === this.selectedCategory
        )
      }

      return filtered
    },
  },
  methods: {
    filterByCategory(category) {
      this.selectedCategory = category
    },
  },
}
</script>

<style scoped>
.branding-container {
  max-width: 1400px;
  margin: 0 auto;
}

.text-shadow {
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.fade-in {
  animation: fadeIn 0.5s ease-in-out;
  animation-fill-mode: both;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Smooth scrolling for anchor links */
html {
  scroll-behavior: smooth;
}
</style>
