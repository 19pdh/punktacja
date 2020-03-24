<template>
  <div>
    <v-data-table :headers="headers" :items="points">
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
    </v-data-table>

    <v-snackbar v-model="snack" :timeout="3000" :color="snackColor">
      {{ snackText }}
      <v-btn text @click="snack = false">Close</v-btn>
    </v-snackbar>
  </div>
</template>

<script>
const parseToken = (token) => Buffer.from(token + ':').toString('base64');

const runQuery = async (query, token) => {
  const options = {
    method: 'POST',
    headers: {
      Authorization: `Basic ${token}`,
    },
    body: JSON.stringify({ query }),
  };
  const response = await fetch(URL, options);
  const data = await response.json();
  return data;
};

const getQuery = `
 {
    getPoints{data {_id points troop{_id name}}}

}`;
const URL = 'https://graphql.fauna.com/graphql';

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
          value: 'troop',
        },
        { text: 'Punkty', value: 'points' },
      ],
      points: [],
      dbCopy: {},
    };
  },
  mounted() {
    this.getPoints();
  },
  methods: {
    success() {
      this.snack = true;
      this.snackColor = 'success';
      this.snackText = 'Data saved';
    },
    save() {
      this.points.forEach(({ _id, points }) =>
        points == this.dbCopy[_id] ? null : this.updatePoints(_id, points)
      );
    },
    error() {
      this.snack = true;
      this.snackColor = 'error';
      this.snackText = 'Error';
    },
    close() {
      console.log('Dialog closed');
    },
    async getPoints() {
      const token = parseToken(localStorage.getItem('password')) || '';
      const { data } = await runQuery(getQuery, token);
      this.points = data.getPoints.data.map(({ _id, troop, points }) => ({
        _id,
        _troopId: troop._id,
        points,
        troop: troop.name,
      }));
      this.dbCopy = this.points.reduce(
        (acc, { _id, _troopId, points }) => ({
          [_id]: { points, _troopId },
          ...acc,
        }),
        {}
      );
    },
    async updatePoints(id, points) {
      const token = parseToken(localStorage.getItem('password')) || '';
      const updateQuery = `mutation {
updatePoints(id: "${id}", data: {
    points: ${points}
    troop: {
      connect: ${this.dbCopy[id]._troopId}
    }
  }) { _id }
}`;
      try {
        const { data } = await runQuery(updateQuery, token);
        if (!data.updatePoints) throw 'Error';
        this.success();
      } catch (e) {
        this.error();
      }
    },
  },
};
</script>
