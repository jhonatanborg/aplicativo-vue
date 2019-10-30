<template class="bg-app">
  <div class="bg-app">
    <v-card class="overflow-hidden">
      <v-app-bar class="bg-primary text-white">
        <v-toolbar-title>
          <b>Vedas</b>
        </v-toolbar-title>
        <div class="flex-grow-1"></div>
        <v-app-bar-nav-icon class="text-white" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      </v-app-bar>
      <div class="card-n bg-primary text-white">
        <div class="card-body">
          <h2 class="text-center text-white">
            <span class>R$</span>
            <b>30,00</b>
          </h2>
          {{nameUser}}
          <p class="text-center text-warning">Total recebido</p>
        </div>
      </div>
        <div v-for="installment in installments" :key="installment.id" class="text-decoration-none">
          <router-link v-bind:to="'/parcelreceive/'+ installment.id" class="text-decoration-none">
            <div class="card mb-1 p-3">
              <div class="d-flex justify-content-between">
                <span class="text-primary">
                  <b>{{installment.company.company[0].name}}</b>
                </span>
                <div :class="installment.status">
                  <span class="personal badge badge-pill">{{installment.status}}</span>
                </div>
              </div>
            </div>
          </router-link>
        </div>
      <v-app></v-app>
      <v-navigation-drawer overlay-color="red" v-model="drawer" absolute right temporary>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title class="title">
              <b>{{username}}</b>
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-divider></v-divider>
        <v-list dense nav hide-overlay>
          <v-list-item
            class="text-decoration-none"
            v-for="item in items"
            :key="item.status"
            link
            tag="router-link"
            :to="item.router"
          >
            <v-list-item-icon class="mr-3">
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
      <v-footer>
        <v-bottom-navigation :value="activeBtn" grow color="teal" fixed>
          <v-btn v-on:click="installmentsReceiveds()">
            <span>Recebidos</span>
            <v-icon>mdi-calendar-check</v-icon>
          </v-btn>

          <v-btn v-on:click="installmentsPendents()">
            <span>Pendentes</span>
            <v-icon>mdi-calendar-multiple</v-icon>
          </v-btn>
          <v-btn v-on:click="installmentsCharged()">
            <span>Cobrados</span>
            <v-icon>mdi-calendar-remove</v-icon>
          </v-btn>
        </v-bottom-navigation>
      </v-footer>
    </v-card>
  </div>
</template>
<script>
import vars from '../plugins/env.local'

export default {
  mounted: function () {
    // `this` points to the vm instance
    this.installmentsPendents()
    this.verifyBox()
  },
  updated: function () {
    this.verifyBox()
  },
  data: () => ({
    selectRouter: '',
    valueBoxOpen: '',
    username: localStorage.getItem('user-name'),
    datenow: '',
    select: ['Sinop', 'Sorriso', 'Santa Carmem', 'Claudia'],
    dialog: false,
    dialogCloseBox: false,
    search: '',
    activeBtn: 1,
    bottomNav: null,
    drawer: null,
    stateBox: ' ',
    nameUser: '',
    items: [{
      title: 'Home',
      icon: 'mdi-view-dashboard',
      router: '/home'
    }, {
      title: 'Novo Emprestimo',
      icon: 'mdi mdi-tab-plus',
      router: '/newloan'
    }, {
      title: 'Empresas',
      icon: 'mdi-account-multiple',
      router: '/companies'
    }, {
      title: 'Entrada',
      icon: 'mdi-code-greater-than',
      router: '/inputs'
    }, {
      title: 'Saída',
      icon: 'mdi-code-less-than',
      router: '/exits'
    },],
    components: [],
    installments: [],
    a: true,
  }),
  methods: {

    installmentsPendents() {
      const url = `${vars.host}parcelController.php`
      let formData = new FormData()
      formData.append('pendents-installments', 'true')
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
      }).then(json => {
        this.installments = json
        // console.log(location)
      })
    },
    installmentsReceiveds() {
      const url = `${vars.host}parcelController.php`
      let formData = new FormData()
      formData.append('receiveds-installments', 'true')
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
      }).then(json => {
        console.log(json)
        this.installments = json
      })
    },
    installmentsCharged() {
      const url = `${vars.host}parcelController.php`
      let formData = new FormData()
      formData.append('charged-installments', 'true')
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
      }).then(json => {
        this.installments = json
      })
    },
    openBox() {
      localStorage.setItem('boxStatus', this.stateBox)
      const url = `${vars.host}boxController.php`
      let formData = new FormData()
      formData.append('open-box', 'true')
      formData.append('value', this.valueBoxOpen)
      formData.append('route', this.selectRouter)
      formData.append('user-id', localStorage.getItem('user-id'))
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
      }).then(json => {
        console.log(json)
        if (json.boxInfo) {
          alert(json.msg)
          localStorage.setItem('boxStatus', 'ABERTO')
          localStorage.setItem('boxId', json.boxInfo.boxId)
        } else {
          localStorage.setItem('boxStatus', json.statusBox)
          localStorage.setItem('boxId', json.boxId)
        }
        document.getElementById('resp').innerHTML = json
        // this.dialog = false
      })
    },
    closeBox() {
      this.stateBox = 'FECHADO'
      localStorage.setItem('boxStatus', this.stateBox)
      const url = `${vars.host}boxController.php`
      let formData = new FormData()
      formData.append('close-box', 'true')
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.text()
      }).then(json => {
        document.getElementById('resp').innerHTML = json
      })

    },
    verifyBox() {
      let date = new Date(), mouth = date.getMonth() + 1
      this.datenow = date.getDate() + '/' + mouth + '/' + date.getFullYear()
      this.stateBox = localStorage.getItem('boxStatus')
    },
    userVerify() {
      this.nameUser = localStorage.getItem('user-name')
    }
  },
}
</script>

<style>
.PENDENTE .personal {
  color: #212529;
  background-color: #ffc100;
}
.RECEBIDA .personal {
  color: #fff;
  background-color: #4d83ff;
}

.COBRADO .personal {
  color: #fff;
  background-color: #ff4747;
}
</style>