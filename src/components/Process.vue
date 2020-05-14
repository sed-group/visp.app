<template>
  <v-stepper v-model="e1" vertical>
    <v-stepper-step
      editable
      :complete="e1 > 1"
      step="1"
    >Capture stakeholders' expectations and needs</v-stepper-step>

    <v-stepper-content step="1">
      <v-data-table :headers="headers" :items="needs" sort-by="description" class="elevation-1">
        <template v-slot:top>
          <v-toolbar flat color="white">
            <v-toolbar-title>Needs</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <template v-slot:activator="{ on }">
                <v-btn color="primary" dark class="mb-2" v-on="on">New Need</v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12" sm="6" md="8">
                        <v-select
                          v-model="editedItem.type"
                          :items="needTypes"
                          label="Type"
                          hint="In relation to the firm"
                        ></v-select>
                      </v-col>
                      <v-col cols="12" sm="6" md="8">
                        <v-text-field
                          v-model="editedItem.stakeholder"
                          label="Stakeholder"
                          hint="Who has this need?"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="8">
                        <v-text-field
                          v-model="editedItem.needName"
                          label="Need name"
                          hint="Sucint name"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="8">
                        <v-text-field
                          v-model="editedItem.description"
                          label="Description"
                          hint="Explanation/Definition of the need"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="8">
                        <v-text-field
                          v-model="editedItem.consequences"
                          label="Consequences"
                          hint="What are the concequences if we are not being able to satisfy this stakeholder need? Please explain"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="8">
                        <v-text-field
                          v-model="editedItem.examples"
                          label="Examples"
                          hint="Please give a concrete example"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="8">
                        <v-select
                          v-model="editedItem.engineeringAspects"
                          :items="valueDrivers"
                          item-text="name"
                          item-value="name"
                          :menu-props="{ maxHeight: '400' }"
                          label="Select"
                          multiple
                          hint="What engineering aspects impact this need - that you can control during design? Please write them, providing a unit of measurement between parenthesis"
                          persistent-hint
                        ></v-select>
                      </v-col>
                      <v-col cols="12" sm="6" md="8">
                        <v-select
                          v-model="editedItem.processName"
                          :items="systemLifeCycleProcesses"
                          item-text="processName"
                          item-value="processName"
                          label="Phase"
                          hint="What life cycle process is affected?"
                        ></v-select>
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
        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">mdi-pencil</v-icon>
          <v-icon small @click="deleteItem(item)">mdi-delete</v-icon>
        </template>
        <template v-slot:no-data>
          <v-btn color="primary" @click="initialize">Reset</v-btn>
        </template>
      </v-data-table>
    </v-stepper-content>

    <v-stepper-step editable :complete="e1 > 2" step="2">Value Drivers</v-stepper-step>

    <v-stepper-content step="2">
      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Need</th>
              <th class="text-left">Value Drivers</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in needs" :key="item.needName">
              <td>{{ item.needName }}</td>
              <td>{{ item.engineeringAspects }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-stepper-content>

    <v-stepper-step editable :complete="e1 > 3" step="3">Value Creation Strategies</v-stepper-step>

    <v-stepper-content step="3">
      <v-container fluid>
        <v-row align="center">
          <v-col cols="6">
            <v-select
              v-model="selectedVCS"
              :hint="`${selectedVCS.name} ${selectedVCS.description}`"
              :items="valueCreationStrategies"
              item-text="name"
              item-value="id"
              label="Select VCS"
              persistent-hint
              return-object
              single-line
            ></v-select>
          </v-col>
          <v-col cols="6">
            <v-btn color="primary" dark class="mb-2" v-on="on">New VCS</v-btn>
          </v-col>
        </v-row>
      </v-container>
      <h3>Prioritization of needs</h3>
      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Need</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in needs" :key="item.needName">
              <td>{{ item.needName }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-stepper-content>

    <v-stepper-step editable :complete="e1 > 4" step="4">System Life Cycle</v-stepper-step>

    <v-stepper-content step="4">
      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">ISO 15288 Clause</th>
              <th class="text-left">Type</th>
              <th class="text-left">Process name</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in systemLifeCycleProcesses" :key="item.processName">
              <td>{{ item.clause }}</td>
              <td>{{ item.processType }}</td>
              <td>{{ item.processName }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-stepper-content>

    <v-stepper-step editable :complete="e1 > 5" step="5">Qualified Objectives</v-stepper-step>

    <v-stepper-content step="5">
      <h3>TBD</h3>
      <p>TBD</p>
    </v-stepper-content>

    <v-stepper-step editable :complete="e1 > 6" step="6">Value Models</v-stepper-step>

    <v-stepper-content step="6">
      <h3>TBD</h3>
      <p>TBD</p>
    </v-stepper-content>

    <v-stepper-step editable :complete="e1 > 7" step="7">Requirements</v-stepper-step>

    <v-stepper-content step="7">
      <h3>TBD</h3>
      <p>TBD</p>
      <h3>Value drivers</h3>
      <p>
        The key engineering parameters that will influence the fulfilment
        of the needs are identified.
      </p>
      <h3>Value Creation Strategies</h3>
      <p>
        The ranking of the value drivers at different levels and from
        different perspectives provides a clear view of the priorites to
        be pursued to maximize value.
      </p>
      <h3>Functional structure</h3>
      <p>
        The Enhanced Function-Means method is used to decompose the
        product into its functions and alternative means for fulfilling
        them.
      </p>
      <h3>Link between design solutions and value drivers</h3>
      <p>
        The value drivers are linked to the parameters of the design
        solutions and constraints represented in the EF-M graph.
      </p>
      <h3>Generation of alternative solutions</h3>
      <p>
        The combinatorial problem is solved, eliminating the branches that
        aren't possible.
      </p>
      <h3>Analysis of alternative solutions in the context of the VCSs</h3>
      <p>
        Each of the alternative solutions is evaluated versus its
        potential to contribute to each of the VCS.
      </p>
      <h3>Decision support system</h3>
      <p>
        The most promising alternatives are presented to the decision
        maker, including the interactive means to explore the effect of
        changes on both the external inputs as well as the structure
        itself.
      </p>

      <v-btn color="primary" @click="e1 = 7">Continue</v-btn>

      <v-btn text @click="e1 = 1">Cancel</v-btn>
    </v-stepper-content>
  </v-stepper>
</template>

<script>
export default {
  name: "process",
  display: "Process",
  order: 8,
  components: {},
  data: () => ({
    e1: 1,
    valueCreationStrategies: [
      { id: 1, name: "Big GTO", description: "15 years, 1 shot" },
      { id: 2, name: "Several GTO", description: "5 satellites/year" },
      { id: 3, name: "Small GEO", description: "20 satellites/year" },
      { id: 4, name: "TBD", description: "" },
      { id: 5, name: "TBD", description: "" }
    ],
    selectedVCS: { id: 1, name: "", description: "" },
    dialog: false,
    headers: [
      { text: "Type", align: "start", value: "type" },
      { text: "Stakeholder", value: "stakeholder" },
      { text: "Need name", value: "needName" },
      { text: "Description", value: "description" },
      { text: "Consequences", value: "consequences" },
      { text: "Examples", value: "examples" },
      //{ text: "Engineering aspects", value: "engineeringAspects" },
      { text: "Phase", value: "processName" },
      { text: "Actions", value: "actions", sortable: false }
    ],
    needs: [],
    needTypes: ["internal", "external"],
    editedIndex: -1,
    editedItem: {
      type: "",
      stakeholder: "",
      description: ""
    },
    defaultItem: {
      type: "",
      stakeholder: "",
      description: ""
    },
    systemLifeCycleProcesses: [
      {
        clause: "6.1.1",
        processType: "Agreement Processes",
        processName: "Acquisition Process"
      },
      {
        clause: "6.1.2",
        processType: "Agreement Processes",
        processName: "Supply Process"
      },
      {
        clause: "6.2.1",
        processType: "Organizational Project-Enabling Processes",
        processName: "Life Cycle Model Management Process"
      },
      {
        clause: "6.2.2",
        processType: "Organizational Project-Enabling Processes",
        processName: "Infrastructure Management Process"
      },
      {
        clause: "6.2.3",
        processType: "Organizational Project-Enabling Processes",
        processName: "Project Portfolio Management Process"
      },
      {
        clause: "6.2.4",
        processType: "Organizational Project-Enabling Processes",
        processName: "Human Resource Management Process"
      },
      {
        clause: "6.2.5",
        processType: "Organizational Project-Enabling Processes",
        processName: "Quality Management Process"
      },
      {
        clause: "6.3.1",
        processType: "Project Processes",
        processName: "Project Planning Process"
      },
      {
        clause: "6.3.2",
        processType: "Project Processes",
        processName: "Project Assessment and Control Process"
      },
      {
        clause: "6.3.3",
        processType: "Project Processes",
        processName: "Decision Management Process"
      },
      {
        clause: "6.3.4",
        processType: "Project Processes",
        processName: "Risk Management Process"
      },
      {
        clause: "6.3.5",
        processType: "Project Processes",
        processName: "Configuration Management Process"
      },
      {
        clause: "6.3.6",
        processType: "Project Processes",
        processName: "Information Management Process"
      },
      {
        clause: "6.3.7",
        processType: "Project Processes",
        processName: "Measurement Process"
      },
      {
        clause: "6.4.1",
        processType: "Technical Processes",
        processName: "Stakeholder Requirements Definition Process"
      },
      {
        clause: "6.4.2",
        processType: "Technical Processes",
        processName: "Requirements Analysis Process"
      },
      {
        clause: "6.4.3",
        processType: "Technical Processes",
        processName: "Architectural Design Process"
      },
      {
        clause: "6.4.4",
        processType: "Technical Processes",
        processName: "Implementation Process"
      },
      {
        clause: "6.4.5",
        processType: "Technical Processes",
        processName: "Integration Process"
      },
      {
        clause: "6.4.6",
        processType: "Technical Processes",
        processName: "Verification Process"
      },
      {
        clause: "6.4.7",
        processType: "Technical Processes",
        processName: "Transition Process"
      },
      {
        clause: "6.4.8",
        processType: "Technical Processes",
        processName: "Validation Process"
      },
      {
        clause: "6.4.9",
        processType: "Technical Processes",
        processName: "Operation Process"
      },
      {
        clause: "6.4.10",
        processType: "Technical Processes",
        processName: "Maintenance Process"
      },
      {
        clause: "6.4.11",
        processType: "Technical Processes",
        processName: "Disposal Process"
      }
    ],
    valueDrivers: [
      {
        name: "Volume",
        unit: "m^3",
        type: "FR",
        verificationMethod: "CAD Model"
      },
      {
        name: "Dry Mass",
        unit: "kg",
        type: "FR",
        verificationMethod: "CAD Model"
      },
      {
        name: "Propellant Mass",
        unit: "kg",
        type: "FR",
        verificationMethod: "CAD Model"
      },
      {
        name: "Specific Impulse Isp",
        unit: "kg/(N·s)",
        type: "FR",
        verificationMethod: "Simulink/Modelica"
      }
    ]
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Need" : "Edit Need";
    }
  },

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.needs = [
        {
          type: "external",
          stakeholder: "Someone",
          needName: "Launch Mass Reduction",
          description: "The vehicle needs to be light.",
          consequences: "Otherwise more fuel will be needed.",
          examples: "The rocket equation explains it.",
          engineeringAspects: "Mass (kg), Specific trust (N), Drag reduction",
          processName: "Operation Process"
        },
        {
          type: "internal",
          stakeholder: "Someone else",
          needName: "Increased manufacturability",
          description: "The manufacturing process needs to be very lean.",
          consequences: "Profitability decrease.",
          examples: "Higher costs.",
          engineeringAspects: "Volume (m^3), Part complexity",
          processName: "Operation Process"
        }
      ];
    },

    editItem(item) {
      this.editedIndex = this.needs.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      const index = this.needs.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.needs.splice(index, 1);
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.needs[this.editedIndex], this.editedItem);
      } else {
        this.needs.push(this.editedItem);
      }
      this.close();
    }
  }
};
</script>
<style scoped></style>
