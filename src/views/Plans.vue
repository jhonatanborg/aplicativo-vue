<template>
  <div>
    <v-toolbar class="bg-primary">
      <v-btn icon dark tag="router-link" to="newloan">
        <v-icon>mdi mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title class="text-white text-center"><b class="text-white">Escolha o plano</b></v-toolbar-title>
    </v-toolbar>
    <div class="container pt-4 bg-app">
      <!--  -->
      <h5 class="border-bottom border-warning pb-2 w-50">Planos</h5>
      <div v-for="plan in plans" :key="plan.id">
        <div class="card-n mb-2 p-2">
          <router-link v-bind:to="'/plansdetails/'+ plan.id">
            <div class="media d-flex align-items-center">
              <i class="mdi mdi-coin-outline icon-md align-self-center mr-3 text-primary"></i>
              <div class="media-body">
                <h6 class="mb-1">R$ {{plan.value}}</h6>
                <p class="mb-0 text-muted"></p>
              </div>
            </div>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import vars from '../plugins/env.local'
export default {
  created: function () {
    // `this` points to the vm instance
    this.listAllPlans()
  },
  methods: {
    listAllPlans() {
      const url = `${vars.host}planController.php`
      let formData = new FormData()
      formData.append('all-plans', 'true')
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
      }).then(json => {
        json.forEach(item => {
          this.plans.push(item)
        })
        console.log(this.plans)
      })
    },
  },
  data: () => ({
    drawer: null,
    items: [{
      title: 'Home',
      icon: 'mdi-view-dashboard',
      router: '/home'
    }, {
      title: 'Novo Emprestimo',
      icon: 'mdi-image',
      router: '/login'
    }, {
      title: 'Empresas',
      icon: 'mdi-help-box',
      router: '/companies'
    },],
    plans: [],
  })

}


</script>
