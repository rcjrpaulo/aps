<template>
  <div class="row">
    <q-card @click="carregaDetalhes(filme.id)" :key="filme.id" v-for="(filme) in filmes" class="my-card col-3">
      <q-img
        :src="'https://image.tmdb.org/t/p/w500/' + filme.poster_path" :alt="filme.original_title"
        basic
      >
        <div class="absolute-bottom text-subtitle2 text-center">
          {{ filme.title }}
        </div>
      </q-img>
    </q-card>

    <q-dialog
      v-model="dialog"
      persistent
      :maximized="maximizedToggle"
      transition-show="slide-up"
      transition-hide="slide-down"
    >
      <q-card class="bg-primary text-white">
        <q-bar>
          <q-space />

          <q-btn dense flat icon="close" @click="dialog = false">
            <q-tooltip content-class="bg-white text-primary">Close</q-tooltip>
          </q-btn>
        </q-bar>

        <q-card-section>
          <div class="text-h6">{{ filmeDetalhes.title }}</div>
        </q-card-section>

        <q-card-section>
          {{ filmeDetalhes.overview }}
        </q-card-section>
      </q-card>
    </q-dialog>
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
      filmes: [],
      filmeDetalhes: '',
      dialog: false,
      maximizedToggle: true
    }
  },
  mounted () {
    this.$axios.get('https://api.themoviedb.org/3/movie/popular?api_key=f661bc3a7d6de05dd66918f3816b5c45&language=pt-BR').then((response) => {
      this.filmes = response.data.results
    })
  },
  methods: {
    carregaDetalhes (filmeId) {
      this.$axios.get('https://api.themoviedb.org/3/movie/' + filmeId + '?api_key=f661bc3a7d6de05dd66918f3816b5c45&language=pt-BR').then((response) => {
        this.filmeDetalhes = response.data
        this.dialog = true
      })
    }
  }
}
</script>
