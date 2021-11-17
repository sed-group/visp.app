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
      <template v-slot:extension>
        <v-tabs 
        v-model="tab" 
        align-with-title
        >
          <v-tab key="Current">
            Current
          </v-tab>
          <v-tab key="Process">
            Process
          </v-tab>
          <v-tab key="Reorder">
            Reorder
          </v-tab>
          <v-tab key="Three">
            Three
          </v-tab>
        </v-tabs>
      </template>
    </v-app-bar>

    <v-main>
      <v-tabs-items v-model="tab">
        <v-tab-item key="Current">
          <Current/>
        </v-tab-item>
        <v-tab-item key="Process">
          <Process/>
        </v-tab-item>
        <v-tab-item key="Reorder">
          <Reorder/>
        </v-tab-item>
        <v-tab-item key="Three">
          <Three/>
        </v-tab-item>
      </v-tabs-items>
      <!--<Reorder/>-->
    </v-main>
  </v-app>
</template>

<script>
import Current from './components/Current';
import Process from './components/Process';
import Reorder from './components/Reorder';
import Three from './components/Three';

export default {
  name: 'App',
  components: {
    Current,
    Process,
    Reorder,
    Three,
  },
  data: () => ({
    //
    tab: null,
  }),
};
</script>
