<template>
  <div class="corpo">
    <h1 class="centralizado">{{ titulo }}</h1>

    <input type="search" class="filtro" @input="filtro = $event.target.value" placeholder="filtre pelo título da foto">
    
    <p v-show="mensagem" class="centralizado">{{ mensagem }}</p>

    <ul class="lista-fotos">
      <li v-for="foto of fotosComFiltro" :key="foto.id" class="lista-fotos-item">
        <meu-painel :titulo="foto.titulo" v-meu-transform:scale.animate="1.2">
          <imagem-responsiva :url="foto.url" :titulo="foto.titulo" v-meu-transform:rotate.animate="15" />
            <meu-botao
                rotulo="remover"
                tipo="button"
                :confirmacao="false"
                estilo="perigo"
                @botaoAtivado="remove(foto)" />
        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/painel.vue'
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'
import Botao from '../shared/botao/Botao.vue';
import FotoService from '../../domain/foto/FotoService';

export default {
  name: 'Home',
  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva,
    'meu-botao': Botao
  },
  methods: {
    remove(foto) {
      this.service
        .apaga(foto._id)
        .then(
          () => {
            let indice = this.fotos.indexOf(foto);
            this.fotos.splice(indice, 1);
            this.mensagem = 'Foto removida com sucesso'
          }, 
          err => {
            this.mensagem = 'Não foi possível remover a foto';
            console.log(err);
          }
        )
    }
  },
  data: () => {
    return {
      titulo: 'Alura Pic',
      fotos: [],
      filtro: '',
      mensagem: '',
    } 
  },
  computed: {
    fotosComFiltro() {
      if (this.filtro) {
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
    }
  },
  created() {

    this.service = new FotoService(this.$resource);

    this.service
      .lista()
      .then(fotos => this.fotos = fotos, err => console.log(err));
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .filtro {
    display: block;
    width: 100%;
  }
  .centralizado {
    text-align: center;
  }

  .corpo {
    font-family: Helvetica, sans-serif;
    margin: 0 auto;
    width: 96%;
  }

  .lista-fotos {
    list-style: none;
  }

  .lista-fotos .lista-fotos-item {
    display: inline-block;
  }
  .painel-conteudo {
    overflow: hidden;
  }
</style>
