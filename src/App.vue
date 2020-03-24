<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        Punktacja
      </div>

      <v-spacer></v-spacer>

      <v-btn href="https://puszcza.netlify.com/punktacja" target="_blank" text>
        <span class="mr-2">Zobacz punktacje</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-content>
      <GetPassword />
      <Punktacja />
    </v-content>
  </v-app>
</template>

<script>
import GetPassword from './components/GetPassword';
import Punktacja from './components/Punktacja';

export default {
  name: 'App',

  components: {
    GetPassword,
    Punktacja,
  },

  data: () => ({
    //
  }),
};
</script>
