<template>
  <div>
      <h1>Cuenta Regresiva</h1>
      <h2>{{ formatoTiempo(tiempo) }}</h2>
      <button v-for="valores in botones" :key="valores" @click="countdown(valores.tiempo)">{{valores.etiqueta}}</button>
      <br>
        <div v-if="tiempo > 0">
          <button @click="pausar(estado.activo)">{{estado.boton}}</button>
          <button @click="tiempo = 0">Reset</button>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Fechas',
  data(){
    return{
      tiempo : 0,
        botones: [
            {etiqueta: '03s'  , tiempo: 3 },
            {etiqueta: '01m'  , tiempo: 60 }, 
            {etiqueta: '05m'  , tiempo: 60*5 }, 
            {etiqueta: '10m' , tiempo: 60*10 }, 
            {etiqueta: '30m' , tiempo: 60*30 },  
        ],
        intervalo : '',
     estado: {activo: false, boton: 'Iniciar'},
    }
  },

  methods: {
      formatoTiempo: function (seg){
          let minutos = ('0' + Math.floor(seg/60)).slice(-2);
          let segundos = ('0' + seg%60).slice(-2);
          return `${minutos}:${segundos} min/seg`
      },
      countdown: function (count){
          clearInterval(this.intervalo)
          this.tiempo = count;
            this.estado.activo = true;
          this.estado.boton = 'Pausar';
          this.intervalo = setInterval(()=>{
              if (this.tiempo > 0) {
                  this.tiempo--; 
              } else {
                  clearInterval(this.intervalo)
              }
          } , 1000);
          
      },
        pausar: function(active){
          if(active){
              clearInterval(this.intervalo);
              this.estado.activo = false;
              this.estado.boton = 'Iniciar';
          }else if(this.tiempo > 0){
              this.countdown(this.tiempo);
          }
      }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  body{
      text-align: center;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }
  h1{
      font-size: 130px;
  }
  h2{
      font-size: 90px;
      color: gray;
  }
  button{
      margin: 20px;
      font-size: 50px;
  }
</style>
