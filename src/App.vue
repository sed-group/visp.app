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
          <v-tab key="EFM">
            EFM
          </v-tab>
          <v-tab key="Process">
            Process
          </v-tab>
          <v-tab key="Reorder">
            Reorder
          </v-tab>
        </v-tabs>
      </template>
    </v-app-bar>

    <v-content>
      <v-tabs-items v-model="tab">
        <v-tab-item key="Current">
          <Current/>
        </v-tab-item>
        <v-tab-item key="EFM">
          <EFM/>
        </v-tab-item>
        <v-tab-item key="Process">
          <Process/>
        </v-tab-item>
        <v-tab-item key="Reorder">
          <Reorder/>
        </v-tab-item>
      </v-tabs-items>
      <!--<Reorder/>-->
    </v-content>
  </v-app>
</template>

<script>
import Current from './components/Current';
import EFM from './components/EFM';
import Process from './components/Process';
import Reorder from './components/Reorder';

export default {
  name: 'App',
  components: {
    Current,
    EFM,
    Process,
    Reorder,
  },
  data: () => ({
    //
    tab: null,
  }),
};
</script>
