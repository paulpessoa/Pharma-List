<template>
  <v-app>
    <v-app-bar
      app
      color="#fff"
      elevation="0"
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mx-4"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
      <h1>Company</h1>
      </div>

      <v-spacer></v-spacer>

      <v-btn
      class="my-8 light-blue lighten-4"  href="https://github.com/paulpessoa/pharma-list" target="_blank" icon x-large>
        <v-icon medium>mdi-account</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main color="#f0f0f0">
      <router-view/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'App',

  data: () => ({
    //
  }),
});
</script>
