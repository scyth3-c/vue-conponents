
<template>
 
   <b-container  style="position: relative;">
  
    <b-card class="col-md-3 ml-1 mt-3 shadow-lg p-1 mb-2 bg-dark rounded" style=" min-width: 310px; min-height: 400px;"> 
        
    <b-badge class="p-2 mb-3    " style="width: 100%;">
    <span><b-badge>Inspector   </b-badge></span>
    </b-badge>

  <b-row class="mt-2" style="display: flex;">
           <b-button @click="reset()" style="margin-left: 80%; position: abosulte; z-index:3;" class="mt-1"><b-icon icon="arrow-counterclockwise"></b-icon></b-button>
          <b-col style="position: absolute">
              <b-badge class="mt-1" style="width: 40%;" variant="danger" ><b-icon icon="bricks"></b-icon> nombre</b-badge>
               <br>
               <b-icon icon="arrow-return-right" variant="white"></b-icon> <b-badge class="ml-3 mt-2 p-1" style="min-width: 20%; max-width: 65%; overflow: auto; " variant="primary" >{{nombre}}</b-badge>
               
          </b-col>
      </b-row>
        <b-row class="mt-3">
          <b-col>
              <b-badge style="width: 40%;" variant="success" ><b-icon icon="box-seam"></b-icon> contenido</b-badge>
               <br>
               <b-icon icon="arrow-down" variant="white"></b-icon>
               <div class="card card-body bg-secondary" style="color:white; font-family: 'Patrick Hand', cursive; max-height: 300px; overflow: auto;">{{contenido}}</div>
          </b-col>
      </b-row>
       <span><b-badge>ID: {{id}}</b-badge></span>
       <b-button @click="_delete" variant="danger" class="btn btn-sm ml-3 mt-1"><b-icon variant="white" icon="trash"></b-icon></b-button>
   
    </b-card>
   <div id="alt"></div>
   </b-container>



</template>



<script>
export default {
    name: 'show',
    data(){
        return {
           nombre: "",
           contenido: "",
           id: ' ',
           url: this.GLOBAL.API
        }
    },
    created(){
       this.$root.$on('show',(id) =>{
          this.charge(id);
       });
    },
    methods: {
        async charge(id){
             const res = await this.axios.get(`${this.url}notes/show?id=${id}`);
             this.id = id;
             this.nombre = res.data.nombre;
             this.contenido = res.data.conten;
        },
        reset(){
             this.nombre ='';
             this.contenido ='';
             this.id=' ';
        },
       async _delete(){
          if(this.nombre == '')
          {
              this.alert('alt','no hay nota que borrar!');
          } else {
              await this.axios.delete(`${this.url}notes/delete?id=${this.id}`);
              this.reset();
              this.alert('alt','borrado! en caso de estar vacio solo se omita! =_=');
          }
          
        },
          alert(id,any){
           const desk = document.getElementById(id);
             const alert = document.createElement('div');
             alert.setAttribute('id','alerta');
             alert.innerHTML = `<div class="alert alert-warning alert-dismissible fade show" role="alert">
                         ${any}
                        <button type="button" class="close" data-dismiss="alert" aria-label="Close" onclick="document.getElementById('alt').removeChild(document.getElementById('alerta'));">
                        <span aria-hidden="true">&times;</span>
                        </button>
                        </div>`; 

            desk.appendChild(alert);
           this.$root.$emit('refresh');
        }
    }
}
</script>
