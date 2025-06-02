<template>
  <v-container class="py-8" fluid>
    <!-- Banner pequeño -->
    <v-sheet
      class="mb-6 px-4 py-3 d-flex align-center"
      color="primary"
      elevation="2"
      rounded
    >
      <v-icon color="white" size="32" class="mr-3">mdi-play-circle</v-icon>
      <div>
        <div class="text-h6 font-weight-bold mb-1">
          Galería de Videos de Proyectos
        </div>
        <div class="text-body-2 text-primary-darken-2">
          Explora presentaciones, demos y casos de éxito de mis desarrollos web.
        </div>
      </div>
    </v-sheet>
    <!-- Título mejorado -->
    <!-- <div class="mb-8">
    <h1 class="text-h3 font-weight-bold mb-1">Videos destacados</h1>
    <div class="text-subtitle-1 text-medium-emphasis">
      Inspiración, tecnología y creatividad en acción
    </div>
  </div> -->
    <VideoPlayer />
  </v-container>
</template>

<script>
import VideoPlayer from '~/components/VideoPlayer.vue'

export default {
  name: 'VideoPage',
  components: {
    VideoPlayer,
  },
}
</script>

<style scoped>
.v-container {
  min-height: 80vh;
}
</style>
