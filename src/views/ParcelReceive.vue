<template>
  <div class="bg-app">
    <v-app-bar class="bg-primary">
      <v-toolbar-title class="text-white">
        <b>Vedas</b>
      </v-toolbar-title>
      <div class="flex-grow-1"></div>
      <router-link to="/home">
        <i class="mdi mdi-arrow-left icon-md text-white"></i>
      </router-link>
    </v-app-bar>
    <div id="resp"></div>
    <div class="container mt-2">
      <div v-for="dados  in installment" :key="dados.id">
        <h5 class="border-bottom border-warning pb-2 w-50">Dados da empresa</h5>
        <div class="card-n p-2">
          <h5 class="text-muted">Empresa</h5>
          <h5 class="text-primary">
            <h1 v-for="hp  in historicP" :key="hp.id">{{hp.amount}}</h1>
            <b>{{dados.company.company[0].name}}</b>
          </h5>
          <h5 class="text-muted">Responsável</h5>
          <h5 class="text-primary">
            <b>{{dados.company.company[0].owner_id[0].name}}</b>
          </h5>
        </div>
        <h5 class="border-bottom border-warning pb-2 mt-4 w-50">Dados da parcela</h5>
        <div class="card p-3" :class="background">
          <div class="d-flex justify-content-bettwen align-items-center">
            <v-icon large>{{icon}}</v-icon>
            <v-divider class="mr-2" vertical></v-divider>
            <div class="font-weight-bold mr-auto">
              <h6 class>Valor da parcela</h6>
              <span>R$</span>
              <span id="valueT">{{dados.value}}</span>
            </div>
            <div class="font-weight-bold">
              <h6>Vencimento</h6>
              <span class>{{dados.date}}</span>
            </div>
          </div>
        </div>
        <div class="card mb-1 p-2 mt-2 bg-primary">
          <div class="d-flex justify-content-between text-white">
            <span class>
              <v-icon class="text-white mr-2">mdi-coin</v-icon>
              <span class="font-weight-bold">Restante</span>
            </span>
            <div>
              <span class="font-weight-bold">R$ {{remaing}}</span>
            </div>
          </div>
        </div>
      </div>
      <v-app class="mt-4">
        <h5 class="border-bottom border-warning pb-2 mt-4 w-50">Historico de cobrança</h5>
        <div class="card-n p-3 mt-2" v-for="item  in historic" :key="item.id">
          <div class="d-flex justify-content-between">
            <div>
              <v-icon small class="pr-1">mdi-calendar-remove</v-icon>
              <small class="text-primary">
                <b>{{item.details}}</b>
              </small>
            </div>
            <div>
              <small class="text-primary mr-3">
                <b>{{item.date}}</b>
              </small>
              <small class="text-primary">
                <b>R$ {{item.amount}}</b>
              </small>
            </div>
          </div>
        </div>
        <div v-if="receive != 3">
          <v-bottom-sheet v-model="Nview">
            <template v-slot:activator="{ on }">
              <button type="submit" v-on="on" class="btn btn-block btn-danger mt-2">Não receber</button>
            </template>
            <v-sheet height="350px">
              <!-- <v-btn class="mt-6" color="error" @click="sheet = !sheet">close</v-btn> -->
              <div class="container">
                <h5 class="border-bottom border-warning pb-1 w-50">Motivo cancelamento</h5>
                <button
                  v-on:click="reasonSelected = 'Viajou'"
                  type="submit"
                  class="btn btn-block btn-outline-primary mt-2"
                >
                  <b>Viajou</b>
                </button>
                <button
                  v-on:click="reasonSelected = 'Não estava no local'"
                  type="submit"
                  class="btn btn-block btn-outline-primary mt-2"
                >
                  <b>Não estava no local</b>
                </button>
                <button
                  v-on:click="reasonSelected = 'Sem dinheiro'"
                  type="submit"
                  class="btn btn-block btn-outline-primary mt-2"
                >
                  <b>Sem dinheiro</b>
                </button>
                <button
                  v-on:click="reasonSelected = 'Estava fechado'"
                  type="button"
                  class="btn btn-block btn-outline-primary mt-2"
                >
                  <b>Estava fechado</b>
                </button>
                <button
                  type="submit"
                  @click="Nview = !Nview"
                  class="btn btn-block btn-danger mt-2"
                >Cancelar</button>
                <button
                  type="submit"
                  v-on:click="sendNoReceive"
                  class="btn btn-block btn-primary mt-2"
                >Confirmar</button>
              </div>
            </v-sheet>
          </v-bottom-sheet>
          <v-bottom-sheet v-model="Rview">
            <template v-slot:activator="{ on }">
              <button v-on="on" type="submit" class="btn btn-block btn-primary mt-2">Receber</button>
            </template>
            <v-sheet height="250px">
              <!-- <v-btn class="mt-6" color="error" @click="sheet = !sheet">close</v-btn> -->
              <div class="container">
                <div class="form-group">
                  <label class="border-bottom border-warning mt-4 w-50">
                    <b>Valor à receber</b>
                  </label>
                  <b>
                    <input
                      v-model="receiveValue"
                      type="text"
                      name
                      class="form-control"
                      placeholder="R$ 0,00"
                      aria-describedby="helpId"
                    />
                  </b>
                </div>
                <button
                  type="submit"
                  @click="Rview = !Rview"
                  class="btn btn-block btn-danger mt-2"
                >Cancelar</button>
                <button
                  type="submit"
                  v-on:click="sendReceive"
                  class="btn btn-block btn-primary mt-2"
                >Confirmar</button>
              </div>
            </v-sheet>
          </v-bottom-sheet>
          <div class="text-center">
            <v-snackbar v-model="snackbar" :multi-line="multiLine">
              {{ text }}
              <v-btn color="red" text @click="snackbar = false">Fechar</v-btn>
            </v-snackbar>
          </div>
        </div>
      </v-app>
    </div>
  </div>
</template>
<script>
import vars from '../plugins/env.local'
export default {
  mounted: function () {
    this.getInstallment()
  },
  data: () => ({
    historicP: '',
    multiLine: true,
    snackbar: false,
    text: 'Valor recebido com sucesso',
    Rview: false,
    installment: '',
    historic: [],
    dialog: false,
    Nview: false,
    reasonSelected: 'null',
    receiveValue: ' ',
    background: '',
    icon: '',
    receive: '',
    remaing: '',
  }),
  methods: {
    getInstallment() {
      const url = `${vars.host}parcelController.php`
      let formData = new FormData()
      formData.append('parcelinfo', 'true')
      formData.append('parcel-id', this.$route.params.id)
      localStorage.setItem('parcel-id', this.$route.params.id)
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
      }).then(json => {
        console.log(json)
        // document.getElementById('resp').innerHTML = json
        this.installment = json
        let array = []
        json.forEach(item => {
          if (item.historic)
            array.push(JSON.parse(item.historic))
        })
        array.forEach((item, key) => {
          this.historic = item
        })
        let status = (json[0].status)
        this.remaing = (json[0].remaing)

        console.log(status)
        switch (status) {
          case 'PENDENTE':
            this.background = "bg-warning",
              this.icon = "mdi mdi-information-outline text-dark"
            this.receive = 1
            break;
          case 'COBRADO':
            this.background = 'bg-danger text-white'
            this.icon = "mdi mdi-close-circle-outline text-white"
            this.receive = 2
            break;
          case 'RECEBIDA':
            this.background = 'bg-primary text-white'
            this.icon = "mdi mdi-checkbox-marked-circle-outline text-white"
            this.receive = 3
            break;
          default:
            break;
        }
      })
    },
    sendReceive() {
      let form = new FormData()
      let send = false
      if (parseFloat(this.receiveValue) == parseFloat(this.installment[0].value) || parseFloat(this.receiveValue) == parseFloat(this.remaing)) {
        form.append('status', 'RECEBIDA')
        form.append('details', 'Pagamento completo')
        send = true
      }
      if (parseFloat(this.receiveValue) < parseFloat(this.remaing) && parseFloat(this.receiveValue) > 0) {
        form.append('status', 'COBRADO')
        form.append('details', 'Pagamento parcial')
        send = true
      }
      let remaing = (this.remaing - this.receiveValue)
      if (remaing == 0) {
        form.append('status', 'RECEBIDA')
        form.append('details', 'Pagamento final')
      }
      form.append('remaing', remaing)
      form.append('user-id', localStorage.getItem('user-id'))
      form.append('box-id', localStorage.getItem('boxId'))
      form.append('id', this.$route.params.id)
      form.append('amount', this.receiveValue)
      form.append('pay', this.receiveValue)
      if (send) {
        const url = `${vars.host}parcelController.php`
        fetch(url, {
          method: 'POST',
          body: form
        }).then(resp => {
          return resp.json()
        }).then(json => {
          document.getElementById('resp').innerHTML = json
          console.log(json)
          this.Rview = false
          this.snackbar = true
          this.getInstallment()
        })
      } else {
        alert('Valor inválido')
      }
    },
    sendNoReceive() {
      console.log(this.reasonSelected)
      let form = new FormData()
      form.append('status', 'COBRADO')
      form.append('details', this.reasonSelected)
      form.append('remaing', this.remaing)
      form.append('user-id', localStorage.getItem('user-id'))
      form.append('box-id', localStorage.getItem('boxId'))
      form.append('id', this.$route.params.id)
      form.append('amount', 0)
      form.append('not-pay', 'true')
      const url = `${vars.host}parcelController.php`
      fetch(url, {
        method: 'POST',
        body: form
      }).then(resp => {
        return resp.json()
      }).then(json => {
        document.getElementById('resp').innerHTML = json
        console.log(json)
        this.Rview = false
        this.snackbar = true
        this.getInstallment()
      })
    }
  }
}
</script>

<style>
</style>