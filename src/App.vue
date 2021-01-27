<template>
  <v-app>
    <v-app-bar
      app
      color="#0093FC"
      dark
    >
      <div class="d-flex align-center">
        <h1 class="mr-2">WORKY</h1>
      </div>
    </v-app-bar>
    <v-main>
      <home/>
    </v-main>
  </v-app>
</template>

<script>
import Home from './components/Home';

export default {
  name: 'App',

  components: {
    Home,
  },

  data: () => ({
    //
  }),
};
</script>
