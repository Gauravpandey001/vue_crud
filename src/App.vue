<template>
  <div class="container">
    <b-card>
      <div class="button-container">
        <b-button @click="showAddInvoiceModal = true" class="custom-button">+ Create Invoice</b-button>
        <SearchUserButton />
      </div>
      <AddInvoice v-if="showAddInvoiceModal" @close="showAddInvoiceModal = false" @invoiceAdded= "fetchInvoices"/>
      <CustomerList :customers="customers" @customerDeleted="fetchInvoices" />
    </b-card>
  </div>
</template>

<script>
import { BCard } from 'bootstrap-vue';
import CustomerList from './components/CustomerList.vue';
import AddInvoice from './components/AddInvoice.vue';
import SearchUserButton from './components/SearchUserButton.vue';
import { API_ENDPOINT } from './components/apiConfig';
export default {
  components: {
    CustomerList,
    AddInvoice,
    BCard,
    SearchUserButton
  },
  data() {
    return {
      customers: [],
      showAddInvoiceModal: false
    };
  },
  methods: {
    fetchInvoices() {
      fetch(`${API_ENDPOINT}/customers`)
        .then(response => response.json())
        .then(data => {
          this.customers = data;
        })
        .catch(error => {
          console.error('Error fetching invoices:', error);
        });
    },
    deleteCustomer(index) {
      this.customers.splice(index, 1);
    }
  },
  mounted() {
    this.fetchInvoices();
  }
};
</script>

<style scoped>
.container {
  margin-top: 20px;
  height: auto; 
  display: flex;
  justify-content: center;
  align-items: flex-start;
}
.custom-button {
  background-color: purple;
  border-color: purple;
  margin-right: 10px;
}
.custom-button:hover {
  background-color: darkpurple;
  border-color: darkpurple;
}
.button-container {
  margin-bottom: 20px;
}
</style>
./components/apiConfig