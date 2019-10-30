<template>
  <v-app>
    <div>
      <v-toolbar class="bg-primary">
        <v-btn icon dark tag="router-link" to="home">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        <v-toolbar-title class="text-white text-center">Entrada</v-toolbar-title>
      </v-toolbar>
      <div id="resp"></div>
      <v-app>
        <div class="container pt-4">
          <h5 class="pt-4">Valor da entrada</h5>
          <v-text-field solo placeholder="0,00" v-model="valueInput" type="number"></v-text-field>
          <div class="mt-4">
            <h5>Selecione o motivo</h5>
            <v-select :items="select" v-model="reasonInput" label="Selecione a o motivo" solo></v-select>
          </div>
          <v-btn block color="red" class="mt-2 text-white">Cancelar</v-btn>
          <v-btn block class="primary mt-2" @click="openInput()">Confirmar</v-btn>
        </div>
      </v-app>
    </div>
  </v-app>
</template>
<script>

import vars from '../plugins/env.local'

const url = `${vars.host}cashflowController.php`

export default {
  data: () => ({
    select: ['Suprimento de caixa', 'Troco'],
    valueInput: null,
    reasonInput: null,
  }),
  methods: {
    openInput() {
      console.log(this.valueInput)
      console.log(this.reasonInput)
      let form = new FormData()
      form.append('add-cashflow', 'true')
      form.append('value', this.valueInput)
      form.append('reason', this.reasonInput)
      form.append('type', 'ENTRADA')
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