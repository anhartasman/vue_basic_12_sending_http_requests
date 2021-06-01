<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <!-- Tambahkan isLoading -->
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">
        {{ error }}
      </p>
      <p v-else-if="!isLoading &&(!results || results.length===0)">No stored experience found</p>
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
  components: {
    SurveyResult,
  },
  data(){
    return {
      results:[],
      isLoading:false,
      error:null
    }
  },
  methods:{
    loadExperiences(){
      this.isLoading=true;
      this.error=null;
      // hapus .json dari URL untuk percobaan error
      fetch('https://vue-basic-12-http-default-rtdb.firebaseio.com/surveys.json').then((response)=>{
        if(response.ok){
          return response.json();
        }
      }).then((data)=>{
      this.isLoading=false;
        console.log(data);
        const results=[];
        for(const id in data){
          results.push({id:id,name:data[id].name,rating:data[id].rating});
          
        }
        this.results=results;
      }).catch((error)=>{
        this.isLoading=false;
        this.error='Failed to fetch data - please try again later.';
        console.log(error);
      });
    }
  },
  mounted(){
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