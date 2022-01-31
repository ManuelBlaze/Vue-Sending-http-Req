<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <ul>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import axios from 'axios';

import SurveyResult from './SurveyResult.vue';

/**
 * firebase DB url
 * @constant
 * @type {String}
 */
const URL =
  'https://vue-http-demo-4ab69-default-rtdb.firebaseio.com/surveys.json';

export default {
  // props: ['results'],
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
    };
  },
  methods: {
    setData(rawData) {
      this.results = Object.values(rawData);
    },
    loadExperiences() {
      axios
        .get(URL)
        .then(({ data }) => this.setData(data))
        .catch((e) => console.log(e));
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
