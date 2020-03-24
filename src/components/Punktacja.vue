<template>
  <div>
    <v-data-table :headers="headers" :items="desserts">
      <template v-slot:item.points="props">
        <v-edit-dialog
          :return-value.sync="props.item.points"
          @save="save"
          @cancel="cancel"
        >
          {{ props.item.points }}
          <template v-slot:input>
            <v-text-field
              v-model="props.item.points"
              :rules="[max25chars]"
              label="Edit"
              single-line
              counter
            ></v-text-field>
          </template>
        </v-edit-dialog>
      </template>
      <template v-slot:item.iron="props">
        <v-edit-dialog
          :return-value.sync="props.item.iron"
          large
          persistent
          @save="save"
          @cancel="cancel"
        >
          <div>{{ props.item.iron }}</div>
          <template v-slot:input>
            <div class="mt-4 title">Update Iron</div>
          </template>
          <template v-slot:input>
            <v-text-field
              v-model="props.item.iron"
              :rules="[max25chars]"
              label="Edit"
              single-line
              counter
              autofocus
            ></v-text-field>
          </template>
        </v-edit-dialog>
      </template>
    </v-data-table>

    <v-snackbar v-model="snack" :timeout="3000" :color="snackColor">
      {{ snackText }}
      <v-btn text @click="snack = false">Close</v-btn>
    </v-snackbar>
  </div>
</template>

<script>
export default {
  data() {
    return {
      snack: false,
      snackColor: '',
      snackText: '',
      max25chars: (v) => v.length <= 25 || 'Input too long!',
      pagination: {},
      headers: [
        {
          text: 'Zastęp',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Punkty', value: 'points' },
      ],
      desserts: [
        {
          name: 'Wydry',
          points: 21,
        },
      ],
    };
  },
  methods: {
    save() {
      this.snack = true;
      this.snackColor = 'success';
      this.snackText = 'Data saved';
    },
    cancel() {
      this.snack = true;
      this.snackColor = 'error';
      this.snackText = 'Canceled';
    },
    close() {
      console.log('Dialog closed');
    },
  },
};
</script>
> } } }
