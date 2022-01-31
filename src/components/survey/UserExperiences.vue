<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>

      <ul v-else-if="!isLoading && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>

      <p v-else-if="!isLoading && error">
        {{ error }}
      </p>

      <p v-else>
        No stored experiences found. Start adding some survey results!
      </p>
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
      isLoading: false,
      error: null,
    };
  },
  methods: {
    setData(rawData) {
      if (!rawData) {
        this.isLoading = false;
        return;
      }

      this.results = Object.values(rawData);
      this.isLoading = false;
    },
    loadExperiences() {
      this.isLoading = true;
      this.error = null;

      axios
        .get(URL)
        .then(({ data }) => this.setData(data))
        .catch((e) => {
          console.log(e);
          this.isLoading = false;
          this.error = 'Failed to fetch data... please try again later.';
        });
    },
  },
  mounted() {
    this.loadExperiences();
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
