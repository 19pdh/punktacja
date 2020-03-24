<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="600px">
      <template v-slot:activator="{ on }">
        <v-btn color="primary" dark v-on="on">Zaloguj</v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="headline">Podaj hasło</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="password"
                  label="Hasło"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="handleSubmit">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  name: 'GetPassword',
  data: () => ({
    dialog: false,
    password: '',
  }),
  methods: {
    handleSubmit() {
      localStorage.setItem('password', this.password);
      console.log(this.password);
      this.dialog = false;
    },
  },
};
</script>
