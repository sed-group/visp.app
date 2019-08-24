<template>
  <v-container>
    <v-col>
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