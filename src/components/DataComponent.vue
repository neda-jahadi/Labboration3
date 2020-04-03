<template>
  <div>
      <h1>Vue code sharing</h1>
        <div class="latestAndReported">
          <div>
              <button  @click="fetchSlowData" :disabled="waitingForData">Latest Snippers!</button>
              <div v-if="errorHappened">
                An error occured, Please try again!
              </div>
              <div v-else-if="dataFromApi">
                 <div class="snipper-box">
                   <SnipperRow v-for="snipper in dataFromApi" :key="snipper.id" :snipper="snipper" />
                     
                 </div><!--snipper-box -->
              </div>

         </div>
          <div>
              <button @click="fetchSlowReports" :disabled="waitingForReport">See all the reported snippets!</button>
              <div v-if="reportError">
                An error occured, Please try again!
              </div>
              <div v-else-if="reportedFromApi">
                <div class="snipper-box">
                  <ReportedRow v-for="reported in reportedFromApi" :key="reported.id" :reported="reported" />
                </div><!--snipper-box-->
                    
              </div> 
           </div>
        </div> <!--latestAndReported-->
        <RegisterSnipp />
  </div>
</template>

<script>
import ReportedRow from './ReportedRow.vue'
import SnipperRow from './SnipperRow.vue'
import RegisterSnipp from './RegisterSnipp.vue';

export default {


    data: ()=>({
        dataFromApi: null,
        reportedFromApi: null,
        url: 'https://www.forverkliga.se/JavaScript/api/api-snippets.php?',
       // selectStatus: 'selected',
        delay:10,
        waitingForData: false,
        errorHappened: false,
        waitingForReport: false,
        reportError: false
    }),
    computed: {
  latestButton(){ if(this.selectStatus=='latest') return 'selected'; else return '';}

  },
  components: {
      RegisterSnipp,
      SnipperRow,
      ReportedRow
      },

  methods: {
      fetchSlowData(){
          this.waitingForData=true;
          this.errorHappened=false;
          setTimeout(this.fetchDataForReal, this.delay);
      },
	async fetchDataForReal() {

		try {
            //this.selectStatus='latest';
			let response = await this.$http.get(this.url+'latest');
            this.dataFromApi = response.data;

		} catch(error) {
            console.log('something went wrong with getting data', error);
             this.errorHappened=true;
            }finally {
				this.waitingForData = false;
            }
    },
          fetchSlowReports(){
             this.waitingForReport=true;
             this.reportError=false;
             setTimeout(this.fetchReportsForReal, this.delay);
          },
    async fetchReportsForReal(){
        try {
			let response = await this.$http.get(this.url+'reported');
            this.reportedFromApi = response.data;

		} catch(error) {
            console.log('something went wrong with getting reports', error);
            this.reportError=true;
            }finally{
              this.waitingForReport =false;
            }
    }
}


}
</script>

<style scoped>
button {
  padding: 0.5em;
}
.showApiResult {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 0.2em;
}
.showApiResult > div {
    margin: 0.5em;
}
 /* .selected {
  background: green;
  color: whitesmoke;
}*/
.snipper-box {
  border: 1px solid gray;
  border-radius: 5px;
  background-color: lightgray;

  margin: 0.2em;
}
.latestAndReported{
  display: grid;
  grid-template-columns: 1fr 1fr;
}
.latestAndReported > div {
  margin: 0.5em;
}
</style>
