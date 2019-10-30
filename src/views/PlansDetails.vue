<template>
  <div>
    <v-toolbar class="bg-primary">
      <v-btn icon dark tag="router-link" to="/plans">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title class="text-white"> <b class="text-white">Plano: R$ {{plan.value}}</b></v-toolbar-title>
    </v-toolbar>
    <v-app>
      <div class="card-n mt-5 pt-5">
        <div class="card-body">
          <div v-for="dado in dados.owner" :key="dado.id">
            <h4>{{dados.name}}</h4>
            <span class="text-muted">{{dado.name}}</span>
          </div>
          <h4>
            Valor Total: R$
            <b>{{plan.value}}</b>
          </h4>
          <v-divider></v-divider>
          <h4>Escolha a quantidade de parcelas:</h4>
          <div v-if="errors.length">
            <b>Por favor escolha um plano</b>
            <ul>
              <li v-for="error in errors" :key="error">{{ error }}</li>
            </ul>
          </div>
          <div class="d-flex justify-content-between p-2">
            <div class="form-check">
              <h4 class="form-check-label">
                <input
                  type="radio"
                  class="form-check-input"
                  name="plano"
                  v-model="radios"
                  :value="`${plan.p11}-11`"
                />
                11 vezes de {{plan.p11}}
                <i class="input-helper"></i>
              </h4>
            </div>
            <div class="form-check">
              <h4 class="form-check-label">
                <input
                  type="radio"
                  class="form-check-input"
                  name="plano"
                  v-model="radios"
                  :value="`${plan.p24}-24`"
                />
                24 vezes de {{plan.p24}}
                <i class="input-helper"></i>
              </h4>
            </div>
          </div>
          <v-divider></v-divider>
          <h4>Dias para cobrar:</h4>
          <div class="d-flex justify-content-between p-2">
            <div class="form-check form-group-md">
              <label class="form-check-label">
                <input
                  type="checkbox"
                  class="form-check-input"
                  id="sabado"
                  value="1"
                  v-model="checkedNames"
                /> Sábado
                <i class="input-helper"></i>
              </label>
            </div>
            <div class="form-check form-group-md">
              <label class="form-check-label">
                <input
                  type="checkbox"
                  class="form-check-input"
                  id="domingo"
                  value="2"
                  v-model="checkedNames"
                />Domingo
                <i class="input-helper"></i>
              </label>
            </div>
            <footer class="fixed-bottom p-2">
              <v-btn block to="/home" tag="router-link" color="red" class="mt-2 text-white">Cancelar</v-btn>
              <v-btn v-on:click="salvar()" block class="primary mt-2">Confirmar</v-btn>
            </footer>
          </div>
        </div>
      </div>
    </v-app>
  </div>
</template>
<script>
import { log } from 'util'
import vars from '../plugins/env.local'
export default {
  created: function () {
    // `this` points to the vm instance
    this.getPlan()
    this.getData()
  },
  data: () => ({
    checkedNames: [],
    errors: [],
    radios: '',
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
  watch: {

    radios(newRadio) {
      localStorage.radios = newRadio;
    },
    checkedNames(newCkeck) {
      localStorage.checkedNames = newCkeck;
    }
  },
  methods: {
    salvar() {
      if (this.radios) {
        console.log("deu certo")
        localStorage.setItem('total', this.plan.value);
      } else {
        console.log('deu errada')
      }
      if (!this.radios) {
        this.errors.push('escolher um plano é obrigatorio');
      }
      this.$router.push('/confirmcontract')
    },
    getPlan() {
      const url = `${vars.host}planController.php`
      let formData = new FormData()
      formData.append('plan', 'true')
      formData.append('plan-id', this.$route.params.id)
      localStorage.setItem('plan-id', this.$route.params.id)
      fetch(url, {
        method: 'POST',
        body: formData
      }).then(resp => {
        return resp.json()
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
            console.log(this.dados.name);

          })
        })
      } else {
        this.$router.push('newloan')
      }
    }

  },

}


</script>