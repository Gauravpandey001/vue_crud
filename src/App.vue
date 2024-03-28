<template>
  <div class="container">
    <loader-component :isLoading="loading"></loader-component>
    <b-card>
      <div class="button-container">
        <b-button @click="showAddInvoiceModal = true" class="custom-button">+ Create Invoice</b-button>
        <SearchUserButton />
      </div>
      <AddInvoice v-if="showAddInvoiceModal" @close="showAddInvoiceModal = false" @invoiceAdded="fetchInvoices"/>
      <CustomerList :customers="customers" @refreshList="fetchInvoices" @updateCustomer="updateCustomer" />
    </b-card>
  </div>
</template>

<script>
import { BCard } from 'bootstrap-vue';
import CustomerList from './components/CustomerList.vue';
import AddInvoice from './components/AddInvoice.vue';
import SearchUserButton from './components/SearchUserButton.vue';
import LoaderComponent from './components/LoaderComponent.vue'; 
import { API_ENDPOINT } from './components/apiConfig';
export default {
  components: {
    CustomerList,
    AddInvoice,
    BCard,
    SearchUserButton,
    LoaderComponent 
  },
  data() {
    return {
      customers: [],
      showAddInvoiceModal: false,
      loading: false 
    };
  },
  methods: {
    fetchInvoices() {
      this.loading = true; 
      fetch(`${API_ENDPOINT}/customers`)
        .then(response => response.json())
        .then(data => {
          this.customers = data;
        })
        .catch(error => {
          console.error('Error fetching invoices:', error);
        })
        .finally(() => {
          this.loading = false;
        });
    },
    updateCustomer({ index, updatedCustomer }) {
      // Update the customers prop with the modified customer
      this.$set(this.customers, index, updatedCustomer);
      this.loading = true;
      // Make PUT request to update customer data on API backend
      fetch(`${API_ENDPOINT}/customers/${updatedCustomer.id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(updatedCustomer),
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Failed to update customer data');
        }
      })
      .catch(error => {
        console.error('Error updating customer data:', error);
      })
      .finally(() => {
          this.loading = false;
        });
    },
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
