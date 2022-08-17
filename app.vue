<template>
  <v-app id="inspire">
    <!-- <v-navigation-drawer
      v-model="drawer"
      app
    >
      <v-list
        dense
        nav
      >
        <v-list-item
          v-for="item in items"
          :key="item.title"
          nuxt
          :to="item.link"
        >
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer> -->

    <v-app-bar app>
      <!-- <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon> -->

      <v-toolbar-title>Fire Starter</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <NuxtPage />
    </v-main>
  </v-app>
</template>

<script setup>
    // const drawer = ref(true);
    const items = [
          { title: 'top', icon: 'mdi-view-dashboard', link: '/'},
          { title: 'About', icon: 'mdi-help-box', link: 'about'},
          { title: 'Test', icon: 'mdi-help-box', link: 'test'}
        ]
</script>