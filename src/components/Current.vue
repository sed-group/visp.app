<template>
  <v-container fluid>
    <v-col>
      <v-row dense>
        <v-col cols="4">
          <v-card class="mb-3">
            <v-card-title>Product</v-card-title>
            <v-card-subtitle>Basic information about the product</v-card-subtitle>
            <v-card-text>
              <v-textarea
                outlined
                label="Description"
                value="The Woodman set to work at once, and so sharp was his axe that the tree was soon chopped nearly through."
              ></v-textarea>
            </v-card-text>
          </v-card>
          <v-card class="mb-3">
            <v-card-title>Stakeholders</v-card-title>
            <v-card-subtitle>Expectations and needs</v-card-subtitle>
            <v-card-text>
              Here goes stuff
            </v-card-text>
          </v-card>
          <v-card class="mb-3">
            <v-card-title>VCS</v-card-title>
            <v-card-subtitle>Value Creation Strategies</v-card-subtitle>
            <v-card-text>
              <v-list dense>
                <v-list-item-group>
                  <v-list-item>One</v-list-item>
                  <v-list-item>Two</v-list-item>
                  <v-list-item>Three</v-list-item>
                </v-list-item-group>
              </v-list>
            </v-card-text>
          </v-card>
          <v-card class="mb-3">
            <v-card-title>Value Drivers</v-card-title>
            <v-card-text>
              <v-list dense>
                <v-list-item-group>
                  <v-list-item>One</v-list-item>
                  <v-list-item>Two</v-list-item>
                  <v-list-item>Three</v-list-item>
                </v-list-item-group>
              </v-list>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="4">
          <v-card class="mb-3">
            <v-card-title>Functional structure</v-card-title>
            <v-card-subtitle>Enhanced Funtion-Means method</v-card-subtitle>
            <v-card-text>
              (Hierarchy construction interface)
              <v-treeview
                v-model="tree"
                :open="open"
                :items="items"
                activatable
                item-key="name"
              >
                <template v-slot:prepend="{ item, open }">
                  <v-icon v-if="!item.type">
                    {{ open ? 'mdi-folder-open' : 'mdi-folder' }}
                  </v-icon>
                  <v-icon v-else>
                    {{ types[item.type] }}
                  </v-icon>
                </template>
              </v-treeview>
              Generate alternative solutions
              <br/>
              Select preferred solutions
            </v-card-text>
          </v-card>
          <v-card class="mb-3">
            <v-card-title>Model</v-card-title>
            <v-card-subtitle>Parameters</v-card-subtitle>
            <v-card-text>
              This is a system of partial abstractions used as a cognitive tool for a certain purpose.
              <v-skeleton-loader
                class="mx-auto"
                type="image"
              ></v-skeleton-loader>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="4">
          <v-card class="mb-3">
            <v-card-title>Visualization</v-card-title>
            <v-card-subtitle>Value impact</v-card-subtitle>
            <v-card-text>
              <VueApexCharts width="380" type="radar" :options="options" :series="series" :labels="labels"></VueApexCharts>
              <v-sparkline
                :value="value"
                :gradient="gradient"
                :smooth="radius || false"
                :padding="padding"
                :line-width="width"
                :stroke-linecap="lineCap"
                :gradient-direction="gradientDirection"
                :fill="fill"
                :type="type"
                :auto-line-width="autoLineWidth"
                auto-draw
              ></v-sparkline>
              Volume
              <v-progress-linear
                color="light-blue"
                height="10"
                value="50"
                striped
              ></v-progress-linear>
              Flexibility
              <v-progress-linear
                color="light-green"
                height="10"
                value="60"
                striped
              ></v-progress-linear>
              Perceived quality
              <v-progress-linear
                color="red"
                height="10"
                value="70"
                striped
              ></v-progress-linear>
              Alternative ranking?
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'

const gradients = [
  ['#222'],
  ['#42b3f4'],
  ['red', 'orange', 'yellow'],
  ['purple', 'violet'],
  ['#00c6ff', '#F0F', '#FF0'],
  ['#f72047', '#ffd200', '#1feaea'],
]
export default {
  name: "current",
  display: "Current",
  components: {
    VueApexCharts,
  },
  data: () => ({
    width: 2,
    radius: 10,
    padding: 8,
    lineCap: 'round',
    gradient: gradients[5],
    value: [0, 2, 5, 9, 5, 10, 3, 5, 0, 0, 1, 8, 2, 9, 0],
    gradientDirection: 'top',
    gradients,
    fill: false,
    type: 'trend',
    autoLineWidth: false,
    options: {},
    series: [
      {
        name: "Radar Series 1",
        data: [45, 52, 38, 24, 33, 10]
      },
      {
        name: "Radar Series 2",
        data: [26, 21, 20, 6, 8, 15]
      }
    ],
    labels: ['April', 'May', 'June', 'July', 'August', 'September'],    
    open: [],
    types: {
      fr: 'mdi-function',
      ds: 'mdi-cogs',
    },
    tree: [],
    items: [
      {
        name: 'Move cursor on the screen',
        type: 'fr',
        children: [
          {
            name: 'Mouse',
            type: 'ds',
            children: [{
              name: 'Connect to computer',
              type: 'fr',
            }],
          },
        ],
      },
    ],
  }),
  methods: {
  },
};
</script>
<style scoped>
</style>