<template>
  <v-container>
    <v-card>
      <v-card-title>Account List</v-card-title>
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
      <v-card-title>Summary</v-card-title>
      <v-card-text>
        <p><strong>Total Accounts:</strong> {{ totalAccounts }}</p>
        <p><strong>Total Balance:</strong> Rp {{ totalBalance }}</p>
        <p><strong>Average Balance:</strong> Rp {{ averageBalance.toFixed(2) }}</p>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import api from '@/plugins/api';

export default {
  data() {
    return {
      accounts: [],
      totalAccounts: 0,
      totalBalance: 0,
      averageBalance: 0
    };
  },
  created() {
    this.fetchAccounts();
  },
  methods: {
    fetchAccounts() {
      api.get('/account/list')
        .then(response => {
          this.accounts = response.data;
          this.calculateStatistics(response.data);
        })
        .catch(error => {
          console.error("Error fetching account list:", error);
        });
    },
    calculateStatistics(data) {
      this.totalAccounts = data.length;
      this.totalBalance = data.reduce((sum, account) => sum + account.balance, 0);
      this.averageBalance = this.totalAccounts ? this.totalBalance / this.totalAccounts : 0;
    }
  }
};
</script>

<style scoped>
.mt-4 {
  margin-top: 16px;
}
</style>
