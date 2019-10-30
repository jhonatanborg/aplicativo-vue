<template>
  <div class="login">
    <div class="bg-primary vh-100">
      <div class="container p-4">
        <div class="card shadow mt-4">
          <form id="login">
            <input type="hidden" name="login" />
            <div class="login p-3 py-4">
              <div class="text-center mb-5">
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
                <v-text-field label="Usuário" outlined id="email" name="email" type="text"></v-text-field>
              </div>
              <div class="form-group">
                <v-text-field label="Senha" outlined id="pass" name="pass" type="password"></v-text-field>
              </div>
              <button
                type="submit"
                v-on:click="login()"
                tag="button"
                class="btn btn-block btn-warning"
              >Login</button>
              <router-link
                to="/register"
                tag="button"
                class="btn btn-block btn-outline-primary"
              >Cadastrar</router-link>
            </div>
            <div id="resp"></div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>

import vars from '../plugins/env.local'

const url = `${vars.host}userController.php`

export default {

  methods: {
    getCookie(cname) {
      let name = cname + "=";
      let decodedCookie = decodeURIComponent(document.cookie);
      let ca = decodedCookie.split(';');
      for (let i = 0; i < ca.length; i++) {
        let c = ca[i];
        while (c.charAt(0) == ' ') {
          c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
          return c.substring(name.length, c.length);
        }
      }
      return "";
    },
    verifyLogin(cname) {
      if (localStorage.getItem('user-name')) {
        this.$router.push('Home')
      }
    },
    login() {
      document.getElementById('login').addEventListener('submit', e => {
        e.preventDefault()
        let form = new FormData(document.getElementById("login"));
        fetch(url, {
          method: "POST",
          body: form
        }).then(resp => {
          return resp.json()
        }).then(json => {
          // let json = JSON.parse(obj)
          console.log(json)
          // document.getElementById('resp').innerHTML = json
          if (json.status === 'ATIVO') {
            localStorage.setItem('user-name', json.name)
            localStorage.setItem('user-id', json.userId)
            localStorage.setItem('boxStatus', json.box_status[1])
            localStorage.setItem('boxId', json.box_status[0])
            this.verifyLogin('user-name')
          }
        })
      })
    }
  }
}
</script>
