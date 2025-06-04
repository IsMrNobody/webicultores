<template>
  <v-row class="video-player" no-gutters>
    <v-col cols="12" md="8" class="main-video-col">
      <v-fade-transition mode="out-in">
        <div :key="selectedVideo.id" class="main-video">
          <iframe
            :src="embedUrl"
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
          title: 'Logo Animación Intro - Arroz Frito Rest',
          url: 'https://youtube.com/shorts/awBgPvo_YvI?feature=share&rel=0',
          description:
            'Logo Animación Intro para el restaurante Arroz Frito Rest.',
          shortDesc: 'Logo Animación Intro',
          thumbnail: 'https://img.youtube.com/vi/awBgPvo_YvI/hqdefault.jpg',
          category: 'Logo Animación',
        },
        {
          id: 2,
          title: 'Animación 2D - Elemento Aire',
          url: 'https://www.youtube.com/watch?v=-pvxoxTGgRY',
          description:
            'Animación 2D de un elemento de aire con efectos de movimiento y iluminación.',
          shortDesc: 'Animación 2D',
          thumbnail:
            'https://i9.ytimg.com/vi/-pvxoxTGgRY/mqdefault.jpg?v=61ad3121&sqp=CLiN-cEG&rs=AOn4CLDroAV_OY_2Ns_B6RFJLN4a8jNbjQ',
          category: 'Animación 2D',
        },
        {
          id: 3,
          title: 'Animación 2D - Elemento Agua',
          url: 'https://www.youtube.com/watch?v=eOG7zGhfUOM',
          description:
            'Animación 2D de un elemento de agua con efectos de movimiento y iluminación.',
          shortDesc: 'Animación 2D',
          thumbnail: 'https://img.youtube.com/vi/eOG7zGhfUOM/hqdefault.jpg',
          category: 'Animación 2D',
        },
        {
          id: 4,
          title: 'Animación 2D - Elemento Fuego',
          url: 'https://www.youtube.com/watch?v=BhIB3obRA8Y',
          description:
            'Animación 2D de un elemento de fuego con efectos de movimiento y iluminación.',
          shortDesc: 'Animación 2D',
          thumbnail: 'https://img.youtube.com/vi/BhIB3obRA8Y/hqdefault.jpg',
          category: 'Animación 2D',
        },
        {
          id: 5,
          title: 'Animación 2D - Cartas',
          url: 'https://www.youtube.com/watch?v=GQ15PRhhYZI',
          description:
            'Animación 2D de cartas con efectos de movimiento y iluminación.',
          shortDesc: 'Animación 2D',
          thumbnail: 'https://img.youtube.com/vi/GQ15PRhhYZI/hqdefault.jpg',
          category: 'Animación 2D',
        },
        {
          id: 6,
          title: 'Video Clip - La Vaca Mariposa',
          url: 'https://www.youtube.com/watch?v=99p136Oyrbs',
          description: 'Video clip animado de la canción "La Vaca Mariposa".',
          shortDesc: 'Video Clip',
          thumbnail: 'https://img.youtube.com/vi/99p136Oyrbs/hqdefault.jpg',
          category: 'IA',
        },
        {
          id: 7,
          title: 'Video Clip - El Loco Juan Carabina',
          url: 'https://www.youtube.com/watch?v=WUPQ-S5FM50',
          description:
            'Video clip animado de la canción "El Loco Juan Carabina".',
          shortDesc: 'Video Clip',
          thumbnail: 'https://img.youtube.com/vi/WUPQ-S5FM50/hqdefault.jpg',
          category: 'IA',
        },
        {
          id: 8,
          title: 'Animación y Mezcla - Taita Querubin',
          url: 'https://www.youtube.com/watch?v=6CQyHIsPVhk',
          description: 'Animación 2D de Taita Querubin.',
          shortDesc: 'Animación 2D',
          thumbnail: 'https://img.youtube.com/vi/6CQyHIsPVhk/hqdefault.jpg',
          category: 'IA',
        },
        {
          id: 9,
          title: 'IA - Shaman Journey',
          url: 'https://www.youtube.com/watch?v=YNVpbm0Ljck',
          description: 'Animación 2D de Shaman Journey.',
          shortDesc: 'Animación 2D',
          thumbnail: 'https://img.youtube.com/vi/YNVpbm0Ljck/hqdefault.jpg',
          category: 'IA',
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
    embedUrl() {
      const url = this.selectedVideo?.url || ''
      // Shorts
      const shortsMatch = url.match(/youtube\.com\/shorts\/([\w-]+)/)
      if (shortsMatch) {
        return `https://www.youtube.com/embed/${shortsMatch[1]}`
      }
      // watch?v=
      const watchMatch = url.match(/[?&]v=([\w-]+)/)
      if (watchMatch) {
        return `https://www.youtube.com/embed/${watchMatch[1]}`
      }
      // Ya es embed o cualquier otro
      return url
    },
  },
  created() {
    this.selectedVideo = this.videos[0]
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
  background: rgb(55, 66, 97) !important;
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
