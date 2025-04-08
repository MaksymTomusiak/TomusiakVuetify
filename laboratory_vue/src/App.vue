<script setup lang="ts">
import { ref } from 'vue'

const drawer = ref(false)
const items = ref([
  { title: 'List', icon: 'mdi-view-dashboard', to: '/' },
  { title: 'About', icon: 'mdi-help-box', to: 'about' },
])
</script>

<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <v-list-item title="My Custom App" subtitle="Awesome Vuetify"></v-list-item>
      <v-divider></v-divider>
      <v-list>
        <v-list-item
            v-for="(item, i) in items"
            :key="i"
            :title="item.title"
            :prepend-icon="item.icon"
            :to="item.to"
            link
        ></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
        color="teal-darken-4"
        image="https://picsum.photos/1920/1080?random"
        app
    >
      <template v-slot:prepend>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      </template>

      <v-app-bar-title>My vue app</v-app-bar-title>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>