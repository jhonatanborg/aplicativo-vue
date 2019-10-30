<template>
  <v-app>
    <div>
      <v-toolbar class="bg-primary">
        <v-btn icon dark tag="router-link" to="home">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        <v-toolbar-title class="text-white text-center">
          <b class="text-white">Saída</b>
        </v-toolbar-title>
      </v-toolbar>
      <div class="container bg-app pt-4">
        <h5 class="pt-4">Valor saída</h5>
        <v-text-field solo placeholder="0,00" v-model="valueExit" type="number"></v-text-field>
        <div class="mt-4">
          <h5>Selecione o motivo</h5>
          <v-select :items="select" v-model="reasonExit" label="Selecione a saída" solo></v-select>
        </div>
        <v-btn block color="red" class="mt-2 text-white">Cancelar</v-btn>
        <v-btn block class="primary mt-2" @click="openExit()">Confirmar</v-btn>
      </div>
    </div>
  </v-app>
</template>
<script>

import vars from '../plugins/env.local'

const url = `${vars.host}cashflowController.php`

export default {
  data: () => ({
    select: ['Almoço', 'Lanche', 'Combustivel', 'Vale', 'Outros'],
    valueExit: null,
    reasonExit: null,
  }),
  methods: {
    openExit() {
      console.log(this.valueExit)
      console.log(this.reasonExit)
      let form = new FormData()
      form.append('add-cashflow', 'true')
      form.append('value', this.valueExit)
      form.append('reason', this.reasonExit)
      form.append('type', 'SAIDA')
      form.append('user-id', localStorage.getItem('user-id'))
      form.append('box-id', localStorage.getItem('boxId'))
      fetch(url, {
        method: "POST",
        body: form
      }).then(resp => {
        return resp.json()
      }).then(json => {
        // let json = JSON.parse(obj)
        console.log(json)
        alert(json.msg)
        // document.getElementById('resp').innerHTML = json
      })
    }
  }
}
</script>

<style>
</style>