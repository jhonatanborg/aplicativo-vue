<template>
  <div>
    <v-toolbar class="bg-primary fixed-top">
      <v-btn icon dark tag="router-link" to="home">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title class="text-white text-center">Novo empréstimo</v-toolbar-title>
      <div class="flex-grow-1"></div>
      <router-link to="/responsible">
        <i class="mdi mdi-account-plus icon-md text-white"></i>
      </router-link>
    </v-toolbar>
    <v-divider></v-divider>mt-1
    <v-container class="bg-app">
      <div class="container p-1 pt-2 bg-app">
        <div class="form-group">
          <v-text-field label="Buscar" outlined></v-text-field>
        </div>
        <h5 class="border-bottom border-warning pb-2 w-50">Empresas</h5>
        <div class="template-demo">
          <div
            v-for="company in companies"
            :id="company.id"
            v-on:click="redirect(company.id)"
            :key="company.id"
            class="card-n rounded mb-2"
          >
            <div class="card-body p-3">
              <div class="media d-flex align-items-center">
                <div class="media-body">
                  <h6 class="mb-1 font-weight-bold text-primary">{{company.name}}</h6>
                  <p class="mb-0 text-muted"></p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </v-container>
  </div>
</template>
<script>

import vars from '../plugins/env.local'

export default {

  created: function () {
    // `this` points to the vm instance
    this.listAllCompanies()
  },
  data: () => ({
    components: [],
    companies: []
  }),
  methods: {
    listAllCompanies() {
      this.comp
      const url = `${vars.host}companyController.php`
      let formData = new FormData()
      formData.append('all-companies', 'true')
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
      }).then(json => {
        json.forEach(item => {
          this.companies.push(item)
        })
        console.log(this.companies)
      })
    },
    redirect(id) {
      // e.preventDefault()
      localStorage.setItem('company-id', id)
      if (id) {
        this.$router.push('plans')
      }
      console.log(localStorage.getItem('company-id'))
    }
  }
}
</script>