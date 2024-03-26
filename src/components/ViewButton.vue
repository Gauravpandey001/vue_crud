<template>
  <div>
    <b-icon-eye @click="openModal" class="eye-icon"></b-icon-eye>
    <b-modal v-model="modalVisible" title="User Details">
      <div v-if="customer">
        <p>Id: #{{ customer.id }}</p>
        <p>Name: {{ customer.name }}</p>
        <p>Email: {{ customer.email }}</p>
        <p>Status: {{ customer.status }}</p>
        <p>Total: ${{ customer.total }}</p>
        <p>Issued Date: {{ customer.issued_date }}</p>
        <p>Balance: {{ customer.balance }}</p>
        <img src="@/assets/delete.png" alt="Delete Icon" @click="deleteCustomer(customer.id)" class="delete-icon" />
      </div>
    </b-modal>
  </div>
</template>

<script>
import { BIconEye, BModal } from 'bootstrap-vue';
import { API_ENDPOINT } from './apiConfig.js';

export default {
  components: {
    BIconEye,
    BModal
  },
  props: {
    customer: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      modalVisible: false
    };
  },
  methods: {
    deleteCustomer(customerId) {
      fetch(`${API_ENDPOINT}/customers/${customerId}`, {
        method: 'DELETE'
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Failed to delete customer');
        }
        this.$emit('customerDeleted');
        console.log('deleted')
        this.modalVisible = false;
      })
      .catch(error => {
        console.error('Error deleting customer:', error);
      });
    },
    openModal() {
      this.modalVisible = true;
    }
    
  }
};
</script>

<style scoped>
.eye-icon {
  cursor: pointer;
  margin-right: 10px;
}
.delete-icon {
  width: 20px;
  height: auto;
  cursor: pointer; 
}
</style>
