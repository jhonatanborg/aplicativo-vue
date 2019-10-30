<template>
  <div>
    <v-app-bar class="bg-primary">
      <v-toolbar-title>
        <b class="text-white">Confirmação</b>
      </v-toolbar-title>
      <div class="flex-grow-1"></div>
    </v-app-bar>
    <v-app>

    <div class="container mt-2">
      <h5 class="border-bottom border-warning pb-2 w-50">Dados da empresa</h5>
      <div class="card-n p-2">
        <h5 class="text-muted">Empresa</h5>
        <h5 class="text-primary">
          <b>{{dados.name}}</b>
        </h5>
        <h5 class="text-muted">Responsável</h5>
        <h5 v-for="dado in dados.owner" :key="dado.id" class="text-primary">
          <b>{{dado.name}}</b>
        </h5>
      </div>
      <h5 class="border-bottom border-warning pb-2 mt-4 w-50">Plano</h5>
      <div class="card-n p-2">
        <h5 class="text-muted">Valor do plano</h5>
        <h5 class="text-primary">
          <b>R$ {{valortotal}}</b>
        </h5>
        <h5 class="text-muted">Nº de parcelas</h5>
        <h5 class="text-primary">
          <b>{{quantidadeparcela}} parcelas</b>
        </h5>
      </div>
      <h5 class="border-bottom border-warning pb-2 mt-4 w-50">Mensalidades</h5>
      <div v-for="item in installments" :key="item[2]">
        <div class="card mb-1 p-3">
          <div class="d-flex justify-content-between">
            <span class="text-primary">
              <b>{{ item[2] }}x {{ item[0]}}</b>
            </span>
            <span class="text-primary">
              <b>{{ item[1] }}</b>
            </span>
          </div>
        </div>
      </div>
      <v-btn v-on:click="confirm()" block class="primary mt-2">Confirmar</v-btn>
    </div>
    </v-app>

  </div>
</template>
<script>

import vars from '../plugins/env.local'

export default {
  created: function () {
    // `this` points to the vm instance
    this.getPlan()
    this.getData()
    this.getInstallments()

  },
  data: () => ({
    valortotal: '',
    quantidadeparcela: ' ',
    valor: ' ',
    installments: [],
    curdate: [],
    radios: ' ',
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
    plan: '',
    dados: ''
  }),
  methods: {
    getInstallments() {
      this.valortotal = localStorage.getItem('total')
      let today = new Date();
      let d = today.getDate() + 1
      let m = today.getMonth()
      let y = today.getFullYear()
      let obj = []
      let verificar = []
      let curdate = []
      let number = 1
      const par = localStorage.getItem('plano')
      let localplan = localStorage.getItem('radios')
      let arrayPlan = localplan.split('-', 2);
      this.valor = arrayPlan[0]
      this.quantidadeparcela = arrayPlan[1]

      let cobrar = localStorage.getItem('checkedNames') || ''
      for (let i = 0; i < arrayPlan[1]; i++) {
        let date = new Date(y, m, d + i)
        let array
        let dataStr = date.toString()
        if (dataStr.indexOf('Sat') < 0) {
          array = [this.valor, date.toLocaleDateString('pt-br'), number, 'cobrar']
        }
        if (dataStr.indexOf('Sun') < 0) {
          array = [this.valor, date.toLocaleDateString('pt-br'), number, 'cobrar']
        }
        if (cobrar.indexOf('1') < 0 && dataStr.indexOf('Sat') >= 0) {
          array = [this.valor, date.toLocaleDateString('pt-br'), number, 'não-cobrar']
          arrayPlan[1]++
          number--
        }
        if (cobrar.indexOf('2') < 0 && dataStr.indexOf('Sun') >= 0) {
          array = [this.valor, date.toLocaleDateString('pt-br'), number, 'não-cobrar']
          arrayPlan[1]++
          number--
        }
        verificar.push(array)
        if (array[3] === 'cobrar')
          obj.push(array)
        number++
      }
      console.log(verificar)
      // console.log(obj)
      this.installments = obj;
    },
    getPlan() {
      const url = `${vars.host}planController.php`
      let formData = new FormData()
      formData.append('plan', 'true')
      formData.append('plan-id', this.$route.params.id)
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
        console.log(json)
      }).then(json => {
        json.forEach(item => {
          this.plan = item
        })
      })
    },
    getData() {
      if (localStorage.getItem('company-id')) {
        const url = `${vars.host}companyController.php`
        let formData = new FormData()
        formData.append('this-company', 'true')
        formData.append('company-id', localStorage.getItem('company-id'))
        fetch(url, {
          method: 'POST',
          body: formData
        }).then(resp => {
          return resp.json()
        }).then(json => {
          json.forEach(item => {
            this.dados = item
          })
        })
      } else {
        this.$router.push('newloan')
      }
    },
    confirm() {
      const url = `${vars.host}contractController.php`
      let form = new FormData()
      form.append('confirm', 'true')
      let json = JSON.stringify(this.installments)
      form.append('parcelas', json)
      form.append('plan-id', localStorage.getItem('plan-id'))
      form.append('company-id', localStorage.getItem('company-id'))
      form.append('user-id', localStorage.getItem('user-id'))
      fetch(url, {
        method: 'POST',
        body: form
      }).then(resp => {
        return resp.json()
      }).then(json => {
        console.log(json)
        // document.getElementById("respp").innerHTML = json
        if (json.msg.indexOf('Sucesso') > 0) {
          alert(json.msg)
          this.$router.push('home')
        }
      })
    }
  },
}
</script>

<style>
</style>