<template>
  <div>
    <h1 class="centralizado"> {{ titulo }} </h1>
    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="Buscar por parte do titulo">
    <ul class="lista-fotos">
        <li class="lista-fotos-item" v-for="foto of fotosComFiltro">
          <meu-painel :titulo="foto.titulo">
            <imagem-responsiva :url="foto.url" :titulo="foto.titulo">
            </imagem-responsiva>
           <meu-botao rotulo="remover" tipo="button" @botaoAtivado="remove(foto)" :confirmacao="true" estilo="perigo"/>
          </meu-painel>
        </li>
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import imagemResponsiva from '../shared/imagem-responsiva/imagemResponsiva.vue';
import Botao from '../shared/botao/Botao.vue';
export default {

  components: {
    'meu-painel' : Painel,
    'imagem-responsiva' : imagemResponsiva,
    'meu-botao' :Botao
  },

  data(){
    return{
      titulo: 'Alurapic',
      fotos: [],
      filtro: ''
    }
  },

  computed: {
    fotosComFiltro() {
      if(this.filtro){
        /* filtrar*/
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
    }
  },

  methods: {
    remove(foto){
      alert(foto.titulo);
    }
  },

  created() {
      // alert('Criei o componente.');
      this.$http.get('http://localhost:3000/v1/fotos')
      .then(res => res.json())
      .then(fotos => this.fotos = fotos, err => console.log(err));
  }
}
</script>

<style>
  .centralizado{
    text-align: center;
  }

  .lista-fotos{
    list-style: none;
  }

  .lista-fotos .lista-fotos-item{
    display: inline-block;
  }

  .filtro{
    display:block;
    width: 100%;
  }

</style>
