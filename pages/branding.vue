<template>
  <v-main class="branding-page">
    <BrandingGrid :projects="brandingProjects" />
  </v-main>
</template>

<script>
import BrandingGrid from '~/components/BrandingGrid.vue'

export default {
  name: 'BrandingPage',
  components: {
    BrandingGrid,
  },
  data() {
    return {
      brandingProjects: [
        {
          name: 'Orion Gallery',
          images: [
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748622572/orion/logorion_2_wj8rol.png5',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748622607/orion/00134-absolutereality_v181_4214998850_jrzjnr.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529741/orion/Wallpaper-orion_dqmo3v.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748622811/orion/portada_jlwj1n.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748622963/orion/Capa_3_vlztxv.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748622963/orion/Capa_2_arcmeb.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748623028/orion/inst_sn4bxp.png',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529741/orion/Wallpaper-orion_dqmo3v.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748532000/Orion/Logo/Logo-Orion-Gallery_3_mlj2zr.png',
          description:
            'Identidad visual para galería de arte contemporáneo. El diseño refleja elegancia y modernidad, con un sistema flexible para exhibir diversas expresiones artísticas.',
          deliverables: [
            'Logotipo',
            'Sistema Visual',
            'Material Promocional',
            'Señalética',
          ],
          category: 'Arte y Cultura',
          year: '2024',
        },
        {
          name: 'InDios Therapy',
          images: [
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748639367/JARTATE/Ciudad/lecheria/indios/Proyecto_nuevo_1_hnii9t.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748639366/JARTATE/Ciudad/lecheria/indios/frnn_m6a6gf.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748639366/JARTATE/Ciudad/lecheria/indios/inlog_maddcz.png',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529741/orion/Wallpaper-orion_dqmo3v.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748532001/InDios/Logo/Logo-Indios-Therapy_2_kn8d9x.png',
          description:
            'Diseño de identidad de marca para centro de terapias holísticas. Se creó un logotipo que combina elementos orgánicos y espirituales, junto con una paleta de colores relajantes y tipografía moderna.',
          deliverables: [
            'Logotipo',
            'Paleta de colores',
            'Tipografía',
            'Aplicaciones',
          ],
          category: 'Identidad Corporativa',
          year: '2024',
        },
        {
          name: 'Mila Café',
          images: [
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748551345/JARTATE/Ciudad/maracay/milacafe/volante_dsafvi.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748530085/JARTATE/Ciudad/maracay/milacafe/mila_04_wogl10.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748551309/JARTATE/Ciudad/maracay/milacafe/menu_02_ycbkc5.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748551325/JARTATE/Ciudad/maracay/milacafe/stand_up_bywb9j.png',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1698510728/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Logo/LogoAFA_mjfhzh.webp',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1698510728/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Logo/LogoAFA_mjfhzh.webp',
          description:
            'Branding completo para cafetería artesanal. Se desarrolló una identidad cálida y acogedora que refleja la calidad de sus productos y el ambiente único del local.',
          deliverables: ['Logotipo', 'Papelería', 'Menú', 'Redes Sociales'],
          category: 'Restaurantes',
          year: '2023',
        },
        {
          name: 'Arroz Frito Anaco',
          images: [
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1698511498/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Logo/poster_AFA_eh2lwe.jpg',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748530085/JARTATE/Ciudad/maracay/milacafe/mila_04_wogl10.png',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1698510728/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Logo/LogoAFA_mjfhzh.webp',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1698510728/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Logo/LogoAFA_mjfhzh.webp',
          description:
            'Diseño de marca para restaurante de comida asiática. Se creó una identidad visual que fusiona elementos orientales con un toque moderno, destacando la autenticidad de sus platillos.',
          deliverables: [
            'Logotipo',
            'Identidad Visual',
            'Menú',
            'Material POP',
          ],
          category: 'Restaurantes',
          year: '2023',
        },
        {
          name: 'Construtech UK',
          images: [
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529859/Construtech/banners/3_bwkjur.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1725375530/Construtech/banners/pexels-pixabay-73833_lrbbxc.jpg',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748552444/Construtech/banners/card85x55_front_zwrpri.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529859/Construtech/Logo/Logo-Construtech-UK_3_mlw9l3.png',
          description:
            'Identidad corporativa para empresa de construcción en Reino Unido. Se desarrolló un sistema visual que transmite solidez, profesionalismo y confiabilidad.',
          deliverables: [
            'Logotipo',
            'Manual de Marca',
            'Presentaciones',
            'Señalética',
          ],
          category: 'Construcción',
          year: '2023',
        },
        {
          name: 'Davadiled',
          images: [
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748529859/Construtech/banners/3_bwkjur.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1725375530/Construtech/banners/pexels-pixabay-73833_lrbbxc.jpg',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748532002/Davadiled/Logo/Logo-Davadiled_2_nyxwvj.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748532002/Davadiled/Logo/Logo-Davadiled_3_gnp3yq.png',
          description:
            'Diseño de marca para tienda de iluminación LED. Se creó una identidad moderna y versátil que refleja innovación y calidad en productos de iluminación.',
          deliverables: [
            'Logotipo',
            'Identidad Visual',
            'Catálogo',
            'Redes Sociales',
          ],
          category: 'Retail',
          year: '2023',
        },
      ],
    }
  },
  head() {
    return {
      title: 'Diseño de Marca | Webicultores',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content:
            'Portafolio de diseño de marca e identidad visual - Creación de marcas memorables y efectivas',
        },
      ],
    }
  },
}
</script>

<style scoped>
.branding-page {
  min-height: 100vh;
  /* background-color: #f8f9fa; */
}
</style>
