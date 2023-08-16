<template>
  <v-row>
    <v-col cols sm="7">
      <!-- <Texto /> -->
    </v-col>
    <v-col cols sm="5">
      <v-carousel
        hide-delimiters
        show-arrows-on-hover
        height="auto"
        width="auto"
        cycle
      >
        <v-carousel-item
          v-for="(item, i) in proyectoActual.slider"
          :key="i"
          :src="item"
          touch
        ></v-carousel-item>
      </v-carousel>
    </v-col>
  </v-row>
</template>

<script>
// import Texto from '@/components/explorer_page/Texto'
export default {
  components: {
    // Texto,
  },
  computed: {
    proyectoActual() {
      return this.$store.state.proyectos.proyectoActual
    },
  },
}
</script>

<style scoped></style>
