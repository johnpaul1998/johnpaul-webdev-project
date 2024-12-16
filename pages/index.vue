<template>
  <v-container>
    <v-form @submit.prevent="addFood">
      <v-text-field v-model="itemData.name" label="Food" required></v-text-field>
      <v-text-field v-model="itemData.calories" label="Calories" required></v-text-field>
      <v-btn depressed color="success" class="my-4" type="submit">
        {{ editingIndex === null ? 'Add' : 'Update' }}
      </v-btn>
      <v-btn v-if="editingIndex !== null" @click="cancelEdit">Cancel</v-btn>
    </v-form>

    <v-simple-table>
      <thead>
        <tr>
          <th class="text-left">Name</th>
          <th class="text-left">Calories</th>
          <th class="text-left">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in desserts" :key="item.name">
          <td>{{ item.name }}</td>
          <td>{{ item.calories }}</td>
          <td>
            <v-btn depressed color="primary" @click="editFood(index)">Edit</v-btn>
            <v-btn depressed color="error" @click="deleteFood(index)">Delete</v-btn>
          </td>
        </tr>
      </tbody>
    </v-simple-table>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      desserts: [
        { name: "Frozen Yogurt", calories: 159 },
        { name: "Ice cream sandwich", calories: 237 },
      ],
      itemData: { name: "", calories: 0 },
      editingIndex: null,
    };
  },
  methods: {
    addFood() {
      if (this.editingIndex === null) {
        // Add new item
        this.desserts.push({ ...this.itemData });
      } else {
        // Update existing item
        this.$set(this.desserts, this.editingIndex, { ...this.itemData });
      }
      this.resetForm();
    },
    deleteFood(index) {
      this.desserts.splice(index, 1); // Remove item at the given index
      this.resetForm();
    },
    editFood(index) {
      this.editingIndex = index;
      this.itemData = { ...this.desserts[index] }; // Pre-fill the form with item data
    },
    resetForm() {
      this.itemData = { name: "", calories: 0 }; // Reset the form fields
      this.editingIndex = null;
    },
    cancelEdit() {
      this.resetForm(); // Cancel editing and clear the form
    },
  },
};
</script>
