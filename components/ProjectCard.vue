<template>
  <v-hover v-slot="{ isHovering, props }">
    <v-card
      v-bind="props"
      :elevation="isHovering ? 8 : 2"
      :class="['project-card', { 'gradient-border': isHovering }]"
      :style="cardBackgroundStyle"
      rounded="lg"
      class="mb-6"
      @click="toggleExpand"
    >
      <img
        v-if="project.image"
        :src="project.image"
        alt="Fondo proyecto"
        class="bg-image"
        draggable="false"
      />
      <v-card-item class="card-content">
        <template #prepend>
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
      </v-card-item>

      <v-dialog v-model="isDialogOpen" max-width="500">
        <!-- El diálogo se abre al hacer click en la tarjeta (no hay botón activador aquí) -->
        <v-card class="pa-3" style="background: #181a20; color: #fff">
          <v-row
            class="dialog-logo-container mb-2"
            align="center"
            justify="center"
          >
            <v-col
              cols="12"
              class="d-flex flex-column align-center justify-center"
            >
              <v-avatar v-if="project.logo" size="60" class="dialog-logo mb-2">
                <v-img :src="project.logo" alt="Logo proyecto" cover></v-img>
              </v-avatar>
              <v-card-title class="text-h5 font-weight-bold mb-2 text-center">
                {{ project.name }}
              </v-card-title>
            </v-col>
          </v-row>
          <v-divider class="divider-light mb-3"></v-divider>
          <v-card-text>
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
                <v-col v-if="project.websiteUrl" cols="12" sm="auto">
                  <a
                    v-if="project.websiteUrl"
                    :href="project.websiteUrl"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="star-btn-gradient d-inline-flex align-center justify-center mt-2"
                    style="text-decoration: none; min-width: 180px"
                  >
                    <v-icon color="white" left class="star-icon"
                      >mdi-web</v-icon
                    >
                    Visitar sitio web
                  </a>
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
        </v-card>
      </v-dialog>
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
        logo: '',
        description: '',
        websiteUrl: '',
        githubUrl: '',
        technologies: [],
      }),
    },
  },
  data() {
    return {
      isDialogOpen: false,
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
      this.isDialogOpen = !this.isDialogOpen
    },
    getChipColor(index) {
      return this.colors[index % this.colors.length]
    },
  },
}
</script>

<style scoped>
.project-card {
  transition: all 0.35s cubic-bezier(0.4, 2, 0.3, 1);
  position: relative;
  overflow: hidden;
  min-height: 300px;
  box-shadow: 0 4px 18px rgba(0, 0, 0, 0.16), 0 1.5px 7px rgba(0, 0, 0, 0.1);
  cursor: pointer;
}

.project-card:hover {
  transform: scale(1.025) translateY(-4px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.22), 0 3px 14px rgba(0, 0, 0, 0.16);
}

.project-card .bg-image {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 0;
  transition: transform 0.5s cubic-bezier(0.4, 2, 0.3, 1);
}

.project-card:hover .bg-image {
  transform: scale(1.07);
}

.card-content {
  position: relative;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  padding: 1.2rem 1.5rem 0.6rem 1.5rem;
}

.project-avatar {
  border: 3px solid rgba(255, 255, 255, 0.8);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.text-white {
  color: #fff !important;
  text-shadow: 1.5px 2px 8px rgba(0, 0, 0, 0.65), 0 1px 2px rgba(0, 0, 0, 0.5);
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
    120deg,
    rgba(0, 0, 0, 0.38) 0%,
    rgba(0, 0, 0, 0.65) 60%,
    rgba(0, 0, 0, 0.85) 100%
  );
  z-index: 1;
  pointer-events: none;
  transition: background 0.4s cubic-bezier(0.4, 2, 0.3, 1);
}

.project-card:hover::after {
  background: linear-gradient(
    120deg,
    rgba(0, 0, 0, 0.32) 0%,
    rgba(0, 0, 0, 0.55) 60%,
    rgba(0, 0, 0, 0.8) 100%
  );
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
/* Botón inspirado en el ejemplo proporcionado */
.star-btn-gradient {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: linear-gradient(
    90deg,
    rgb(56, 102, 255) 0%,
    rgb(143, 220, 255) 100%
  );
  color: #fff;
  border: 1.5px solid rgba(255, 255, 255, 0.18);
  border-radius: 2rem;
  padding: 0.5rem 1.2rem;
  font-weight: 500;
  font-size: 1rem;
  box-shadow: 0 2px 8px rgba(161, 143, 255, 0.08);
  cursor: pointer;
  transition: box-shadow 0.2s, transform 0.2s;
  outline: none;
  position: relative;
  z-index: 2;
}
.star-btn-gradient:hover {
  box-shadow: 0 4px 18px rgba(255, 123, 123, 0.16),
    0 2px 8px rgba(161, 143, 255, 0.16);
  transform: scale(1.04);
}
.star-icon {
  color: #ffb86c;
  font-size: 1.25em;
  margin-right: 0.5rem;
}
.dialog-logo-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
.dialog-logo {
  background: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.12);
  padding: 3px;
}
</style>
