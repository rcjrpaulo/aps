<template>
  <q-page class="flex flex-center">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title class="text-center">
          {{ titulo }}
        </q-toolbar-title>
      </q-toolbar>
    </q-header>
    <q-body v-if="filmes.length" class="text-center">
      <div :key="filme.id" v-for="(filme, chave) in filmes">
        <img :src="'https://image.tmdb.org/t/p/w500/' + filme.poster_path" :alt="filme.original_title">
        <h2>{{(chave+1) + '. ' + filme.original_title }}</h2>
      </div>
    </q-body>
  </q-page>
</template>

<style>
</style>

<script>
export default {
  name: 'PageIndex',
  data: function () {
    return {
      titulo: 'PopFilmes',
      filmes: []
    }
  },
  mounted () {
    this.$axios.get('https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=f661bc3a7d6de05dd66918f3816b5c45').then((response) => {
      this.filmes = response.data.results
    })
  }
}
</script>
