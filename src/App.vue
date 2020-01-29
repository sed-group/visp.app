<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>VISP</span>
        <span class="font-weight-light">APP</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text
        href="/about"
      >
        <span class="mr-2">About</span>
      </v-btn>
      <v-btn
        text
        href="https://github.com/sed-group/visp.app"
        target="_blank"
      >
        <span class="mr-2">Github</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <Reorder/>
    </v-content>
  </v-app>
</template>

<script>
import Reorder from './components/Reorder';

export default {
  name: 'App',
  components: {
    Reorder,
  },
  data: () => ({
    //
  }),
};
</script>
