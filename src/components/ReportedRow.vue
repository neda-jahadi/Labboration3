<template>
  <div class='report'>
    <div class="reported">

        <div><span style="font-size:1.2em;"> Title:</span> {{reported.title}}</div>
        <div><span style="font-size:1.2em;">Content:</span>{{reported.content}}</div>
        
    </div>
  </div>
</template>

<script>
export default {
  props : {
     reported : Object(null)
  },
    
}
</script>

<style scoped>
button {
  padding:0.5em;
}
  .reported {
    display: grid;
	grid-template-columns: repeat(2, 1fr);
  
}
.report:nth-child(even) {
  background-color: whitesmoke;
}
.reported > div {
	display: inline-block;
  margin: 1em;
}





</style>
