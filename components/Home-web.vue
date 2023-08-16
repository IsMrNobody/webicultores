<template>
  <div>
    <v-col>
      <v-card
        max-width="600"
        flat
        class="mx-auto text-center transparent top"
      >
        <v-col>
          <h2 class="bien">Bienvenidos a Webicultores</h2>
          <span>
            Tu solución de vanguardia para llevar tú restaurante al mundo
            digital
          </span>
        </v-col>
        <v-col>
          <v-img class="mx-auto" width="80" src="/W.png"></v-img>
        </v-col>
        <v-card-subtitle>
          En Webicultores creamos aplicaciones web personalizadas que te
          permiten tener una tienda en línea para recibir pedidos de manera
          eficiente y satisfacer las necesidades de tus clientes al instante
        </v-card-subtitle>
      </v-card>
    </v-col>
    <v-col class="top" max-width="800">
      <h2 class="text-center mb-12 mt-12">
        Mejora el servicio de entrega a domicilio
      </h2>
      <v-row>
        <v-col sm="6">
          <v-img class="mx-auto" max-width="200" src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040947/webicultores/de2_tdtlfw.png"></v-img>
        </v-col>
        <v-col sm="6" cols>
          <v-card>
            <v-card-title class="mb-12">
              Crea tu tienda online y mejora la calidad de servicio a tus
              clientes
            </v-card-title>
            <v-card-subtitle>
              Destaca tus platos para atraer nuevos comensales con una página de
              inicio atractiva y fácil de usar. Nuestra experiencia en diseño y
              desarrollo web garantiza que tu presencia en linea refleje calidad
              y el estilo de tu restaurante o local.
            </v-card-subtitle>
          </v-card>
        </v-col>
      </v-row>
    </v-col>

    <v-col class="top" max-width="800">
      <h2 class="text-center mb-12">Recibe pedidos al instante</h2>
      <v-row>
        <v-col sm="6">
          <v-card>
            <v-card-title class="mx-auto mb-12 pr-1">
              Tus pedidos directo a tu número de WhatsApp
            </v-card-title>
            <v-card-subtitle>
              Nuestra integración con WhatsApp permite que tus pedidos lleguen
              en tiempo real a tú numero de WhatsApp comercial. Olvídate de las
              llamadas telefónicas y mejora la eficiencia de tu restaurante o
              negocio al recibir los pedidos de forma directa y sin
              complicaciones.
            </v-card-subtitle>
          </v-card>
        </v-col>
        <v-col sm="6">
          <v-img max-width="400" class="mx-auto" src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040941/webicultores/order_pfcxs2.png"></v-img>
        </v-col>
      </v-row>
    </v-col>

    <v-col max-width="800">
      <h2 class="mb-12 top text-center">Menú y gestión de pedidos</h2>
      <v-row>
        <v-col sm="6">
          <v-img max-width="250" class="mx-auto" src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040940/webicultores/de3_gs8wxy.png"></v-img>
        </v-col>
        <v-col sm="6" cols>
          <v-card>
            <v-card-title class="mb-12">
              Muestra tu menú o catálogo de productos en línea
            </v-card-title>
            <v-card-subtitle>
              Ofrecemos una página de menús y gestión de pedidos intuitiva y
              práctica, podrás mostrar tus platos o productos, descripciones
              detalladas y precios actualizados, tu clientes podrán navegar por
              tu menú, seleccionar los elementos que desea y realizar los
              pedidos directamente desde tu sitio web.
            </v-card-subtitle>
          </v-card>
        </v-col>
      </v-row>
    </v-col>

    <v-row class="top text-center" justify="center">
      <v-card class="transparent">
        <h2>Diseño totalmente personalizado</h2>
        <v-card-subtitle>
          Crea una página web totalmente única y personalizada ajustada a tu
          marca, sorprende a tus clientes con nuevas herramientas
        </v-card-subtitle>
        <v-img height="200" width="100%" src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040942/webicultores/ban_zejeog.png">
          <v-row justify="center">
            <v-col>
              <v-btn outlined color="#00B0FF" x-large class="mt-12"
                >Crear tienda</v-btn
              >
              <p class="mt-2">
                Sumergete en la webicultura y has florecer tu negocio
              </p>
            </v-col>
          </v-row>
        </v-img>
      </v-card>
    </v-row>

    <History class="top" />

    <v-row class="top">
      <v-col sm="5" cols>
        <v-img max-width="200" class="mx-auto mb-8" src="https://res.cloudinary.com/dku13l2ep/image/upload/v1692040940/webicultores/de_srvrzy.png"></v-img>
        <v-card outlined color="#01579B">
          <v-card-text>
            En Webicucultores nos esforzamos por brindar una experiencia fluida,
            tanto para los restaurantes como para los clientes.
          </v-card-text>
        </v-card>
      </v-col>

      <v-col>
        <p>
          Contactanos para descubrir cómo podemos ayudarte a llevar tu negocio
          al siguiente nivel!
        </p>
        <Contacto />
      </v-col>
    </v-row>

    <Redes class="top" />
  </div>
</template>

<script>
import History from '@/components/explorer_page/History'
import Contacto from '@/components/explorer_page/ContactForm'
import Redes from '@/components/explorer_page/Redes'
export default {
  components: {
    History,
    Contacto,
    Redes,
  },
  data: () => ({
    model: null,
  }),

  computed: {
    galerias() {
      return this.$store.state.proyectos.galerias2
    },
  },
  methods: {
    verProyecto(i) {
      this.$store.dispatch('proyectos/proyectoActual', i)
    },
  },
}
</script>

<style scoped>
.bien {
  margin-top: 100px;
}
.top {
  margin-top: 18%;
}
.mar {
  margin: 2% 0 8% 0;
}
.tamCard {
  opacity: 0.6;
  max-height: 80px;
  transition: 0.3s;
}
.tamCard:hover {
  opacity: 1;
  margin-top: -20px;
  max-height: 200px;
}
.opaText {
  margin: 0.5rem 0;
}
</style>
