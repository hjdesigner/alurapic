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
      fotos: [
        {
          id: 0,
          url: 'https://abrilsuperinteressante.files.wordpress.com/2018/05/filhotes-de-cachorro-alcanc3a7am-o-c3a1pice-de-fofura-com-8-semanas1.png?quality=70&strip=info&resize=680,453',
          titulo: 'Cachorro',
        },
        {
          id: 1,
          url: 'https://abrilsuperinteressante.files.wordpress.com/2018/05/filhotes-de-cachorro-alcanc3a7am-o-c3a1pice-de-fofura-com-8-semanas1.png?quality=70&strip=info&resize=680,453',
          titulo: 'Cachorro',
        },
      ]
    } 
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
