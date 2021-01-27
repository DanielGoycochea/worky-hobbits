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
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
