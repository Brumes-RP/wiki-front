<template>
  <v-app dark>
    <navigation-drawer />
    <v-app-bar
      :src="require('../assets/images/wallpapers/ESO_Morrowind_keyart.jpg')"
      color="#563028"
      fade-img-on-scroll
      shrink-on-scroll
      clipped-left
      prominent
      fixed
      app
    >
      <template v-slot:img="{ props }">
        <v-img
          v-bind="props"
          gradient="to top right, rgba(86, 48, 40, 0.8), rgba(128, 208, 199, .2)"
        />
      </template>
      <v-app-bar-nav-icon @click.stop="handleDrawer" />
      <v-toolbar-title class="pl-3">
        <h1 class="title">
          {{ appTitle }}
          <span class="subtitle-1 hidden-sm-and-down">
            - {{ $store.state.pageTitle }}
          </span>
        </h1>
      </v-toolbar-title>
      <v-spacer />
      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
      <side-menu />
      <template v-slot:extension>
        <v-tabs
          align-with-title
          background-color="transparent"
        >
          <v-tab>Site web</v-tab>
          <v-tab>Infos légales</v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <v-content>
      <v-container fill-height>
        <v-layout column>
          <breadcrumb />
          <nuxt />
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Breadcrumb from './components/Breadcrumb'
import SideMenu from './components/SideMenu'
import NavigationDrawer from './components/NavigationDrawer'

export default {
  name: 'MainLayout',

  components: {
    Breadcrumb,
    SideMenu,
    NavigationDrawer
  },

  data: () => ({
    appTitle: 'The Elder Scrolls'
  }),

  methods: {
    handleDrawer () {
      this.$store.commit('TOGGLE_IS_DRAWER_OPEN', !this.$store.state.isDrawerOpen)
    }
  }
}
</script>