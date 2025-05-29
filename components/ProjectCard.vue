<template>
  <v-hover v-slot="{ isHovering, props }">
    <v-card
      v-bind="props"
      :elevation="isHovering ? 8 : 2"
      :class="['project-card', { 'gradient-border': isHovering }]"
      :style="cardBackgroundStyle"
      @click="toggleExpand"
      rounded="lg"
      class="mb-6"
    >
      <v-card-item class="card-content">
        <template v-slot:prepend>
          <v-avatar rounded="lg" size="100" class="mr-3 project-avatar">
            <v-img
              :src="project.image"
              alt="Project thumbnail"
              cover
              :aspect-ratio="1"
              class="rounded-lg"
            ></v-img>
          </v-avatar>
        </template>

        <v-card-title class="text-h5 font-weight-bold text-white">{{
          project.name
        }}</v-card-title>

        <template v-slot:append>
          <v-icon
            size="large"
            :color="isExpanded ? 'primary' : 'grey'"
            :icon="isExpanded ? 'mdi-chevron-up' : 'mdi-chevron-down'"
            class="mr-2 transition-swing"
          ></v-icon>
        </template>
      </v-card-item>

      <v-expand-transition>
        <div v-if="isExpanded">
          <v-divider class="mx-4 divider-light"></v-divider>

          <v-card-text class="pt-4 card-content">
            <v-sheet class="pa-4 mb-4 content-overlay rounded-lg">
              <p v-if="project.description" class="text-body-1 mb-4">
                {{ project.description }}
              </p>

              <!-- <div
                v-if="project.technologies && project.technologies.length"
                class="mb-4 d-flex flex-wrap gap-2"
              >
                <v-chip
                  v-for="(tech, index) in project.technologies"
                  :key="index"
                  variant="elevated"
                  :color="getChipColor(index)"
                  class="font-weight-medium ma-1"
                  label
                >
                  {{ tech }}
                </v-chip>
              </div> -->

              <v-row class="mt-4">
                <v-col cols="12" sm="auto" v-if="project.websiteUrl">
                  <v-btn
                    color="primary"
                    block
                    :href="project.websiteUrl"
                    target="_blank"
                    prepend-icon="mdi-web"
                    variant="elevated"
                  >
                    Visitar sitio web
                  </v-btn>
                </v-col>
                <!-- <v-col cols="12" sm="auto" v-if="project.githubUrl">
                  <v-btn 
                    color="secondary" 
                    :href="project.githubUrl" 
                    target="_blank" 
                    prepend-icon="mdi-github"
                    variant="elevated"
                    rounded="pill"
                    class="px-6"
                  >
                    Ver código
                  </v-btn>
                </v-col> -->
              </v-row>
            </v-sheet>
          </v-card-text>
        </div>
      </v-expand-transition>
    </v-card>
  </v-hover>
</template>

<script>
export default {
  name: 'ProjectCard',
  props: {
    project: {
      type: Object,
      required: true,
      default: () => ({
        name: '',
        image: '',
        description: '',
        websiteUrl: '',
        githubUrl: '',
        technologies: [],
      }),
    },
  },
  data() {
    return {
      isExpanded: false,
      colors: ['primary', 'secondary', 'info', 'success', 'warning', 'error'],
    }
  },
  computed: {
    cardBackgroundStyle() {
      if (this.project.backgroundImage) {
        return {
          backgroundImage: `url(${this.project.backgroundImage})`,
          backgroundSize: 'cover',
          backgroundPosition: 'center',
          backgroundRepeat: 'no-repeat',
          backgroundColor: 'rgba(0, 0, 0, 0.5)',
          backgroundBlendMode: 'overlay',
        }
      } else if (this.project.image) {
        return {
          backgroundImage: `url(${this.project.image})`,
          backgroundSize: 'cover',
          backgroundPosition: 'center',
          backgroundRepeat: 'no-repeat',
          backgroundColor: 'rgba(0, 0, 0, 0.7)',
          backgroundBlendMode: 'overlay',
        }
      }
      return {}
    },
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded
    },
    getChipColor(index) {
      return this.colors[index % this.colors.length]
    },
  },
}
</script>

<style scoped>
.project-card {
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.card-content {
  position: relative;
  z-index: 200;
}

.project-avatar {
  border: 3px solid rgba(255, 255, 255, 0.8);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.text-white {
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.6);
}

.divider-light {
  border-color: rgba(255, 255, 255, 0.2);
}

.content-overlay {
  background-color: rgba(80, 99, 128, 0.48);
  backdrop-filter: blur(5px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.gradient-border::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(
    90deg,
    var(--v-primary-base),
    var(--v-secondary-base),
    var(--v-info-base),
    var(--v-success-base)
  );
  background-size: 300% 100%;
  animation: gradient-animation 3s ease infinite;
  z-index: 3;
}

/* Overlay oscuro para mejorar la legibilidad del texto */
.project-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.3) 0%,
    rgba(0, 0, 0, 0.7) 100%
  );
  z-index: 1;
  pointer-events: none;
}

@keyframes gradient-animation {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
</style>
