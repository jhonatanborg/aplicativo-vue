<template>
  <div>
    <v-card class="overflow-hidden">
      <v-app-bar right class="bg-warning">
        <v-toolbar-title>Contratos</v-toolbar-title>
        <div class="flex-grow-1"></div>
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      </v-app-bar>
      <v-sheet id="playground-example" class="overflow-y-auto">
        <v-container style="height:100%;">
<button type="button" name="" id="" class="btn btn-primary btn-lg btn-block">Novo contrato</button>
     </v-container>
      </v-sheet>
      <v-navigation-drawer v-model="drawer" absolute temporary right>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title class="title">Application</v-list-item-title>
            <v-list-item-subtitle>subtext</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <v-divider></v-divider>

        <v-list dense rounded>
          <v-list-item v-for="item in items" :key="item.title" link>
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title>
                <router-link :to="item.router">{{ item.title }}</router-link>
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
    </v-card>
  </div>
</template>
<script>
export default {
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
    components: [],

  })
}
</script>

<style>
</style>