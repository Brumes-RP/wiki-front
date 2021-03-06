<template>
  <v-app dark>
    <div v-if="loading">
      Chargement du thème...
    </div>
    <div v-else>
      <NavigationDrawer />
      <v-app-bar
        :src="theme ? theme.cover.url : undefined"
        color="primary darken-1"
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
            gradient="to top, rgba(31, 31, 31, .8) 25%, rgba(48, 48, 48, .2)"
          />
        </template>
        <v-app-bar-nav-icon @click.stop="handleDrawer" />
        <v-toolbar-title class="pl-3">
          <h1 class="title">
            {{ appTitle ? appTitle : '...' }}
            <span class="subtitle-1 hidden-sm-and-down">
              - {{ $store.state.pageTitle }}
            </span>
          </h1>
        </v-toolbar-title>
        <v-spacer />
        <side-menu v-if="isAuthenticated" />
        <v-btn v-else @click="handleLogin" icon>
          <v-icon>
            mdi-account
          </v-icon>
        </v-btn>
      <!-- <template v-slot:extension>
        <v-tabs
          align-with-title
          background-color="transparent"
        >
          <v-tab>Site web</v-tab>
          <v-tab>Infos légales</v-tab>
        </v-tabs>
      </template> -->
      </v-app-bar>
      <v-content>
        <v-container>
          <v-layout column>
            <Breadcrumb />
            <nuxt />
            <Notifications />
          </v-layout>
        </v-container>
      </v-content>
    </div>
  </v-app>
</template>

<script>
import { mapState } from 'vuex'

import Breadcrumb from './components/Breadcrumb'
import SideMenu from './components/SideMenu'
import Notifications from './components/Notifications'
import NavigationDrawer from './components/NavigationDrawer'
import pkg from '@/package.json'

export default {
  name: 'MainLayout',

  components: {
    Breadcrumb,
    SideMenu,
    NavigationDrawer,
    Notifications
  },

  data: () => ({
    loading: true,
    theme: ''
  }),

  computed: {
    ...mapState({
      isAuthenticated: state => state.auth.isAuthenticated,
      user: state => state.auth.session.user
    }),

    appTitle () {
      return this.theme.title
    }
  },

  watch: {
    // Define the global theme
    theme (oldTheme, newTheme) {
      if (this.theme.primary) { this.$vuetify.theme.themes.dark.primary = this.theme.primary }
      if (this.theme.secondary) { this.$vuetify.theme.themes.dark.secondary = this.theme.secondary }
      if (this.theme.accent) { this.$vuetify.theme.themes.dark.accent = this.theme.accent }
      if (this.theme.info) { this.$vuetify.theme.themes.dark.info = this.theme.info }
      if (this.theme.warning) { this.$vuetify.theme.themes.dark.warning = this.theme.warning }
      if (this.theme.error) { this.$vuetify.theme.themes.dark.error = this.theme.error }
    }

  },

  beforeMount () {
    this.fetchTheme()
  },

  methods: {
    handleDrawer () {
      this.$store.commit('TOGGLE_IS_DRAWER_OPEN', !this.$store.state.isDrawerOpen)
    },

    handleLogin () {
      this.$router.push('/login')
    },

    async fetchTheme () {
      await this.$axios
        .get('/themes', {
          params: {
            'game.title': pkg.targetDomain
          }
        })
        .then((response) => {
          this.theme = response.data[0]
        })
      this.loading = false
    }
  }
}
</script>
