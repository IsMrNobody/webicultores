<template>
  <v-hover v-slot="{ hover }" open-delay="100">
    <v-card
      :class="{ 'on-hover': hover }"
      class="d-flex flex-column"
      elevation="2"
      :ripple="false"
      @click="openDialog"
    >
      <!-- Project image with overlay -->
      <div class="image-container">
        <v-img
          :src="
            project.image ||
            (project.images && project.images.length > 0
              ? project.images[0]
              : project.backgroundImage)
          "
          :alt="project.name"
          height="100%"
          width="100%"
          class="project-image"
          cover
          gradient="to bottom, rgba(0,0,0,0.1), rgba(0,0,0,0.4)"
        >
          <template #placeholder>
            <v-row class="fill-height ma-0" align="center" justify="center">
              <v-progress-circular
                indeterminate
                color="grey lighten-5"
              ></v-progress-circular>
            </v-row>
          </template>
          <div class="overlay d-flex flex-column justify-space-between">
            <div class="d-flex justify-space-between align-start pa-4">
              <v-chip
                v-if="project.category"
                small
                color="primary"
                class="font-weight-bold"
              >
                {{ project.category }}
              </v-chip>
              <span v-if="project.year" class="white--text caption">
                {{ project.year }}
              </span>
            </div>
            <div class="px-4 pb-4">
              <h3 class="text-h5 font-weight-bold white--text mb-2">
                {{ project.name }}
              </h3>
              <v-chip-group column>
                <v-chip
                  v-for="(item, i) in project.deliverables.slice(0, 3)"
                  :key="i"
                  x-small
                  color="white"
                  text-color="primary"
                  class="mr-1 mb-1"
                >
                  {{ item }}
                </v-chip>
                <v-chip
                  v-if="project.deliverables.length > 3"
                  x-small
                  color="white"
                  text-color="primary"
                  class="mr-1 mb-1"
                >
                  +{{ project.deliverables.length - 3 }} más
                </v-chip>
              </v-chip-group>
            </div>
          </div>
        </v-img>
      </div>

      <!-- Project details -->
      <div class="d-flex flex-column flex-grow-1 pa-4">
        <p class="text--secondary text-body-2 mb-4 line-clamp-3">
          {{ project.description }}
        </p>
        <v-spacer></v-spacer>
        <div class="d-flex justify-space-between align-center">
          <v-btn
            color="primary"
            text
            small
            class="text-capitalize px-0"
            @click.stop="openDialog"
          >
            Ver proyecto
            <v-icon right small>mdi-arrow-right</v-icon>
          </v-btn>
          <v-btn
            v-if="project.websiteUrl"
            icon
            small
            color="primary"
            :href="project.websiteUrl"
            target="_blank"
            rel="noopener noreferrer"
            @click.stop
          >
            <v-icon small>mdi-open-in-new</v-icon>
          </v-btn>
        </div>
      </div>

      <!-- Expanded Project Dialog -->
      <v-dialog v-model="dialog" max-width="1200" scrollable>
        <v-card>
          <v-card-title class="headline">
            {{ project.name }}
            <v-spacer></v-spacer>
            <v-btn icon @click="dialog = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-text class="pa-0">
            <v-container fluid class="pa-0">
              <v-row no-gutters>
                <v-col cols="12" md="6" class="d-flex align-center">
                  <v-carousel
                    v-if="project.images && project.images.length > 0"
                    height="400"
                    hide-delimiters
                    show-arrows-on-hover
                    cycle
                    interval="3500"
                  >
                    <v-carousel-item
                      v-for="(img, idx) in project.images"
                      :key="idx"
                    >
                      <v-img
                        :src="img"
                        :alt="project.name + ' ' + (idx + 1)"
                        height="400"
                        class="project-image"
                        gradient="to bottom, rgba(0,0,0,0.1), rgba(0,0,0,0.6)"
                      >
                        <template #placeholder>
                          <v-row
                            class="fill-height ma-0"
                            align="center"
                            justify="center"
                          >
                            <v-progress-circular
                              indeterminate
                              color="grey lighten-5"
                            ></v-progress-circular>
                          </v-row>
                        </template>
                      </v-img>
                    </v-carousel-item>
                  </v-carousel>
                  <v-img
                    v-else
                    :src="project.backgroundImage || project.image"
                    :alt="project.name"
                    height="400"
                    class="project-image"
                    gradient="to bottom, rgba(0,0,0,0.1), rgba(0,0,0,0.4)"
                  >
                    <template #placeholder>
                      <v-row
                        class="fill-height ma-0"
                        align="center"
                        justify="center"
                      >
                        <v-progress-circular
                          indeterminate
                          color="grey lighten-5"
                        ></v-progress-circular>
                      </v-row>
                    </template>
                  </v-img>
                </v-col>
                <v-col cols="12" md="6" class="pa-6">
                  <div class="mb-6">
                    <v-chip
                      v-if="project.category"
                      color="primary"
                      class="mb-4"
                    >
                      {{ project.category }}
                    </v-chip>
                    <h2 class="text-h4 mb-2">{{ project.name }}</h2>
                    <div class="d-flex align-center mb-4">
                      <v-icon small class="mr-1">mdi-calendar</v-icon>
                      <span class="text-caption">{{ project.year }}</span>
                    </div>
                    <p class="text-body-1 mb-6">
                      {{ project.description }}
                    </p>
                  </div>

                  <div class="mb-6">
                    <h3 class="text-subtitle-1 font-weight-bold mb-3">
                      Entregables
                    </h3>
                    <v-chip-group column>
                      <v-chip
                        v-for="(item, i) in project.deliverables"
                        :key="i"
                        color="primary"
                        outlined
                        class="mr-2 mb-2"
                      >
                        {{ item }}
                      </v-chip>
                    </v-chip-group>
                  </div>

                  <v-divider class="my-4"></v-divider>

                  <div class="d-flex justify-space-between align-center">
                    <v-btn
                      v-if="project.websiteUrl"
                      color="primary"
                      :href="project.websiteUrl"
                      target="_blank"
                      rel="noopener noreferrer"
                      class="text-capitalize"
                      @click.stop
                    >
                      Visitar sitio web
                      <v-icon right>mdi-open-in-new</v-icon>
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      class="text-capitalize"
                      @click="dialog = false"
                    >
                      Cerrar
                    </v-btn>
                  </div>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
        </v-card>
      </v-dialog>
    </v-card>
  </v-hover>
</template>

<script>
export default {
  name: 'BrandingCard',
  props: {
    project: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      dialog: false,
    }
  },
  watch: {
    dialog(val) {
      if (!val) {
        this.closeDialog()
      }
    },
  },
  beforeDestroy() {
    // Ensure body scroll is re-enabled if component is destroyed with dialog open
    if (this.dialog) {
      this.closeDialog()
    }
  },
  methods: {
    openDialog() {
      this.dialog = true
      // Prevent body scroll when dialog is open
      document.body.style.overflow = 'hidden'
    },
    closeDialog() {
      this.dialog = false
      // Re-enable body scroll when dialog is closed
      document.body.style.overflow = ''
    },
  },
}
</script>

<style scoped>
.v-card {
  transition: all 0.3s ease-in-out;
  border-radius: 12px !important;
  overflow: hidden;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.v-card:not(.on-hover) {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06) !important;
}

.v-card.on-hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1),
    0 10px 10px -5px rgba(0, 0, 0, 0.04) !important;
}

.image-container {
  position: relative;
  width: 100%;
  height: 300px; /* Fixed height for the image container */
  overflow: hidden;
}

.project-image {
  width: 100%;
  height: 100%;
  object-fit: none;
  background: #222;
}

.overlay {
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
  opacity: 1;
  transition: opacity 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 16px;
}

.v-card:not(:hover) .overlay {
  opacity: 1;
}

.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Custom scrollbar for dialog */
.v-dialog .v-card__text {
  scrollbar-width: thin;
  scrollbar-color: #e0e0e0 transparent;
}

.v-dialog .v-card__text::-webkit-scrollbar {
  width: 6px;
}

.v-dialog .v-card__text::-webkit-scrollbar-track {
  background: transparent;
}

.v-dialog .v-card__text::-webkit-scrollbar-thumb {
  background-color: #e0e0e0;
  border-radius: 3px;
}

/* Fix for dialog close button on mobile */
@media (max-width: 600px) {
  .v-dialog .v-card__title {
    padding: 12px 16px;
  }

  .v-dialog .v-btn--icon {
    margin: 0;
  }
}
</style>
