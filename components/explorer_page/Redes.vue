<template>
  <div>
    <v-row class="text-center">
      <v-col v-for="(red, i) in redes" :key="i" align="center">
        <v-card flat class="transparent" :to="red.to">
          <v-icon color="#00B0FF" size="40">{{ red.img }}</v-icon>
          <p>{{ red.red }}</p>
        </v-card>
      </v-col>
      <v-col cols="12">
        <a href="mailto:webicultures@gmail.com" class="white--text">
          E-mail
          <v-icon color="#00B0FF">mdi-email</v-icon>
        </a>
        <p class="subtitle--text">
          Cultura web que fortalece el crecimiento humano
        </p>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data: () => ({
    redes: [
      { red: 'Instagram', img: 'mdi-instagram', to: '/' },
      { red: 'Youtube', img: 'mdi-youtube', to: '/' },
      { red: 'Facebook', img: 'mdi-facebook', to: '/' },
    ],
  }),
}
</script>

<style>
a {
  text-decoration: none;
}
</style>
