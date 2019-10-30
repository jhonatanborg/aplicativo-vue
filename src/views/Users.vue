<template>
  <div>
    <v-app-bar class="bg-primary fixed-top">
      <v-toolbar-title class="text-white">
        <b>Usuarios</b>
      </v-toolbar-title>
      <div class="flex-grow-1"></div>
      <router-link to="/home">
        <i class="mdi mdi-arrow-left icon-md text-white"></i>
      </router-link>
    </v-app-bar>
    <v-divider></v-divider>
    <v-divider></v-divider>
    <v-spacer class="mt-5"></v-spacer>
    <div class="container mt-4">
      <h5 class="border-bottom border-warning pb-2 w-50">Recentes</h5>
      <v-card tag="router-link"
            :to="'/userdetails/' + 4" class="card mb-1 p-2 mt-2">
        <div class="d-flex justify-content-between text-primary text-decoration-none">
          <span class>
            <span class="font-weight-bold">Jhonatan Borges</span>
          </span>
        </div>
      </v-card>
    <v-spacer class="mt-5"></v-spacer>
      <v-divider></v-divider>
      <h5 class="border-bottom border-warning pb-2 w-50">Usuários</h5>
      <div class="card mb-1 p-2 mt-2">
        <div class="d-flex justify-content-between text-primary">
          <span class>
            <span class="font-weight-bold">Jhonatan Borges</span>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style>
</style>