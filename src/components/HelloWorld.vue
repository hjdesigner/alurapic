<template>
  <div class="corpo">
    <h1 class="centralizado">{{ titulo }}</h1>

    <input type="search" class="filtro" @input="filtro = $event.target.value" placeholder="filtre pelo título da foto">
    
    <ul class="lista-fotos">
      <li v-for="foto of fotosComFiltro" :key="foto.id" class="lista-fotos-item">
        <meu-painel :titulo="foto.titulo">
          <imagem-responsiva :url="foto.url" :titulo="foto.titulo"/>
        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
import Painel from './shared/painel/painel.vue'
import ImagemResponsiva from './shared/imagem-responsiva/ImagemResponsiva.vue'

export default {
  name: 'HelloWorld',
  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva
  },
  data: () => {
    return {
      titulo: 'Alura Pic',
      fotos: [],
      filtro: '',
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
    this.$http.get('v1/fotos')
      .then(res => res.json())
      .then(fotos => this.fotos = fotos, err => console.log(err))
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
</style>
