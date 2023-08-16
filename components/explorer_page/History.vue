<template>
  <div>
    <v-row>
      <v-col cols="12">
        <h3 class="text-center">Maneja cada pedido de manera eficiente</h3>
      </v-col>
      <v-col v-for="(his, i) in history" :key="i">
        <v-card width="300" class="mx-auto mt-12">
          <v-card-title>{{ his.title }}</v-card-title>
          <v-card-subtitle>{{ his.subtitle }}</v-card-subtitle>
          <v-card-actions>
            <!-- <v-btn>Explorar</v-btn> -->
            <v-spacer></v-spacer>
            <v-btn text outlined color="blue" @click="shower(i)">
              <v-icon>{{ show ? 'mdi-chevronup' : 'mdi-chevron-down' }}</v-icon>
            </v-btn>
          </v-card-actions>
          <v-expand-transition>
            <div v-show="his.show">
              <v-card-text>{{ his.text }}</v-card-text>
            </div>
          </v-expand-transition>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data: () => ({
    show: false,
    history: [
      {
        title: 'Historial',
        subtitle: 'Administra tus pedidos',
        text: 'Administra tus pedidos recibidos en un solo lugar. Con nuestra intuitiva plataforma, puedes ver todos los detalles de cada pedido, inlcuido los productos solicitados, la información de contacto del cliente y el estado actual del pedido.',

        show: false,
      },
      {
        title: 'Estado',
        subtitle: 'Actualizalo al instante',
        text: 'Actualiza el estado de los pedidos según avancen, ya sea "en proceso", "cancelado" o "entregado". Esto te permite mantener un seguimiento claro y asegurarte de que cada pedido se maneje de manera eficiente.',
        show: false,
      },
      {
        title: 'Organización',
        subtitle: 'Todo en un su lugar',
        text: 'Visualiza y gestiona todos tus pedidos para llevar un mejor control de tu negocio',
        show: false,
      },
    ],
  }),
  methods: {
    shower(i) {
      this.$set(this.history[i], 'show', !this.history[i].show)
    },
  },
}
</script>
