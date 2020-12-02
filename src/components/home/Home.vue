<template>
  <div>
    <h1 class="centralizado">{{titulo}}</h1>
    <p v-show="mensagem" class="centralizado">{{ mensagem }} </p>
    <input type="search" class="filtro" @input="filtro = $event.target.value" placeholder="filtre por partes" name="search">
    <!-- {{ filtro }} data bind : fonte de dados -> view -->
    <ul class="lista-fotos">
    
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro">
        
        <my-panel :titulo="foto.titulo">
          
          <my-image v-my-transform:scale.animate="1.4" :url="foto.url" :titulo="foto.titulo"/>
          <router-link :to="{ name: 'altera', params: {id: foto._id} }"> 
              <my-button tipo="button" rotulo="ALTERAR" />
          </router-link>
          <my-button 
            tipo="button" 
            rotulo="REMOVER"
            @botaoAtivado="remove(foto)"
            :confirmacao="false"
            estilo="perigo"/>
        </my-panel>     

      </li>    
    </ul>

  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue';
import Botao from '../shared/botao/Botao.vue';
import Transform from '../../directives/Transform';
import FotoService from '../../domain/foto/FotoService';

export default { 

  components: {
    'my-panel' : Painel,
    'my-image' : ImagemResponsiva,
    'my-button' : Botao

  },

  directives: {
    'my-transform' : Transform 
  },
  
  /* Fonte de dados... */
  data() {

    return {
      titulo: 'Alurapic',
      fotos: [], 
      filtro: '',
      mensagem : ''
    }
  },

  computed: {
    fotosComFiltro(){
      if (this.filtro) {
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
    }
  },

  methods: {
    remove(foto) {
      
      this.service.apaga(foto._id)
      .then(() => {
        let indice = this.fotos.indexOf(foto);
        this.fotos.splice(indice,1);
        this.mensagem = 'Foto removida com sucesso';
        
      }, err => { 
        this.mensagem = err.message; 
      });
    }
  },

  created(){

    this.service = new FotoService(this.$resource);

    this.service
      .lista()
      .then(fotos => this.fotos = fotos, err => {
        this.mensagem = err.message;
        });

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

.lista-fotos .lista-fotos-item {
  display: inline;
}

.filtro {
  display: block;
  width: 100%;
}

</style>
