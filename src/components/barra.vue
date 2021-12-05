<template>

     <nav id="barra" class="navbar navbar-expand-lg navbar-dark bg-dark mb-3" style="overflow: auto;">
      <div class="col-md-2">
         <b-icon class="navbar-brand"  scale="2.5" icon="file-text-fill" variant="white"></b-icon>  
         <span class="navbar-brand h3">KnockNotes</span>
      </div>
    
          <input id="barra-input" placeholder=" aprender vue" class="col-md-3 mr-ms-2 form-control" v-model="search" autofocus @change="sendSearch" type="text">
           <b-button  @click="refresh" class="_query ml-2 my-sm-2 btn btn-secondary">fix refresh <b-icon icon="hammer"></b-icon> </b-button>
       
       <div class="ml-5">
           <span class="navbar-brand badge badge-warning"> {{totalCount}} Documentos Actuales </span>
        <a href="https://github.com/scyth3-c">
          <b-icon class="ml-5" variant="white" icon="github" scale="1.5"></b-icon>
        </a>
         <a href="https://www.paypal.com/paypalme/bohorquezrojas17">
           <b-icon class="ml-3" variant="white" icon="suit-heart-fill" scale="1.5"></b-icon>
         </a>
       
       </div>
        
     </nav>  
  
</template>

<style>
 ._logo{
    visibility: collapse;
    display: none;
 }

@media only screen and (max-width: 880px) {
  ._query {
    visibility: collapse;
  }
  ._logo{
    visibility: visible;
    display:  flex;
    width: 50px;
    height: 50px;
  }
}

</style>

<script>

export default {
    name: 'barra',
    data() {
        return {
          url: this.GLOBAL.API,
          totalCount: 0,
          search: ''
        }
    },
    created(){
      this.$root.$on('refresh',()=>{
      this.count();
      });
      this.$root.$on('input-value',(data)=>{
        document.getElementById('barra-input').textContent=data;
        this.search = data;
      });
    },
    methods:{
      refresh(){
         this.$root.$emit('refresh');
         this.count();
      },
      async count(){
          const total = await this.axios.post(`${this.url}notes/items`);
          if(this.totalCount >= 1000) {
            this.totalCount = '+999';
          } else {
          this.totalCount = total.data.items;
          }
       }
    },
    computed: {
        sendSearch()
        {
          this.refresh();
         return this.$root.$emit('buscar',this.search);
        }
                
     }
    
}
</script>
