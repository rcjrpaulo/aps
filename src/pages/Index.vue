<template>
  <div class="content">
<!--    TELA DE LOGIN-->
    <div class="form-login">
      <div class="login" v-if="!logado && !telaRegistro">
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
                    label="Senha"
                    helper="Helper"
                  >
                    <q-input type="password" v-model="credentials.password" />
                  </q-field>
                  <div class="submit row reverse" style="margin-top: 20px;">
                    <q-btn color="primary" @click="logar">Logar</q-btn>
                    <q-btn color="secondary" style="margin-left: 10px; margin-right: 10px;" @click="telaRegistro = 1">Registrar</q-btn>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!--    TELA DE CADASTRO-->
    <div class="form-login">
      <div class="login" v-if="!logado && telaRegistro">
        <div>
          <div class="input-page window-width bg-light column items-center no-wrap">
            <div class="input-card shadow-4 bg-white column items-center justify-center no-wrap">
              <div class="full-width">
                <q-toolbar color="secondary">
                  <q-icon name="edit" />
                  <q-toolbar-title>
                    Cadastro de usuário
                  </q-toolbar-title>
                </q-toolbar>

                <div class="layout-view layout-padding">
                  <q-field
                    icon="cloud"
                    label="Nome do usuário"
                    helper="Helper"
                  >
                    <q-input v-model="credentials.name" />
                  </q-field>
                  <q-field
                    icon="cloud"
                    label="Email de autenticação"
                    helper="Helper"
                  >
                    <q-input v-model="credentials.email" />
                  </q-field>
                  <q-field
                    icon="cloud"
                    label="Senha"
                    helper="Helper"
                  >
                    <q-input type="password" v-model="credentials.password" />
                  </q-field>
                  <div class="submit row reverse" style="margin-top: 20px;">
                    <q-btn color="secondary" style="margin-left: 10px; margin-right: 10px;" @click="registrar">Registrar</q-btn>
                    <q-btn color="primary" style="margin-left: 10px; margin-right: 10px;" @click="telaRegistro = 0">Voltar</q-btn>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!--    TELA DE FILMES-->
    <div class="row movies" v-show="logado">
      <q-header elevated>
        <q-toolbar>
          <q-toolbar-title>
            Lançamentos
          </q-toolbar-title>

          <div><a href="#" style="color: #FFF;" @click="deslogar">Sair</a></div>
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
      credentials: { password: '', email: '', name: '' },
      logado: 0,
      telaRegistro: 0,
      filmes: [],
      filmeDetalhes: '',
      dialog: false,
      maximizedToggle: true
    }
  },
  mounted () {
    this.carregaFilmes()
  },
  methods: {
    carregaDetalhes (filmeId) {
      this.$axios.get('https://api.themoviedb.org/3/movie/' + filmeId + '?api_key=f661bc3a7d6de05dd66918f3816b5c45&language=pt-BR').then((response) => {
        this.filmeDetalhes = response.data
        this.dialog = true
      })
    },
    carregaFilmes () {
      this.$axios.get('https://api.themoviedb.org/3/movie/popular?api_key=f661bc3a7d6de05dd66918f3816b5c45&language=pt-BR').then((response) => {
        this.filmes = response.data.results
      })
    },
    logar () {
      let self = this
      let email = this.credentials.email
      let password = this.credentials.password
      this.$axios.post('http://apiauth.localhost/api/login?password=' + password + '&email=' + email)
        .then((response) => {
          self.logado = 1
        })
        .catch((error) => {
          alert('Login Inválido! ' + error.message)
          self.logado = 0
        })
    },
    registrar () {
      let self = this
      let name = this.credentials.name
      let email = this.credentials.email
      let password = this.credentials.password
      this.$axios.post('http://apiauth.localhost/api/register?password=' + password + '&email=' + email + '&name=' + name)
        .then((response) => {
          self.telaRegistro = 0
          alert('Registrado com sucesso!')
        })
        .catch((error) => {
          alert('Erro ao registrar ' + error.message)
        })
    },
    deslogar () {
      this.logado = 0
    }
  }
}
</script>
