<template>
  <v-container>
    <v-col>
      <h2>Summary of the process</h2>
      <h3>Stakeholder expectations and needs</h3>
      <p>Both internal and external stakeholders are considered when collecting expectations, that are translated into needs.</p>
      <h3>Value drivers</h3>
      <p>The key engineering parameters that will influence the fulfilment of the needs are identified.</p>
      <h3>Value Creation Strategies</h3>
      <p>The ranking of the value drivers at different levels and form diffferent perspectives provides a clear view of the priorites to be pursued to maximize value.</p>
      <h3>Functional structure</h3>
      <p>The Enhanced Function-Means method is used to decompose the product into its functions and alternative means for fulfilling them.</p>
      <h3>Link between design solutions and value drivers</h3>
      <p>The value drivers are linked to the parameters of the design solutions and constraints represented in the EF-M graph.</p>
      <h3>Generation of alternative solutions</h3>
      <p>The combinatorial problem is solved, eliminating the branches that aren't possible.</p>
      <h3>Analysis of alternative solutions in the context of the VCSs</h3>
      <p>Each of the alternative solutions is evaluated versus its potential to contribute to each of the VCS.</p>
      <h3>Decision support system</h3>
      <p>The most promising alternatives are presented to the decision maker, including the interactive means to explore the effect of changes on both the external inputs as well as the structure itself.</p>
      <h2>Components under development</h2>
      <h3>Drag and drop to order</h3>

      <v-simple-table>
        <thead>
          <tr>
            <th class="text-left">Name</th>
            <th class="text-left">Description</th>
            <th class="text-left">Position</th>
          </tr>
        </thead>
        <draggable v-model="list" tag="tbody" @end="onEnd">
          <tr v-for="item in list" :key="item.name">
            <td>{{ item.name }}</td>
            <td>{{ item.description }}</td>
            <td>{{ list.indexOf(item) }}</td>
          </tr>
        </draggable>
      </v-simple-table>
    </v-col>
  </v-container>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: "table-example",
  display: "Table",
  order: 8,
  components: {
    draggable
  },
  data() {
    return {
      list: [
        { id: 1, name: "Cost", description: "How much does it cost to produce the item." },
        { id: 2, name: "Performance", description: "How well does it perform its main function." },
        { id: 3, name: "Reliability", description: "How reliable it is." },
        { id: 4, name: "Maintainability", description: "How easy to maintain it is." }
      ],
      dragging: false
    };
  },
  methods: {
    onEnd: function (event) {
      // now we have access to the native event
      console.log(event)
      console.log(event.item.cells["0"].innerText)
      console.log(this.list)
    }
  },
};
</script>
<style scoped>
.buttons {
  margin-top: 35px;
}
</style>