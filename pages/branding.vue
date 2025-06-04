<template>
  <v-main class="branding-page">
    <BrandingGrid :projects="brandingProjects" />
  </v-main>
</template>

<script>
import BrandingGrid from '~/components/BrandingGrid.vue'

export default {
  layout: 'project',
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
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748787398/JARTATE/Ciudad/lecheria/indios/portada_ind_jbdsjr.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748639367/JARTATE/Ciudad/lecheria/indios/Proyecto_nuevo_1_hnii9t.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748645080/JARTATE/Ciudad/lecheria/indios/002p_dao8lm.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748645080/JARTATE/Ciudad/lecheria/indios/001p_lki4pz.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748639366/JARTATE/Ciudad/lecheria/indios/frnn_m6a6gf.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748639366/JARTATE/Ciudad/lecheria/indios/inlog_maddcz.png',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748787398/JARTATE/Ciudad/lecheria/indios/portada_ind_jbdsjr.png',
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
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748645130/JARTATE/Ciudad/maracay/milacafe/recursos/mil2_vifelg.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748530085/JARTATE/Ciudad/maracay/milacafe/mila_04_wogl10.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748645129/JARTATE/Ciudad/maracay/milacafe/recursos/mil5_tagjbz.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748645129/JARTATE/Ciudad/maracay/milacafe/recursos/mil3_sotlpo.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748645128/JARTATE/Ciudad/maracay/milacafe/recursos/mil4_h8l5di.png',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748645129/JARTATE/Ciudad/maracay/milacafe/recursos/mil5_tagjbz.png',
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
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704762/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Banner/001_1_cvgxog.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704746/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Banner/frt1_e053ho.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704745/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Banner/frt2_a4dwuy.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704744/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Banner/frt45_1_csnmkt.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704763/JARTATE/Ciudad/Anaco/Arroz%20frito%20anaco/Banner/frt4_yjve1u.png',
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
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748552444/Construtech/banners/card85x55_front_zwrpri.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704716/Construtech/banners/web%20page/cons1_iglkbu.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704637/Construtech/banners/web%20page/con5_fizqau.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704715/Construtech/banners/web%20page/const_hl6g2i.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704606/Construtech/banners/web%20page/cons5_qgjepz.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748704604/Construtech/banners/web%20page/tarjetcn_vzrdig.png',
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
          name: 'Latam Turquesa',
          images: [
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748705558/latamturquesa/lat1_vwlbwj.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748706356/latamturquesa/lat3_gksx1f.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748706355/latamturquesa/lat4_qev4mk.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748705578/latamturquesa/lat5_or0yec.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748705558/latamturquesa/lat6_wmdpvd.png',
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748705557/latamturquesa/lat7_lyfhqb.png',
          ],
          image:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748706356/latamturquesa/lat3_gksx1f.png',
          backgroundImage:
            'https://res.cloudinary.com/dku13l2ep/image/upload/v1748532002/Davadiled/Logo/Logo-Davadiled_3_gnp3yq.png',
          description:
            'Sitio web corporativo para firma de consultoría especializada en conectar América Latina y Turquía. Incluye presentación de servicios B2B, turismo y consultoría con diseño elegante y funcional.',
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
