<template>
  <div class="bg-primary">
    <div class="container p-3">
      <div class="card shadow">
        <form id="register">
          <input type="hidden" name="register" />
          <div class="login p-3 py-4">
            <div class="d-flex justify-content-left">
              <router-link to="/">
                <i class="mdi mdi-arrow-left"></i>
              </router-link>
            </div>
            <div class="text-center mb-4">
              <div class="d-flex justify-content-center">
                <img
                  class="mr-2"
                  src="../assets/images/vedas-logo.png"
                  alt
                  width="10%"
                  height="10%"
                />
                <h2 class="text-primary">Vedas</h2>
              </div>
            </div>
            <div class="form-group">
              <label for>Nome</label>
              <input
                type="text"
                name="name"
                id="name"
                class="form-control"
                placeholder="Ex: Pedro Barbosa"
              />
            </div>
            <div class="form-group">
              <label for>E-mail</label>
              <input
                type="text"
                name="email"
                class="form-control"
                placeholder="Ex: (66) 99999-9999"
              />
            </div>
            <div class="form-group">
              <label for>Telefone</label>
              <input type="text" name="tel" class="form-control" placeholder="Ex: (66) 99999-9999" />
            </div>
            <div class="form-group">
              <label for>Senha</label>
              <input
                type="password"
                id="password"
                name="pass"
                class="form-control"
                placeholder="********"
                aria-describedby="helpId"
              />
            </div>
            <button
              v-on:click="register()"
              type="submit"
              id
              class="btn btn-block btn-warning"
            >Cadastrar</button>
            <router-link to="/" tag="button" class="btn btn-block btn-outline-primary">Voltar</router-link>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import vars from '../plugins/env.local'

export default {
  data: function () {
    return {
      url: `${vars.host}userController.php`
    }
  },
  methods: {
    register() {
      document.getElementById("register").addEventListener("submit", event => {
        event.preventDefault();
        let form = new FormData(document.getElementById("register"));
        fetch(this.url, {
          method: "POST",
          body: form
        }).then(resp => {
          return resp.json()
        }).then(json => {
          console.log(json)
          this.$router.push('/')
        })
      })
    }
  }
};
</script>

<style>
</style>