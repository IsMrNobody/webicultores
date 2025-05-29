<template>
  <div>
    <v-row justify="center">
      <v-img
        class="ornament d-none d-lg-flex d-xl-none"
        src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692057331/webicultores/banner_sowbqu-transformed_1_aute43.png"
      >
      </v-img>
      <v-col class="logo">
        <v-img
          class="mx-auto"
          max-width="600"
          src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040944/webicultores/log_s_dy28na.png"
        ></v-img>
      </v-col>
      <v-col class="text text-center">
        <h1>Aplicaciones para negocios con servicio de entrega delivery</h1>
        <h2>Dale a tus clientes una nueva herramienta de compra</h2>
        <!-- <v-btn class="mt-4" color="blue">Crear tienda</v-btn> -->
      </v-col>
    </v-row>
    <v-parallax
      height="460"
      src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040950/webicultores/bg1_x2rolh.png"
    >
    </v-parallax>
  </div>
</template>

<style scoped>
.ornament {
  position: absolute;
  margin-top: 170px;
  z-index: 2;
  transform: scaleY(-1);
}
.logo {
  position: absolute;
  z-index: 2;
  margin-top: 110px;
}
.text {
  margin-top: 210px;
  position: absolute;
  z-index: 2;
  color: rgb(33, 107, 136);
  padding: 4px;
}
</style>
