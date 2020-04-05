<template>

  <div>
    <h1 v-if="foto._id" class="centralizado">Alteração</h1>
    <h1 v-else class="centralizado">Inclusão</h1>

    <ValidationObserver v-slot="{ invalid, reset }">
      <form @submit.prevent="grava()" @reset.prevent="reset">
        <div class="controle">
          <label for="titulo">TÍTULO</label>
          <validation-provider rules="minmax:1,30" v-slot="{ errors }">
            <input v-model="foto.titulo" id="titulo" autocomplete="off">
            <span>{{ errors[0] }}</span>
          </validation-provider>
        </div>

        <div class="controle">
          <label for="url">URL</label>
          <input v-model.lazy="foto.url" id="url" autocomplete="off">
          <imagem-responsiva v-show="foto.url" :url="foto.url" :titulo="foto.titulo"/>
        </div>

        <div class="controle">
          <label for="descricao">DESCRIÇÃO</label>
          <textarea v-model="foto.descricao" id="descricao" autocomplete="off"></textarea>
        </div>

        <div class="centralizado">
          <meu-botao rotulo="GRAVAR" tipo="submit" :disabled="invalid" />
          <router-link :to="{name: 'home'}"><meu-botao rotulo="VOLTAR" tipo="button"/></router-link>
        </div>

      </form>
    </ValidationObserver>
  </div>
</template>

<script>

import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'
import Botao from '../shared/botao/Botao.vue';
import Foto from '../../domain/foto/Foto.js';
import FotoService from '../../domain/foto/FotoService';
import { ValidationProvider, ValidationObserver, extend } from 'vee-validate';
import { required } from 'vee-validate/dist/rules';

extend('required', {
  ...required,
  message: 'This field is required'
});
extend('minmax', {
  ...required,
  validate(value, { min, max }) {
    return value.length >= min && value.length <= max;
  },
  params: ['min', 'max'],
  message: 'Esse campo não pode ser vazio ou maior que 30 caracteres'
});

export default {
  name: 'Cadastro',
  components: {
    ValidationProvider,
    ValidationObserver,
    'imagem-responsiva': ImagemResponsiva, 
    'meu-botao': Botao
  },
  data() {
    return {
      id: this.$route.params.id,
      foto: new Foto(),
    }
  },
  methods: {

    grava() {

      this.service
        .cadastra(this.foto)
        .then(() => {
          if(this.id) this.$router.push({ name: 'home'});
          this.foto = new Foto()
        }, err => console.log(err));

    }
  },
  created() {
    this.service = new FotoService(this.$resource);
    if(this.id) {
      this.service
        .busca(this.id)
        .then(foto => this.foto = foto);
    }
  },
}

</script>
<style scoped>

  .centralizado {
    text-align: center;
  }
  .controle {
    font-size: 1.2em;
    margin-bottom: 20px;

  }
  .controle label {
    display: block;
    font-weight: bold;
  }

 .controle label + input, .controle textarea {
    width: 100%;
    font-size: inherit;
    border-radius: 5px
  }

  .centralizado {
    text-align: center;
  }

</style>