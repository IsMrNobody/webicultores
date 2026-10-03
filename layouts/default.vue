<template>
  <v-app dark>
    <!-- App Bar -->
    <!-- <v-app-bar app color="dark" dark elevate-on-scout>
      <v-app-bar-nav-icon
        class="hidden-md-and-up"
        @click.stop="drawer = !drawer"
      />

      <v-toolbar-title class="d-flex align-center">
        <v-img
          src="/logo.png"
          max-height="40"
          max-width="40"
          contain
          class="mr-2"
        ></v-img>
        <span class="font-weight-bold">Webicultores</span>
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <div class="d-none d-md-flex">
        <v-btn
          v-for="(item, i) in navItems"
          :key="i"
          :to="item.to"
          text
          class="mx-1 text-capitalize"
          active-class="white--text"
        >
          <v-icon left>{{ item.icon }}</v-icon>
          {{ item.title }}
        </v-btn>
      </div>

      <v-btn icon class="d-md-none" @click.stop="drawer = !drawer">
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar> -->

    <!-- Mobile Navigation Drawer -->
    <v-navigation-drawer v-model="drawer" app temporary right class="d-md-none">
      <v-list nav dense>
        <v-list-item-group v-model="group" active-class="primary--text">
          <v-list-item
            v-for="(item, i) in navItems"
            :key="i"
            :to="item.to"
            @click="drawer = false"
          >
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <!-- Main Content -->
    <v-main>
      <v-container fluid class="pa-0">
        <Nuxt />
      </v-container>
    </v-main>
    <!-- Footer -->
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      drawer: false,
      group: null,
      // Header Navigation
      navItems: [
        {
          icon: 'mdi-code-tags',
          title: 'Desarrollo Web',
          to: '/portfolio',
        },
        {
          icon: 'mdi-silverware-fork-knife',
          title: 'Restaurantes',
          to: '/restaurantes',
        },
        {
          icon: 'mdi-palette',
          title: 'Diseño de Marca',
          to: '/branding',
        },
        {
          icon: 'mdi-email',
          title: 'Contacto',
          to: '/contact',
        },
      ],
      // Social Media Icons
      socialIcons: [
        {
          name: 'mdi-facebook',
          link: 'https://facebook.com/webicultores',
        },
        {
          name: 'mdi-instagram',
          link: 'https://instagram.com/webicultores',
        },
        {
          name: 'mdi-linkedin',
          link: 'https://linkedin.com/company/webicultores',
        },
        {
          name: 'mdi-github',
          link: 'https://github.com/webicultores',
        },
      ],
    }
  },
  watch: {
    group() {
      this.drawer = false
    },
  },
}
</script>

<style scoped>
.gradient-hover-btn {
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
  border-radius: 4px;
  padding: 8px 16px;
}

.gradient-hover-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border: 2px solid transparent;
  background: transparent;
  border-radius: 4px;
  background: linear-gradient(45deg, #2196f3, #e91e63) border-box;
  -webkit-mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: destination-out;
  mask-composite: exclude;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.gradient-hover-btn:hover::before {
  opacity: 1;
}

.gradient-hover-btn.gradient-active {
  background: linear-gradient(45deg, #2196f3, #e91e63);
  -webkit-background-clip: text;
  -webkit-text-fill-color: grey;
  background-clip: text;
  font-weight: bold;
}

.gradient-hover-btn.gradient-active::before {
  opacity: 1;
}

.v-toolbar__title {
  overflow: visible;
}
</style>
