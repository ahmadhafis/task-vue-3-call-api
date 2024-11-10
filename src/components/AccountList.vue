<template>
  <v-container>
    <v-card>
      <v-card-title style="background-color: antiquewhite;">Account List</v-card-title>
      <v-card-text>
        <v-list>
          <v-list-item v-for="account in accounts" :key="account.id">
            <v-list-item-content>
              <v-list-item-title>{{ account.account_id }}</v-list-item-title>
              <v-list-item-title>{{ account.name }}</v-list-item-title>
              <v-list-item-subtitle>Balance: ${{ account.balance }}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-card-text>
    </v-card>

    <v-card class="mt-4">
      <v-card-title style="background-color: aquamarine;">Summary</v-card-title>
      <v-card-text>
        <p><strong>Total Accounts:</strong> {{ totalAccounts }}</p>
        <p><strong>Total Balance:</strong> Rp {{ totalBalance }}</p>
        <p><strong>Average Balance:</strong> Rp {{ averageBalance.toFixed(2) }}</p>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios';

  export default {
    data() {
      return {
        accounts: [],
        error: null,
      };
    },
    computed: {
      totalAccounts() {
        return this.accounts.length;
      },
      totalBalance() {
        return this.accounts.reduce((acc, account) => acc + account.balance, 0);
      },
      averageBalance() {
        return this.totalBalance / this.totalAccounts || 0;
      },
    },
    methods: {
      async fetchAccounts() {
        try {
          const response = await axios.get('http://localhost:8080/account/list');
          this.accounts = response.data.data;
        } catch (error) {
          this.error = error.message || 'An error occurred';
        }
      },
      formatCurrency(value) {
        return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR' }).format(value);
      },
    },
    created() {
      this.fetchAccounts();
    },
  };
</script>

<style scoped>
.mt-4 {
  margin-top: 16px;
}
</style>
