<template>
  <v-app>
    <v-toolbar class="bg-primary fixed-top">
      <v-btn icon dark tag="router-link" to="home">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title class="text-white text-center font-weight-bolder">Caixas</v-toolbar-title>
    </v-toolbar>
    <v-app>
      <div class="container">
        <v-divider></v-divider>
        <v-divider></v-divider>
        <v-divider></v-divider>
        <h5 class="border-bottom border-warning pb-2 mt-4 w-50">Pesquisar caixas</h5>
        <v-dialog ref="dialog" v-model="modal" :return-value.sync="date" persistent>
          <template v-slot:activator="{ on }">
            <v-spacer></v-spacer>
            <v-text-field
              v-model="date"
              solo
              label="Picker in dialog"
              prepend-icon="mdi-calendar"
              readonly
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker full-width locale="pt-BR" v-model="date" scrollable>
            <v-spacer></v-spacer>
            <v-btn text color="primary" @click="modal = false">Cancel</v-btn>
            <v-btn text color="primary" @click="$refs.dialog.save(date)">OK</v-btn>
          </v-date-picker>
        </v-dialog>
        <div class="mx-auto w-50 mt-4 mb-4">
          <h5 class="border-bottom border-warning text-center"></h5>
        </div>
        <div v-for="item in BoxList" :key="item.id">
          <v-card
            tag="router-link"
            :to="'/boxdetails/' + item.id"
            class="card mb-1 p-2 mt-2 text-decoration-none"
          >
            <div class="d-flex justify-content-between text-primary">
              <span class>
                <v-icon class="text-primary mr-2">mdi-folder-lock-open</v-icon>
                <span class="font-weight-bold">{{item.user}}</span>
              </span>
              <div>
                <span class="font-weight-bold">{{item.date}}</span>
              </div>
            </div>
          </v-card>
        </div>
      </div>
    </v-app>
  </v-app>
</template>
<script>
import vars from '../plugins/env.local'

export default {
  mounted: function () {
    this.getBoxDay()
  },
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    menu: false,
    modal: false,
    menu2: false,
    boxId: 20,
    BoxList: [{
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
    getBoxDay() {
      const url = `${vars.host}boxController.php`
      let form = new FormData()
      form.append('get-box-day', 'true')
      form.append('date', this.date)
      fetch(url, {
        method: "POST",
        body: form
      }).then(resp => {
        return resp.text()
      }).then(json => {
        // let json = JSON.parse(obj)
        console.log(json)
        // alert(json.msg)
        // document.getElementById('resp').innerHTML = json
      })
    }
  }
}
</script>

<style>
.Fechado {
  background-color: #0275d8;
}
</style>