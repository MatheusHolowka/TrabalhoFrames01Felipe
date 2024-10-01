<template>
  <v-app>
    <!-- Toolbar -->
    <v-toolbar color="primary">
      <v-toolbar-title>Smart Eventos</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="mr-2">
        <v-btn text to="/" prepend-icon="mdi-home">Home</v-btn>
        <v-btn text to="/cadastrar" prepend-icon="mdi-account">Cadastro de Eventos</v-btn>
        <v-btn text to="/apagar" prepend-icon="mdi-account-group">Gerenciar Eventos</v-btn>
      </v-toolbar-items>
    </v-toolbar>

    <!-- Main Content -->
    <v-main>
      <router-view />
    </v-main>

    <!-- Footer -->
    <AppFooter />
  </v-app>
</template>

<script setup>
  // Script setup block
</script>