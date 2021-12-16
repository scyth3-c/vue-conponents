<template>
  <div id="app" >
    
  <barra/>
 
  <div class="container">
      <div class="row">
      
      
      <div v-show="widthMatch" class="col-md-6"> <result/> </div>
      <div v-show="!widthMatch" class="col-md-4"> <result/> </div>
      <div class="col-md-4"> <show/> </div>
      <div class="col-md-4"> <add/> </div>



    </div>
  </div>

  </div>
</template>

<script>

import barra from './components/barra.vue'
import result from './components/result.vue'
import show from './components/show.vue'
import add from './components/add.vue'
export default {
  name: 'App',
  components: {
      barra,
      result,
      show,
      add
  },
  data(){
    const widthQuery = matchMedia("(min-width: 768px) and (max-width:  991px )") 
    return {
        widthQuery: widthQuery,
        widthMatch: widthQuery.matches
    }
  },
  mounted(){
    this.widthQuery.addEventListener('change',()=>{
      this.widthMatch = this.widthQuery.matches;
    });
  }
}
</script>

