<template>
  <section class="bg-container">
    <div class="bg-form"></div>
      <div class="content-geral">
      <img class="imagem-map" src="./assets/map.svg" alt="ilusreação de mapa"/>
      <div class="inputBox" v-show="input">
        <h2 class="title">Consulte o CEP</h2>
        <label>Digite um CEP: <span style="color:red;">*</span></label>
        <input type="text" value=cep v-model="recebeCep">
        <small id="nomeErro" v-if="erroVazio">preencha o campo</small> 
        <small id="nomeErro" v-else-if="erroIncompleto">Digite o CEP completo</small>
        <small id="nomeErro" v-else-if="erroCepInvalido">CEP inexistente</small>
        <small id="exemplo"> Ex.: 60762080</small>
        <a class="btn-busca" @click="buscarCep">Buscar</a>
      </div>  
      <div class="tableBox" v-show="table">
        <Consulta :bairro="bairro" :rua="rua" :cidade="cidade" :estado="estado" :cep="cep" v-show="table"/>
        <a class="btn-busca" @click="resetar">Nova busca</a>
      </div>
    </div>
  </section>  
</template>

<script>
import axios from 'axios';
import Consulta from './components/Consulta';

export default {
  name: 'App',

  data(){
    return{
      infosCep: [],
      bairro: "",
      rua: "",
      cidade: "",
      estado: "",
      cep: "",
      recebeCep: "",
      erroVazio: false,
      erroIncompleto: false,
      table: false,
      input:true,
      erroCepInvalido: false,
      validandoCep: "",
      
    }
  },
  components: {
    Consulta
  },
  methods: {
    buscarCep: function(){

      axios.get("https://api.postmon.com.br/v1/cep/" + this.recebeCep).then(res => {
          this.bairro = res.data.bairro;
          this.rua = res.data.logradouro;
          this.cidade = res.data.cidade;
          this.estado = res.data.estado;
          this.cep = res.data.cep;

          this.infosCep = res.data;
          this.validandoCep = res.data.cep;
        })


      if(this.recebeCep == '' || this.recebeCep == ' '){

        this.erroVazio = true;
        this.erroIncompleto = false;

        this.recebeCep = "";
        this.validandoCep = "";

      }else if(this.recebeCep.length < 8){

        this.erroVazio = false;
        this.erroIncompleto = true;

        this.recebeCep = "";
        this.validandoCep = "";
        
      }else if(this.recebeCep != this.validandoCep){
        
        this.erroCepInvalido = true;

        this.recebeCep = "";
        this.validandoCep = "";

      }else{
        
          this.erroVazio = false;
          this.erroIncompleto = false;
          this.erroCepInvalido = false;
          this.input = false;
          this.table = true;
          
          this.recebeCep = "";
          this.validandoCep = "";

          console.log("deu bom");
      }
    },
    resetar: function(){
      this.table = false;
      this.input = true;

      this.recebeCep = "";
      this.validandoCep = "";
    }
  },  
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@300;400;700&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Roboto Condensed', sans-serif;
}

.bg-container{
  position: relative;
  width: 100%;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background:#201f2d;
}

.bg-container .bg-form{
  position: absolute;
  top:0;
  left: 0;
  width: 100%;
  height: 100%;
  min-width: 600px;
  background-color: #bd07ca;
  clip-path: polygon(0 0, 0 100%, 36% 51%);
  transition: 0.8s ease-in;
}

.imagem-map{
  max-width: 400px;
  width: 50%;
}

.content-geral{
  position: relative;
  width: 100%;
  height: 100%;
  max-height:500px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}

.title{
  color: #201f2d;
}

.inputBox{
  position: relative;
  max-height: 700px;
  max-width: 500px;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: #f2f4f5;
  border-radius: 20px;
  padding: 50px;
}

.inputBox label{
  font-weight: 500;
  color: #000;
}
.inputBox input{
  max-width: 400px;
  height: 30px;
  background: #FFFFFF 0% 0% no-repeat padding-box;
  padding: 12px 10px;
  margin: 8px 0px 1px;
  border: 1px solid #707070;
  border-radius: 8px;
}

.tableBox {
  position: relative;
  max-width: 400px;
  width:100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: #f2f4f5;
  border-radius: 20px;
  padding: 15px;
}

.btn-busca{
  max-width: 150px;
  margin-top:2%;
  padding: 8px 20px;
  background: #bd07ca;
  border-radius: 50px;
  color:#fff;
  font-weight: 500;
  letter-spacing: 1px;
  display: flex;
  justify-content: center;
  text-decoration: none !important; 
}

.btn-busca:hover{
  color: #f2f4f5;
  background: #bd07cac0;
}

#nomeErro{
  color: #bd07ca;
}

#exemplo{
  color: #6e6e6e;
}

@media (max-width:991px){
  .bg-container{
    padding: 40px 40px;
  }

  .bg-container .bg-form{
    min-width:100px;
    width:100%;
    clip-path: polygon(0 0, 100% 0, 50% 45%);
  }

  .content-geral{
    flex-direction:column;
    margin: 0;
  }
  .imagem-map{
    width: 100%;
    max-width: 250px;
  }

  .inputBox{
    max-width: 500px;
    width: 100%;
    margin: 20px;
  }

  .tableBox{
    max-width: 500px;
    width: 100%;
  }
}
</style>
