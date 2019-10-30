<template>
  <div>
    <v-toolbar class="bg-primary fixed-top">
      <v-btn icon dark tag="router-link" to="boxlist">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title class="text-white text-center font-weight-bolder">Caixa</v-toolbar-title>
    </v-toolbar>
    <v-divider></v-divider>
    <v-divider></v-divider>
    <v-spacer class="mt-5"></v-spacer>
    <v-app class="container">
      <h5 class="border-bottom border-warning pb-2 w-50">Relátorio</h5>
      <div class="card mb-1 p-2 mt-2">
        <div class="d-flex justify-content-between text-primary">
          <span class>
            <span class="font-weight-bold">Recebimentos</span>
          </span>
          <div>
            <span class="font-weight-bold">R$ 5000,00</span>
          </div>
        </div>
      </div>
      <div class="card mb-1 p-2 mt-2">
        <div class="d-flex justify-content-between text-primary">
          <span class>
            <span class="font-weight-bold">Outras entradas</span>
          </span>
          <div>
            <span class="font-weight-bold">R$ 20,00</span>
          </div>
        </div>
      </div>
      <div class="card mb-1 p-2 mt-2">
        <div class="d-flex justify-content-between text-primary">
          <span class>
            <span class="font-weight-bold">Outras saídas</span>
          </span>
          <div>
            <span class="font-weight-bold">R$ 15,00</span>
          </div>
        </div>
      </div>
      <div class="card mb-1 p-2 mt-2 bg-primary">
        <div class="d-flex justify-content-between text-white">
          <span class>
            <v-icon class="text-white mr-2">mdi-coin</v-icon>
            <span class="font-weight-bold">Saldo do dia</span>
          </span>
          <div>
            <span class="font-weight-bold">R$ 20.000,00</span>
          </div>
        </div>
      </div>
      <v-divider></v-divider>
      <h5 class="border-bottom border-warning pb-2 w-50">Parcelas Recebidas</h5>
      <v-card tag="router-link"
            :to="'/parceldetails/' + 420"
            class="card mb-1 p-2 mt-2 text-decoration-none">
        <div class="d-flex justify-content-between">
          <span class="text-primary">
            <b>Vedas sistemas</b>
          </span>
          <div>
            <span class="text-primary font-weight-bold">3X</span>
          </div>
          <div>
            <span class="text-primary font-weight-bold">R$ 40,00</span>
          </div>
        </div>
      </v-card>
    </v-app>
  </div>
</template>
<script>
export default {

  data: () => ({
    boxParcels: [{
      user: 'Jhonatan',
      date: '20/04/2019',
      state: 'aberto',
      id: '22'
    }, {
      user: 'Eduardo',
      date: '30/04/2019',
      state: 'Fechado',
      id: '23'
    }, {
      user: 'Renato',
      date: '12/10/2019',
      state: 'Fechado',
      id: '24'

    }, {
      user: 'Gabriel',
      date: '10/10/2019',
      state: 'Fechado',
      id: '25'
    }, {
      user: 'Daiane',
      date: '28/10/2019',
      state: 'Fechado',
      id: '20'
    },],
  }),
  methods: {
    getBoxId() {
      this.$route.params.boxid
    }
  }
}
</script>

<style>
</style>