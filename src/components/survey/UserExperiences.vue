<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <!-- adding isLoading while we wait for the server to respond -->
      <p v-if="isLoading">Loading...</p>
      <!-- showing a database error -->
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <!-- telling the user there is no data at this current time -->
      <p v-else-if="!loading && (!results || results.length === 0)">No Data as yet</p>
      <!-- checking if we are not loading and that we are actually getting data back -->
      <ul v-else-if="!loading && results && results.length > 0">
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
  // we want to GET info from the db to display in this component
  data() {
    return {
      results: [],
      isLoading: false,
      error: null
    }
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      this.error = null;
      let vm = this;
      fetch('https://vue-http-demo-4a081-default-rtdb.firebaseio.com/surveys.json')
        .then(function(response) {
          if (response.ok) {
            return response.json();
          }
        })
        .then(function(data) {
          vm.isLoading = false
          // looping through the data received and generating an object for every id in data
          const results = [];
          for (const id in data) {
            results.unshift({ 
              id: id, 
              name: data[id].name, 
              rating: data[id].rating
            });
          }
          vm.results = results;
        })
        .catch((error) => {
          console.log(error);
          this.isLoading = false;
          this.error = 'Failed to fetch Data - please try again later'
        })
      }
  },
  // loading the existing experiences so we don't have to click the button
  mounted() {
    //  NOTE: we need to run the method with () not just this.loadExperiences
    this.loadExperiences();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>