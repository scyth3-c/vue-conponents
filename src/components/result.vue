<template>
    
   <b-container  style="position: relative;">
  
   <b-card class="col-md-3 ml-1 mt-3 shadow-lg p-2 mb-3  bg-dark rounded" style="min-width: 310px;"> 
    <b-badge class="p-1" style="width: 100%;">
     <p>Busqueda Filtro &nbsp;<b-badge style="max-width: 70px; p-1 overflow: hidden;" class="bg-success">{{searchText}} <b-icon icon="funnel"></b-icon> </b-badge> </p>
    </b-badge>
     <b-card class="bg-dark rounded" style="min-height: 300px; max-height: 390px; overflow:auto; border: none;">
        <b-table hover dark table-variant="dark" :items="searchResult" :fields="field" >
          <template #cell(nombre)="data"> 
             <button class="btn btn-large btn-primary" @click="show(data.item._id)"  style="min-width: 150px;" >{{data.value}}</button>
             <b-icon class="ml-2" icon="search"></b-icon>
          </template>
        </b-table>
     </b-card>
    </b-card>
   
  
   </b-container>



</template>

   <style>

::-webkit-scrollbar {
  width: 10px;
  border: none;
}
::-webkit-scrollbar-track {
  background: #343a40; 
}
::-webkit-scrollbar-thumb {
  background: #484f58; 
  border-radius: 10px;
}
::-webkit-scrollbar-thumb:hover {
  background: #555; 
}
 </style>

<script>

export default {
    name: 'result',
    data(){
        return {
            
            url: this.GLOBAL.API,
            searchText: '',
            uids: 1,
             field: ['nombre'],
            resultSearch: [
            ]

        }
    },
    created() {
       this.charge();
       this.$root.$on('buscar', (text) =>{
           this.searchText = text;
       });
       this.$root.$on('refresh',()=>{
         this.charge();
       });
    },
    methods:{
      async charge(){
        const res = await this.axios.get(`${this.url}notes/recollector`);
        this.resultSearch = res.data;
      },
      show(id){
        this.$root.$emit('show',id);
      }
    },

    computed: {
         searchResult(){
             return this.resultSearch.filter( item => item.nombre.includes(this.searchText));
         }
    }
}
</script>
