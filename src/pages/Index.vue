<template>
  <div class="content">
    <div class="form-login">
      <div class="login" v-if="!logado">
        <div>
          <div class="input-page window-width bg-light column items-center no-wrap">
            <div class="input-card shadow-4 bg-white column items-center justify-center no-wrap">

              <div class="full-width">
                <q-toolbar color="secondary">
                  <q-icon name="edit" />
                  <q-toolbar-title>
                    Login
                  </q-toolbar-title>
                </q-toolbar>

                <div class="layout-view layout-padding">
                  <q-field
                    icon="cloud"
                    label="Email"
                    helper="Helper"
                  >
                    <q-input v-model="credentials.email" />
                  </q-field>
                  <q-field
                    icon="cloud"
                    label="Password"
                    helper="Helper"
                  >
                    <q-input type="password" v-model="credentials.password" />
                  </q-field>
                  <div class="submit row reverse">
                    <q-btn color="primary" @click="logado = 1">Login</q-btn>
                    <q-btn color="secondary">Cancel</q-btn>
                  </div>
                </div>
              </div>

            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="row" v-show="logado">
      <q-header elevated>
        <q-toolbar>
          <q-toolbar-title>
            Quasar App
          </q-toolbar-title>

          <div><a href="#" style="color: #FFF;" @click="logado = 0">Sair</a></div>
        </q-toolbar>
      </q-header>

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
        content-style="'opacity: 0.5"
        @click="dialog = false"
      >
        <q-img
          v-if="filmeDetalhes && filmeDetalhes.poster_path"
          :src="'https://image.tmdb.org/t/p/w500/' + filmeDetalhes.poster_path"
          basic
          content-style="'opacity: 0.9"
        >
          <q-card class="bg-dark text-white relative-position">
            <q-bar>
              <q-space />

              <q-btn dense flat icon="close">
                <q-tooltip content-class="bg-white text-primary">Close</q-tooltip>
              </q-btn>
            </q-bar>

            <q-card-section>
              <div class="text-h6 text-center">{{ filmeDetalhes.title }}</div>
            </q-card-section>

            <q-card-section class="text-center">
              {{ filmeDetalhes.overview }}
            </q-card-section>
          </q-card>
        </q-img>
      </q-dialog>
    </div>
  </div>
</template>

<style>
  .my-card {
    max-width: 250px;
  }
  .input-card {
    border-radius: 5px;
    margin-top: -50px;
    width: 80vw;
    max-width: 600px;
    margin: 10vh;
  }
  .layout-padding {margin: 0 32px}
  .submit {margin: 5px}
</style>

<script>
export default {
  name: 'PageIndex',
  data: function () {
    return {
      titulo: 'PopFilmes',
      credentials: { password: '', email: '' },
      logado: 0,
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
