<template>
  <v-app>
    <v-content>
      <v-toolbar app dark class="indigo">
        <v-toolbar-side-icon @click="drawer = !drawer"></v-toolbar-side-icon>
        <v-toolbar-title>Jaula inteligente</v-toolbar-title>
      </v-toolbar>
      <v-navigation-drawer app v-model="drawer" class="indigo">
        <p>test</p>
      </v-navigation-drawer>
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data () {
    return {
      drawer: false
    }
  }
}
</script>
