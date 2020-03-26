<template>
  <div>
    <h1>{{ titulo }}</h1>
    <ul>
      <li v-for="foto of fotos" :key="foto.id"><img :src="foto.url" :alt="foto.titulo" /></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: () => {
    return {
      titulo: 'Alura Pic',
      fotos: []
    } 
  },
  created() {
    this.$http.get('http://localhost:3000/v1/fotos')
      .then(res => res.json())
      .then(fotos => this.fotos = fotos, err => console.log(err))
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
