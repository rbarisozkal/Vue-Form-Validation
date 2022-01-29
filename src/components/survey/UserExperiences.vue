<template>
  <section>
    <base-card>
      <h2>Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >List Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoaded">Loading Data</p>
      <p v-else-if="!isLoaded && (!results || results.length === 0)">Data not found</p>
      <ul v-else-if="!isLoaded && results && results.length > 0">
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
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoaded: false,
    };
  },
  methods: {
    loadExperiences() {
      this.isLoaded = true;
      fetch(`https://vueproject-5a032-default-rtdb.firebaseio.com/surveys.json`)
        .then((res) => {
          this.isLoaded = false;
          if (res.ok) {
            return res.json();
          }
        })
        .then((data) => {
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].userName,
              rating: data[id].rating,
            });
          }
          this.results = results;
        });
    },
  },
  created() {
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
