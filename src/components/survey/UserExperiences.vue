<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperience">Load Submitted Experiences</base-button>
      </div>
      <h2 v-if="isLoading && !error">Loading...</h2>
      <h4 v-else-if="!isLoading && error">{{ error }}</h4>
      <h4 v-else-if="!isLoading && (!results || results.length === 0)">No Stored Experiences Found! Start Adding Some Servay Results First.</h4>
      <ul v-else>
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
  // props: ['results'],
  data() {
    return {
      results: [],
      isLoading: false,
      error: null
    }
  },
  methods: {
    loadExperience() {
      this.isLoading = true;
      this.error = null;
      this.results = [];
      fetch('https://vue-http-demo-a1c6b-default-rtdb.firebaseio.com/surveys.json')
        .then(res => {
          if(res.ok) {
            return res.json();
          }
        })
        .then(data => {
          // const results = [];
          this.isLoading = false;

          for(const id in data) {
            this.results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating
            })
          }
          // this.results = results;
        })
        .catch(err => {
          this.isLoading = false;
          this.error = 'Failed To Fetch Data - Please Try Again Later';
          console.log(err);
        });
    },
  },
  components: {
    SurveyResult,
  },
  mounted() {
    this.loadExperience();
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