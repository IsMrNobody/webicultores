<template>
  <v-row class="video-player" no-gutters>
    <v-col cols="12" md="8" class="main-video-col">
      <v-fade-transition mode="out-in">
        <div class="main-video" :key="selectedVideo.id">
          <iframe
            :src="selectedVideo.url"
            frameborder="0"
            allowfullscreen
            class="responsive-iframe"
          ></iframe>
          <div class="video-details pa-4">
            <h2 class="video-title">{{ selectedVideo.title }}</h2>
            <p class="video-description">{{ selectedVideo.description }}</p>
          </div>
        </div>
      </v-fade-transition>
    </v-col>
    <v-col cols="12" md="4" class="video-list-col">
      <v-select
        v-model="selectedCategory"
        :items="categoryOptions"
        label="Filtrar por categoría"
        dense
        outlined
        class="mb-4"
      />
      <v-list
        two-line
        class="video-list"
        style="height: 400px; overflow-y: auto"
      >
        <v-slide-x-transition group>
          <v-list-item
            v-for="video in filteredVideos"
            :key="video.id"
            :class="{ selected: video.id === selectedVideo.id }"
            @click="selectVideo(video)"
          >
            <v-list-item-avatar>
              <v-img :src="video.thumbnail" />
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title>{{ video.title }}</v-list-item-title>
              <v-list-item-subtitle>{{ video.shortDesc }}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-slide-x-transition>
      </v-list>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'VideoPlayer',
  data() {
    return {
      selectedCategory: 'Todas',
      videos: [
        {
          id: 1,
          title: 'Webicultores - Plataforma de Branding',
          url: 'https://www.youtube.com/embed/4WZg8v3v0nQ',
          description:
            'Demo del proyecto Webicultores, una plataforma moderna para branding digital y gestión de portafolios.',
          shortDesc: 'Branding y portafolio',
          thumbnail: 'https://img.youtube.com/vi/4WZg8v3v0nQ/hqdefault.jpg',
          category: 'Branding',
        },
        {
          id: 2,
          title: 'Sistema de Pedidos Online',
          url: 'https://www.youtube.com/embed/3nQNiWdeH2Q',
          description:
            'Presentación del sistema de pedidos online desarrollado para restaurantes y comercios.',
          shortDesc: 'Pedidos',
          thumbnail: 'https://img.youtube.com/vi/3nQNiWdeH2Q/hqdefault.jpg',
          category: 'E-commerce',
        },
        {
          id: 3,
          title: 'Landing Page - Delivery Express',
          url: 'https://www.youtube.com/embed/2Vv-BfVoq4g',
          description:
            'Ejemplo de landing page creada para el servicio Delivery Express.',
          shortDesc: 'Landing Page',
          thumbnail: 'https://img.youtube.com/vi/2Vv-BfVoq4g/hqdefault.jpg',
          category: 'Marketing',
        },
        {
          id: 4,
          title: 'Portafolio Personal Animado',
          url: 'https://www.youtube.com/embed/ScMzIvxBSi4',
          description:
            'Portafolio personal interactivo con animaciones y presentación de proyectos.',
          shortDesc: 'Portafolio Animado',
          thumbnail: 'https://img.youtube.com/vi/ScMzIvxBSi4/hqdefault.jpg',
          category: 'Diseño',
        },
        {
          id: 5,
          title: 'Video de Presentación de Empresa',
          url: 'https://www.youtube.com/embed/dQw4w9WgXcQ',
          description:
            'Video de presentación de empresa que muestra los servicios y productos ofrecidos.',
          shortDesc: 'Presentación de Empresa',
          thumbnail: 'https://img.youtube.com/vi/dQw4w9WgXcQ/hqdefault.jpg',
          category: 'Publicidad',
        },
        {
          id: 6,
          title: 'Tutoriales de Photoshop',
          url: 'https://www.youtube.com/embed/Gs9jFR5lXZ8',
          description:
            'Serie de tutoriales de Photoshop para aprender técnicas básicas y avanzadas.',
          shortDesc: 'Tutoriales de Photoshop',
          thumbnail: 'https://img.youtube.com/vi/Gs9jFR5lXZ8/hqdefault.jpg',
          category: 'Educación',
        },
        {
          id: 7,
          title: 'Análisis de Mercado',
          url: 'https://www.youtube.com/embed/7UCvLzjGxNk',
          description:
            'Análisis de mercado para entender las tendencias y oportunidades de negocio.',
          shortDesc: 'Análisis de Mercado',
          thumbnail: 'https://img.youtube.com/vi/7UCvLzjGxNk/hqdefault.jpg',
          category: 'Negocios',
        },
        {
          id: 8,
          title: 'Desarrollo de Aplicaciones Móviles',
          url: 'https://www.youtube.com/embed/3r4yJ3N3jKQ',
          description: 'Desarrollo de aplicaciones móviles para Android y iOS.',
          shortDesc: 'Desarrollo de Aplicaciones Móviles',
          thumbnail: 'https://img.youtube.com/vi/3r4yJ3N3jKQ/hqdefault.jpg',
          category: 'Tecnología',
        },
        {
          id: 9,
          title: 'Marketing Digital',
          url: 'https://www.youtube.com/embed/9bZkp7q19f0',
          description:
            'Estrategias de marketing digital para aumentar la visibilidad y ventas en línea.',
          shortDesc: 'Marketing Digital',
          thumbnail: 'https://img.youtube.com/vi/9bZkp7q19f0/hqdefault.jpg',
          category: 'Marketing',
        },
        {
          id: 10,
          title: 'Diseño de Interiores',
          url: 'https://www.youtube.com/embed/4ZnD6xQ8q4g',
          description:
            'Diseño de interiores para crear espacios funcionales y estéticos.',
          shortDesc: 'Diseño de Interiores',
          thumbnail: 'https://img.youtube.com/vi/4ZnD6xQ8q4g/hqdefault.jpg',
          category: 'Arquitectura',
        },
        {
          id: 11,
          title: 'Webicultores - Plataforma de Branding',
          url: 'https://www.youtube.com/embed/4WZg8v3v0nQ',
          description:
            'Demo del proyecto Webicultores, una plataforma moderna para branding digital y gestión de portafolios.',
          shortDesc: 'Branding y portafolio',
          thumbnail: 'https://img.youtube.com/vi/4WZg8v3v0nQ/hqdefault.jpg',
          category: 'Branding',
        },
        {
          id: 4,
          title: 'Portafolio Personal Animado',
          url: 'https://www.youtube.com/embed/ScMzIvxBSi4',
          description:
            'Portafolio personal interactivo con animaciones y presentación de proyectos.',
          shortDesc: 'Portafolio Animado',
          thumbnail: 'https://img.youtube.com/vi/ScMzIvxBSi4/hqdefault.jpg',
        },
      ],
      selectedVideo: {},
    }
  },
  created() {
    this.selectedVideo = this.videos[0]
  },
  computed: {
    categoryOptions() {
      const categories = this.videos.map((v) => v.category).filter(Boolean)
      return ['Todas', ...Array.from(new Set(categories))]
    },
    filteredVideos() {
      if (this.selectedCategory === 'Todas') {
        return this.videos
      }
      return this.videos.filter(
        (video) => video.category === this.selectedCategory
      )
    },
  },
  methods: {
    selectVideo(video) {
      this.selectedVideo = video
    },
  },
}
</script>

<style scoped>
.video-player {
  min-height: 60vh;
}
.main-video-col {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}
.main-video {
  background: #222;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.12);
  margin-bottom: 16px;
}
.responsive-iframe {
  width: 100%;
  aspect-ratio: 16 / 9;
  border: none;
  background: #000;
  transition: box-shadow 0.3s;
}
.video-details {
  background: #fff;
  border-top: 1px solid #eee;
}
.video-title {
  margin: 0 0 8px 0;
  font-weight: 700;
  color: #222;
}
.video-description {
  color: #444;
}
.video-list-col {
  padding-left: 16px;
  padding-right: 16px;
}
.video-list {
  background: transparent;
  height: 400px;
  overflow-y: auto;
}
@media (max-width: 960px) {
  .video-list {
    height: 200px;
  }
}
.v-list-item.selected {
  background: #f5f5f5 !important;
  border-left: 4px solid #1976d2;
}
@media (max-width: 960px) {
  .main-video-col,
  .video-list-col {
    padding-left: 0 !important;
    padding-right: 0 !important;
  }
  .video-list-col {
    margin-top: 24px;
    padding: 0;
  }
}
</style>
