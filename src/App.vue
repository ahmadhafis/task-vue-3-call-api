<script setup>
import { RouterLink, RouterView } from 'vue-router'
import AccountList from './components/AccountList.vue'
</script>

<template>
  <v-app>
    <v-container>
      <AccountList />
    </v-container>
  </v-app>
</template>

<script>
export default {
  components: {
    AccountList,
  },
}
</script>
