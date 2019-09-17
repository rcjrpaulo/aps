<template>
  <div class="row">
    <q-card :key="filme.id" v-for="(filme) in filmes" class="my-card col-3">
      <q-img
        :src="'https://image.tmdb.org/t/p/w500/' + filme.poster_path" :alt="filme.original_title"
        basic
      >
        <div class="absolute-bottom text-subtitle2 text-center">
          {{ filme.title }}
        </div>
      </q-img>
    </q-card>
  </div>
</template>

<style>
  .my-card {
    max-width: 250px;
  }
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
    this.$axios.get('https://api.themoviedb.org/3/movie/popular?api_key=f661bc3a7d6de05dd66918f3816b5c45&language=pt-BR').then((response) => {
      this.filmes = response.data.results
    })
  }
}
</script>
