<template>
  <v-container>
    <v-col cols="12" sm="6" md="4">
      <v-data-table
        v-model="selected"
        :headers="headers"
        :items="nodes"
        item-key="name"
        sort-by="description"
        class="elevation-1"
        show-select
        single-select
        dense
      >
        <template v-slot:top>
          <v-toolbar flat color="white">
            <v-toolbar-title>Nodes</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <template v-slot:activator="{ on }">
                <v-btn color="primary" dark class="mb-2" v-on="on">New Node</v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedNode.name" label="Name"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedNode.description" label="Description"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedNode.type" label="Type"></v-text-field>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                  <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template v-slot:item.action="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editNode(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            @click="deleteNode(item)"
          >
            mdi-delete
          </v-icon>
        </template>
        <template v-slot:no-data>
          <v-btn color="primary" @click="initialize">Reset</v-btn>
        </template>
        <template v-slot:item.type="{ item }">
          <v-chip x-small :color="getColor(item.type)" dark>{{ item.type }}</v-chip>
        </template>
      </v-data-table>
    </v-col>
  </v-container>
</template>

<script>
export default {
  name: "efm",
  display: "EFM",
  order: 8,
  components: {
  },
  data: () => ({
    selected: [],
    dialog: false,
    headers: [
      {
        text: 'Name',
        align: 'start',
        value: 'name',
      },
      { 
        text: 'Type', 
        align: 'right',
        value: 'type' 
      },
      { 
        text: 'Actions', 
        align: 'end', 
        value: 'action', 
        sortable: false 
      },
    ],
    nodes: [],
    editedIndex: -1,
    editedNode: {
      name: '',
      type: '',
      description: '',
    },
    defaultNode: {
      name: '',
      type: '',
      description: '',
    },
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Node' : 'Edit Node'
    },
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
  },

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      this.nodes = [
        {
          name: 'Show info to driver',
          description: 'Information superimposed on the view of the road from the driver\'s perspective.',
          type: 'FR',
        },
        {
          name: 'HUD',
          description: 'Head-Up Display',
          type: 'DS',
        },
        {
          name: 'Project picture',
          description: '',
          type: 'FR',
        },
        {
          name: 'Reflect picture towards driver',
          description: '',
          type: 'FR',
        },
        {
          name: 'Contain parts',
          description: '',
          type: 'FR',
        },
        {
          name: 'Reduce glare',
          description: '',
          type: 'FR',
        },
        {
          name: 'Block dust ingress',
          description: '',
          type: 'FR',
        },
        {
          name: 'Reduce picture issues',
          description: '',
          type: 'FR',
        },
        {
          name: 'Collimate light',
          description: '',
          type: 'FR',
        },
        {
          name: 'Receive information',
          description: '',
          type: 'FR',
        },
      ]
    },

    editNode (node) {
      this.editedIndex = this.nodes.indexOf(node)
      this.editedNode = Object.assign({}, node)
      this.dialog = true
    },

    deleteNode (node) {
      const index = this.nodes.indexOf(node)
      confirm('Are you sure you want to delete this node?') && this.nodes.splice(index, 1)
    },

    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedNode = Object.assign({}, this.defaultNode)
        this.editedIndex = -1
      }, 300)
    },

    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.nodes[this.editedIndex], this.editedNode)
      } else {
        this.nodes.push(this.editedNode)
      }
      this.close()
    },

    getColor (type) {
      if (type == 'DS') return 'orange'
      else if (type == 'FR') return 'blue'
      else if (type == 'C') return 'black'
      else if (type == 'CC') return 'grey'
      else return 'red'
    },
  },

};
</script>
<style scoped>
</style>