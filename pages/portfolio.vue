<template>
  <v-main class="portfolio-page">
    <PortfolioGrid :projects="projects" />
  </v-main>
</template>

<script>
import PortfolioGrid from '~/components/PortfolioGrid.vue'

export default {
  layout: 'project',
  name: 'PortfolioPage',
  components: {
    PortfolioGrid,
  },
  data() {
    return {
      projects: [
        {
          name: 'Orion Gallery',
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529741/orion/Wallpaper-orion_dqmo3v.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529741/orion/Wallpaper-orion_dqmo3v.png',
          description:
            'Galería de arte digital que presenta una colección exclusiva de obras contemporáneas. Interfaz minimalista que permite explorar y apreciar el arte en un entorno digital inmersivo.',
          websiteUrl: 'https://orion-gallery.netlify.app/',
          technologies: [
            'Vue.js',
            'Nuxt.js',
            'Vuetify',
            'Responsive Design',
            'Art Gallery',
          ],
        },
        {
          name: 'Delivery App',
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748530259/JARTATE/bann_uiilbf.png',
          description:
            'Sistema de reservas para restaurantes con gestión de disponibilidad en tiempo real.',
          websiteUrl: 'https://jartate.online',
          githubUrl: 'https://github.com/usuario/booking-app',
          technologies: ['Vue.js', 'Socket.io', 'Express', 'MongoDB'],
        },
        {
          name: 'InDios Therapy',
          logo: '/W.png',
          image:
            'https://images.unsplash.com/photo-1544161515-4ab6ce6db874?ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80',
          backgroundImage:
            'https://images.unsplash.com/photo-1544161515-4ab6ce6db874?ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80',
          description:
            'Plataforma de bienestar integral que ofrece terapias corporales, productos naturales y un jardín botánico virtual. Combina técnicas ancestrales con métodos modernos para el bienestar físico y mental.',
          websiteUrl: 'https://indiostherapy.netlify.app/',
          technologies: [
            'Vue.js',
            'Nuxt.js',
            'Vuetify',
            'Responsive Design',
            'SPA',
            'MongoDB',
          ],
        },
        {
          name: 'Construtech UK',
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748552444/Construtech/banners/card85x55_front_zwrpri.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529859/Construtech/banners/3_bwkjur.png',
          description:
            'Sitio web corporativo para empresa de construcción y remodelación especializada en fabricación de acero y carpintería personalizada. Diseño elegante que resalta la calidad de sus servicios para proyectos industriales, comerciales y residenciales en Reino Unido.',
          websiteUrl: 'https://construtech.uk/',
          technologies: [
            'HTML5',
            'CSS3',
            'JavaScript',
            'Responsive Design',
            'SEO',
            'WhatsApp Integration',
          ],
        },
        {
          name: 'Latam Turquesa',
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748705558/latamturquesa/lat1_vwlbwj.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748530206/latamturquesa/colorlatam_xbb88s.png',
          description:
            'Sitio web corporativo para firma de consultoría especializada en conectar América Latina y Turquía. Incluye presentación de servicios B2B, turismo y consultoría con diseño elegante y funcional.',
          websiteUrl: 'https://latamturquesa.com/',
          technologies: [
            'Vue.js',
            'Nuxt.js',
            'Responsive Design',
            'SEO',
            'WhatsApp',
          ],
        },
        {
          name: 'Mila Café',
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748645130/JARTATE/Ciudad/maracay/milacafe/recursos/mil2_vifelg.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748530085/JARTATE/Ciudad/maracay/milacafe/mila_04_wogl10.png',
          description:
            'Sitio web para cafetería con servicio de delivery y pick-up. Presenta su menú de productos, horarios de atención y opciones de pedidos online. Diseño atractivo enfocado en la experiencia del cliente.',
          websiteUrl: 'https://milacafe.site/',
          technologies: [
            'Vue.js',
            'Nuxt.js',
            'Responsive Design',
            'UX/UI',
            'Delivery',
            'Menu Online',
          ],
        },
        {
          name: 'Arroz Frito Anaco',
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1698510728/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Logo/LogoAFA_mjfhzh.webp',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1698510728/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Logo/LogoAFA_mjfhzh.webp',
          description:
            'Sitio web para restaurante de comida asiática con servicio de delivery y pick-up. Muestra el menú, horarios y opciones de pedido en línea para una experiencia culinaria auténtica.',
          websiteUrl: 'https://arrozfritoanaco.netlify.app/',
          technologies: [
            'HTML5',
            'CSS3',
            'JavaScript',
            'Node.js',
            'Responsive Design',
            'Menu Online',
          ],
        },

        // {
        //   name: 'E-commerce Moderno',
        //   image:
        //     'https://scontent.fccs3-1.fna.fbcdn.net/v/t39.30808-6/500140464_10162970367814791_6597943751469168474_n.jpg?stp=dst-jpg_s720x720_tt6&_nc_cat=1&ccb=1-7&_nc_sid=aa7b47&_nc_ohc=iMnEEYDBaoQQ7kNvwGKHVgW&_nc_oc=AdnznljU5Io5ODoTGtTC5cVHSdE8xhHX0xuwMbHYYUph4t7ndUuumMhyNdAuD6EPpOg&_nc_zt=23&_nc_ht=scontent.fccs3-1.fna&_nc_gid=GLWRW0kAarPzmGtGOV75IA&oh=00_AfJwnxC1H8qknYHGXhpVYmfODPt3H0bidpcGeRPaUxNKCg&oe=683D82B5',
        //   description:
        //     'Tienda online completa con carrito de compras, pagos y gestión de productos.',
        //   websiteUrl: 'https://ejemplo-ecommerce.com',
        //   githubUrl: 'https://github.com/usuario/ecommerce-project',
        //   technologies: ['Vue.js', 'Node.js', 'MongoDB', 'PayPal'],
        // },
        // {
        //   name: 'Aplicación de Gestión de Tareas',
        //   image: '/images/projects/tasks.jpg',
        //   description:
        //     'Aplicación para gestionar tareas y proyectos con funcionalidades colaborativas.',
        //   websiteUrl: 'https://ejemplo-tasks.com',
        //   githubUrl: 'https://github.com/usuario/task-manager',
        //   technologies: ['Vue.js', 'Vuetify', 'Express', 'PostgreSQL'],
        // },
        // {
        //   name: 'Sitio Web Corporativo',
        //   image: '/images/projects/corporate.jpg',
        //   description:
        //     'Sitio web responsive para empresa con múltiples secciones y formulario de contacto.',
        //   websiteUrl: 'https://ejemplo-corporate.com',
        //   githubUrl: 'https://github.com/usuario/corporate-site',
        //   technologies: ['Vue.js', 'Nuxt.js', 'SCSS', 'Netlify'],
        // },
        // {
        //   name: 'Blog Personal',
        //   image: '/images/projects/blog.jpg',
        //   description:
        //     'Plataforma de blog con sistema de gestión de contenidos y comentarios.',
        //   websiteUrl: 'https://ejemplo-blog.com',
        //   githubUrl: 'https://github.com/usuario/personal-blog',
        //   technologies: ['Vue.js', 'Nuxt.js', 'Markdown', 'Contentful'],
        // },
      ],
    }
  },
  head() {
    return {
      title: 'Mi Portafolio | Webicultores',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Portafolio de proyectos web y aplicaciones desarrolladas',
        },
      ],
    }
  },
}
</script>

<style scoped>
.portfolio-page {
  min-height: 100vh;
}
</style>
