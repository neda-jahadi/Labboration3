<template>
  <div class="snipper">

      <div><span style="font-size:1.2em;"> Title:</span> {{snipper.title}}</div>
       <div><span style="font-size:1.2em;">Content:</span>{{snipper.content}}</div>
      <div class="ranking">
            <div><span style="font-size:1.2em;">Score:</span>{{snipper.score}}</div>
            <input type="range" min="1" max="16" v-model="snipper.score" />
      </div>
      
         <div>
            <div v-if="!reported">
             <button  @click="slowReport" :disabled="deleted || waitingForReport">Report!</button>
            </div>
            <div v-else-if="reported">
              <button @click="slowUnreport" :disabled="waitingForUnreport">Unreport!</button>
            </div>
            <div class="upvote">
              <button  @click="slowUpvote" :disabled="deleted || waitingForUpvote">Upvote!</button>
            </div>
        </div>
        
      
      <div>
      
         <div v-if="!deleted">
            <button @click="slowDelete" :disabled="waitingForDelete">Delete</button>
         </div>
         <div v-else-if="deleted">
            <button :disabled="deleted">Deleted!</button>
         </div>
      </div>
  </div>
</template>

<script>
export default {
    props: {
    snipper: Object(null)
    
    },
    data:()=>({
        url: 'https://www.forverkliga.se/JavaScript/api/api-snippets.php?',
        deleted: false,
        reported: false,
        unreported: false,
        waitingForDelete: false,
        waitingForReport: false,
        waitingForUnreport: false,
        waitingForUpvote: false,
        delay:10
    }),
    methods: {
      slowUpvote(){
        this.waitingForUpvote=true;
        setTimeout(this.upvoteForReal, this.delay);
      },
      async upvoteForReal() {
            await fetch(this.url, {
                 method: 'POST',
                 body: new URLSearchParams('upvote&id=' + this.snipper.id)   
            })
   .then((response) => {
    console.log('upvote response:',response);
    
   })
   .catch((error) => {
      console.log('This error occured:',error);
    })
      this.waitingForUpvote=false;
        },
      slowReport() {
       this.waitingForReport=true;
       setTimeout(this.reportForReal, this.delay);
      },
        async reportForReal() {
            await fetch(this.url, {
                 method: 'POST',
                 body: new URLSearchParams('report&id=' + this.snipper.id)   
            })
   .then((response) => {
    console.log('report response:',response);
    this.reported=true;
   })
   .catch((error) => {
      console.log('This error occured:',error);
    })
     this.waitingForReport=false;
        },
        slowUnreport() {
        this.waitingForUnreport=true;
        setTimeout(this.unreportForReal, this.delay);
        },
        async unreportForReal() {
            await fetch(this.url, {
                 method: 'POST',
                 body: new URLSearchParams('unreport&id=' + this.snipper.id)   
            })
   .then((response) => {
    console.log('unreport response:',response);
    this.reported=false;
   })
   .catch((error) => {
      console.log('This error occured:',error);
    })
     this.waitingForUnreport=false;
        },

    slowDelete(){
      this.waitingForDelete=true;
      setTimeout(this.deleteForReal, this.delay);
    },
        async deleteForReal() {
          await fetch(this.url, {
                 method: 'POST',
                 body: new URLSearchParams('delete&id=' + this.snipper.id)   
            })
   .then((response) => {
    console.log('delete response:',response);
    this.deleted=true; 
   })
   .catch((error) => {
      console.log('This error occured:',error);
    })
      this.waitingForDelete=false;            
        }//deleteSnippet
    }//methods

}
</script>

<style scoped>
button {
  padding: 0.5em;
  
}
.snipper {
    display: grid;
	grid-template-columns: repeat(5, 1fr);

}
.snipper > div {
	display: inline-block;
  padding: 0.5em;
}


  .snipper:nth-child(even){
       background-color: whitesmoke;
  }
  button[disabled] { background-color: #ddd; border-color: #ccc; }
button[disabled]:hover { cursor: not-allowed; }

.upvote {
  margin: 0.5em;
}

</style>
