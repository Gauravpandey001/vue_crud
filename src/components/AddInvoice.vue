<template>
  <div class="modal">
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>
      <h2>Add New Invoice</h2>
      <form @submit.prevent="addInvoice">
        <label for="status">Status:</label>
        <select id="status" v-model="newInvoice.status" required>
          <option value="Pending">Pending</option>
          <option value="Paid">Paid</option>
          <option value="Overdue">Overdue</option>
        </select>
        <label for="date">Date:</label>
        <input type="date" id="date" v-model="newInvoice.date" required />
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="newInvoice.name" required />
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="newInvoice.email" required />
        <label for="total">Total:</label>
        <input type="number" id="total" v-model="newInvoice.total" required />
        <label for="issuedDate">Issued Date:</label>
        <input type="date" id="issuedDate" v-model="newInvoice.issuedDate" required />
        <label for="balance">Balance:</label>
        <input type="number" id="balance" v-model="newInvoice.balance" required />
        <label for="id">ID:</label>
        <input type="text" id="id" v-model="newInvoice.id" required />
        <button type="submit">Add Invoice</button>
      </form>
    </div>
  </div>
</template>

<script>
import { API_ENDPOINT } from './apiConfig.js';

export default {
  data() {
    return {
      newInvoice: {
        status: 'Pending',
        date: '',
        name: '',
        email: '',
        total: '',
        issuedDate: '',
        balance: '',
        id: ''
      }
    };
  },
  methods: {
    addInvoice() {
      console.log('Adding new invoice:', this.newInvoice);
      fetch(`${API_ENDPOINT}/customers`, { 
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.newInvoice)
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Failed to add invoice');
        }
        this.closeModal();
        this.$emit('invoiceAdded');
      })
      .catch(error => {
        console.error('Error adding invoice:', error);
      });
    },
    closeModal() {
      this.$emit('close');
    }
  }
};
</script>

<style scoped>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  padding: 20px;
  border-radius: 8px;
}

.close {
  color: #aaa;
  float: right;
  font-size: 24px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
