<template>
  <div>
    <v-app-bar class="bg-warning fixed-top">
      <v-toolbar-title>Nova Empresa</v-toolbar-title>
      <div class="flex-grow-1"></div>
      <router-link to="/responsible">
        <i class="mdi mdi-arrow-left icon-md text-dark"></i>
      </router-link>
    </v-app-bar>
    <div class="mt-5">
      <div class="container">
        <div class>
          <form id="register-company">
            <input type="hidden" name="register-company" />
            <div class="login p-1 py-4">
              <div class="form-group">
                <label for>Nome comercial</label>
                <input
                  type="text"
                  name="name"
                  id="name"
                  class="form-control form-control-sm"
                  placeholder="Ex: Pedro Barbosa"
                />
              </div>
              <div class="form-group">
                <label for>Endereço comercial completo</label>
                <input type="text" name="street" class="form-control form-control-sm" />
              </div>
              <div class="form-group row">
                <div class="col">
                  <label>Bairro</label>
                  <div id="the-basics">
                    <input
                      id="district"
                      name="district"
                      class="form-control form-control-sm"
                      aria-describedby="helpId"
                    />
                  </div>
                </div>
                <div class="col">
                  <label>Numero</label>
                  <div id="bloodhound">
                    <input
                      id="number"
                      name="number"
                      class="form-control form-control-sm"
                      aria-describedby="helpId"
                    />
                  </div>
                </div>
              </div>
              <div class="form-group">
                <label for>CEP</label>
                <input
                  id="cep"
                  name="cep"
                  class="form-control form-control-sm"
                  aria-describedby="helpId"
                />
              </div>
              <div class="form-group">
                <label for>Telefone</label>
                <input
                  placeholder="Ex: (66) 99999-9999"
                  id="tel"
                  name="tel"
                  class="form-control form-control-sm"
                />
              </div>
              <button
                v-on:click="register()"
                type="submit"
                class="btn btn-block btn-warning"
              >Avançar</button>
            </div>
            <input id="owner-id" type="hidden" name="owner-id" />
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import vars from '../plugins/env.local'
const url = `${vars.host}companyController.php`
export default {
  methods: {
    register() {
      if (localStorage.getItem('owner-id')) {
        document.getElementById('owner-id').value = localStorage.getItem('owner-id')
        document.getElementById("register-company").addEventListener("submit", event => {
          event.preventDefault();
          let form = new FormData(document.getElementById("register-company"));
          fetch(url, {
            method: "POST",
            body: form
          }).then(resp => {
            return resp.json()
          }).then(json => {
            console.log(json)
            // let json = JSON.parse(obj)
            if (json.msg.indexOf("sucesso") != -1) {
              localStorage.removeItem('owner-id')
              localStorage.setItem('company-id', json.company_id)
              this.$router.push('plans')
            }
          })
        })
      } else {
        console.log('Ocorreu algum erro ao cadastrar o proprietário da empresa')
      }
    }
  }
}
</script>