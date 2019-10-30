<template class="bg-app">
  <div class="bg-app">
    <v-card class="overflow-hidden">
      <v-app-bar fixed class="bg-primary text-white">
        <v-toolbar-title>
          <b>Vedas</b>
        </v-toolbar-title>
        <div class="flex-grow-1"></div>
        <v-btn icon tag="router-link" to="newloan">
          <v-icon class="text-white">mdi mdi-tab-plus</v-icon>
        </v-btn>
        <v-btn icon v-if="stateBox != 'ABERTO'">
          <v-icon @click="dialog = true" class="text-white">mdi mdi-inbox-arrow-down</v-icon>
        </v-btn>
        <v-btn icon v-if="stateBox == 'ABERTO'">
          <v-icon @click="dialogCloseBox = true" class="text-white">mdi mdi-package-up</v-icon>
        </v-btn>
        <v-app-bar-nav-icon class="text-white" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      </v-app-bar>
      <v-app class="pt-5">
        <div class="card-n bg-primary text-white py-5">
          <div class="card-body p-4">
            <h2 class="text-center text-white">
              <span class>R$</span>
              <b>30,00</b>
            </h2>
            {{nameUser}}
            <p class="text-center text-warning">Total recebido</p>
          </div>
        </div>
        <v-sheet>
          <v-container class="bg-app">
            <v-text-field label="Buscar" outlined></v-text-field>
            <h5 class="border-bottom border-warning pb-2 w-50">Parcelas</h5>
            <div
              v-for="installment in installments"
              :key="installment.id"
              class="text-decoration-none"
            >
              <router-link
                v-bind:to="'/parcelreceive/'+ installment.id"
                class="text-decoration-none"
              >
                <v-card class="card mb-2 p-2">
                  <div class="d-flex justify-content-between">
                    <span class="text-primary">
                      <b>{{installment.company.company[0].name}}</b>
                    </span>
                    <div :class="installment.status">
                      <span class="personal badge badge-pill">{{installment.status}}</span>
                    </div>
                  </div>
                </v-card>
              </router-link>
            </div>
          </v-container>
        </v-sheet>
        <v-dialog
          v-model="dialog"
          fullscreen
          hide-overlay
          transition="dialog-bottom-transition"
          class="bg-app"
        >
          <v-card tile>
            <v-toolbar class="bg-primary">
              <v-btn icon dark @click="dialog = false">
                <v-icon>mdi-close</v-icon>
              </v-btn>
              <v-toolbar-title class="text-white text-center">Abrir caixa</v-toolbar-title>
            </v-toolbar>
            <div class="container bg-app mt-4">
              <h5>Funcionário</h5>
              <div class="card mb-1 p-2 mt-2 text-primary">
                <div class="d-flex justify-content-between text-white">
                  <span class="text-primary">
                    <v-icon class="text-primary">mdi mdi-account-check</v-icon>
                  </span>
                  <div>
                    <span class="font-weight-bold text-primary">{{username}}</span>
                  </div>
                </div>
              </div>
              <div class="card mb-1 p-2 mt-4">
                <div class="d-flex justify-content-between text-white">
                  <span class="text-primary">
                    <v-icon class="text-primary">mdi mdi-calendar-clock</v-icon>
                  </span>
                  <div>
                    <span class="font-weight-bold mr-2 text-primary">Data:</span>
                    <span class="font-weight-bold text-primary">{{datenow}}</span>
                  </div>
                </div>
              </div>
              <div class="mx-auto w-50 mt-4">
                <h5 class="border-bottom border-warning pb-2 text-center"></h5>
              </div>
              <div class="mt-5">
                {{selectRouter}}
                <h5>Selecione a rota</h5>
                <v-select :items="select" v-model="selectRouter" label="Selecione a rota" solo></v-select>
              </div>
              <div class>
                {{valueBoxOpen}}
                <h5>Suprimento inicial</h5>
                <div id="resp">NÃO EXCLUI</div>
                <v-text-field v-model="valueBoxOpen" placeholder="0,00" solo></v-text-field>
              </div>
              <v-btn block color="red" @click="dialog = false" class="mt-2 text-white">Cancelar</v-btn>
              <v-btn block class="primary mt-2" @click="openBox()">Confirmar</v-btn>
            </div>
          </v-card>
        </v-dialog>
        <v-dialog
          v-model="dialogCloseBox"
          fullscreen
          hide-overlay
          transition="dialog-bottom-transition"
          class="bg-app"
        >
          <v-card tile>
            <v-toolbar class="bg-primary">
              <v-btn icon dark @click="dialogCloseBox = false">
                <v-icon>mdi-close</v-icon>
              </v-btn>
              <v-toolbar-title class="text-white text-center">Fechar caixa</v-toolbar-title>
            </v-toolbar>
            <div class="container bg-app mt-4">
              <div class="card mb-1 p-2 mt-2 bg-primary">
                <div class="d-flex justify-content-between text-white">
                  <span class>
                    <v-icon class="text-white mr-2">mdi mdi-calendar-clock</v-icon>
                    <span class="font-weight-bold">Data</span>
                  </span>
                  <div>
                    <span class="font-weight-bold">{{datenow}}</span>
                  </div>
                </div>
              </div>
              <div class="card mb-1 p-2 mt-2">
                <div class="d-flex justify-content-between text-primary">
                  <span class>
                    <v-icon class="text-primary mr-2">mdi-coin</v-icon>
                    <span class="font-weight-bold">Saldo anterior</span>
                  </span>
                  <div>
                    <span class="font-weight-bold">R$ {{oldValue}}</span>
                  </div>
                </div>
              </div>

              <!-- asdasdasd -->

              <div class="card mb-1 p-2 mt-2">
                <div class="d-flex justify-content-between text-primary">
                  <span class>
                    <v-icon class="text-primary mr-2">mdi-coin</v-icon>
                    <span class="font-weight-bold">Saldo Total</span>
                  </span>
                  <div>
                    <span class="font-weight-bold">R$ {{totalValue}}</span>
                  </div>
                </div>
              </div>
              <!-- asdasdasd -->

              <div class="mx-auto w-50 mt-4">
                <h5 class="border-bottom border-warning pb-2 text-center"></h5>
              </div>
              <div class="mt-5">
                <h5 class="border-bottom border-warning pb-2 w-50">Relátorio</h5>
                <div class="card mb-1 p-2 mt-2">
                  <div class="d-flex justify-content-between text-primary">
                    <span class>
                      <span class="font-weight-bold">Recebimentos</span>
                    </span>
                    <div>
                      <span class="font-weight-bold">R$ {{receiveds}}</span>
                    </div>
                  </div>
                </div>
                <div class="card mb-1 p-2 mt-2">
                  <div class="d-flex justify-content-between text-primary">
                    <span class>
                      <span class="font-weight-bold">Outras entradas</span>
                    </span>
                    <div>
                      <span class="font-weight-bold">R$ {{inputs}}</span>
                    </div>
                  </div>
                </div>
                <div class="card mb-1 p-2 mt-2">
                  <div class="d-flex justify-content-between text-primary">
                    <span class>
                      <span class="font-weight-bold">Outras saídas</span>
                    </span>
                    <div>
                      <span class="font-weight-bold">R$ {{outputs}}</span>
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
                      <span class="font-weight-bold">R$ {{valueDay}}</span>
                    </div>
                  </div>
                </div>
              </div>
              <v-spacer></v-spacer>
              <div class="mt-5">
                <v-btn
                  block
                  color="red"
                  @click="dialogCloseBox = false"
                  class="mt-2 text-white"
                >Cancelar</v-btn>
                <v-btn block class="primary mt-2" v-on:click="closeBox()
                ">Confirmar</v-btn>
              </div>
            </div>
          </v-card>
        </v-dialog>
      </v-app>
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
          <v-divider></v-divider>
          <v-list-item
            class="text-decoration-none"
            v-for="item in itemsAdm"
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
        <v-bottom-navigation :value="activeBtn" color="red" fixed>
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
    this.getBoxValues()
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
    oldValue: 0,
    receiveds: 0,
    inputs: 0,
    outputs: 0,
    valueDay: 0,
    totalValue: 0,
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

    itemsAdm: [{
      title: 'Caixas',
      icon: 'mdi-animation',
      router: '/boxlist'
    }, {
      title: 'Usuários',
      icon: 'mdi-account-box-outline',
      router: '/users'
    },
    {
      title: 'Usuários',
      icon: 'mdi-account-box-outline',
      router: '/users'
    }, 
     {
      title: 'Contratos',
      icon: 'mdi-account-box-outline',
      router: '/listcontracts'
    },
    
    ],
    components: [],
    installments: [],
    a: true,
    state: "fixed-top",
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
        // this.stateBox = 'FECHADO'
        console.log(json)
      })

    },
    getBoxValues() {
      localStorage.setItem('boxStatus', this.stateBox)
      const url = `${vars.host}boxController.php`
      let formData = new FormData()
      formData.append('get-box-values', 'true')
      formData.append('box-id', localStorage.getItem('boxId'))
      formData.append('user-id', localStorage.getItem('user-id'))
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.text()
      }).then(json => {
        // document.getElementById('resp').innerHTML = json
        // this.stateBox = 'FECHADO'
        // this.inputs = json.inputs
        // this.outputs = json.outputs
        // this.receiveds = json.receiveds
        // this.totalValue = json.valueTotal
        // this.oldValue = json.boxInfo.openValue
        console.log(json)
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