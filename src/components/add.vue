 <template>
   <b-container  style="position: relative;">
   <b-card class="col-md-3 ml-1 mt-3 shadow-lg p-0 mb-2 bg-dark rounded" style="min-width: 320px; min-height:400px;"> 
         <b-badge class="p-2" style="width: 100%;"> <b-icon icon="pencil-fill"></b-icon>  escribir nueva</b-badge>
              <b-row class="mt-2">
          <b-col>
              <b-badge class="mt-1" style="width: 40%;" variant="danger" ><b-icon icon="bricks"></b-icon> nombre</b-badge>
               <br>
               <input id="nombre_" v-model="nombre" class="mt-1 mb-3" style=" outline:0; border:none; border-radius:7px;" placeholder="minimo 4 caracteres" type="text" required maxlength="40" minlength="4">
          </b-col>
          </b-row>
         <b-row class="mt-2">
          <b-col>
              <b-badge class="mt-1" style="width: 40%;" variant="success" ><b-icon icon="plus-circle-fill"></b-icon> contenido</b-badge>
               <br>
               <textarea placeholder="minimo 10 caracteres" v-model="conten"  class="lg-textarea form-control mt-2" maxlength="2000" minlength="10"></textarea>
               <b-icon variant="white" icon="chevron-compact-up" class="float-right"></b-icon>
               <p style="font-size: 10px;" class="text-white float-right">desliza esta parte hacia abajo</p>
           </b-col>
          </b-row>
           <b-button class="mt-2" @click="charge">enviar</b-button>
    </b-card>  
  <div id="add"></div>
   </b-container>
</template>
<style>
 input,textarea {

     font-family: 'Patrick Hand', cursive;

    }
</style>
<script>
export default {
    name: 'add',
    data(){
        return{
           nombre: '',
           conten: '',
           url: this.GLOBAL.API
        }
    },
    methods: {
        async charge(){
          if(this.nombre.length <= 4 && this.conten.length <= 10)
          {
           this.alert('add', 'los datos son muy cortos');
          } else{
              try {
              let body = {
                  nombre: this.nombre,
                  conten: this.conten
              }
              await this.axios({
                  method:'POST',
                  url:`${this.url}notes/new`,
                  data:body
                  });
              } catch (error) {
                this.alert('add','error al enviar la nota!');
              }
              }
              this.$root.$emit('refresh');
              this.alert('add',`${this.nombre} agregado correctamente!`);
              this.$root.$emit('buscar',this.nombre);
              this.$root.$emit('input-value',this.nombre);
              this.nombre = '';
              this.conten = '';
          },
           alert(id,any){
           const desk = document.getElementById(id);
             const alert = document.createElement('div');
             alert.setAttribute('id','alerta');
             alert.innerHTML = `<div class="alert alert-warning alert-dismissible fade show" role="alert">
                         ${any}
                        <button type="button" class="close" data-dismiss="alert" aria-label="Close" onclick="   document.getElementById('add').removeChild(document.getElementById('alerta'));">
                        <span aria-hidden="true">&times;</span>
                        </button>
                        </div>`; 
             desk.appendChild(alert);
             this.$root.$emit('refresh');
         }
        }
    }

</script>
