<template>
  <b-container fluid>
    <b-table hover :items="displayedCustomers" :fields="fields">
      <template v-slot:cell(action)="data">
        <img src="@/assets/delete.png" alt="Delete Icon" @click="deleteCustomer(data.item.id)" class="delete-icon" />
      </template>
      <template v-slot:cell(check)="data">
        <b-form-checkbox v-model="data.item.selected" />
      </template>
      <template v-slot:cell(status)="data">
        <img :src="getStatusIcon(data.item.status)" alt="Status Icon" class="status-icon" />
      </template>
      <template v-slot:cell(id)="data">
        <span style="color: purple;">{{ data.item.id }}</span>
      </template>
      <template v-slot:cell(name)="data">
        <div class="user-image-container">
          <img :src="data.item.user_image" alt="User Image" class="user-image" />
          <div>{{ data.item.name }}</div>
        </div>
        <div class="user-email">{{ data.item.email }}</div>
      </template>
      <template v-slot:cell(balance)="data">
        <img v-if="data.item.balance == 0" src="@/assets/paid.png" alt="Paid Icon" class="paid-icon" />
        <span v-else>{{ data.item.balance }}</span>
      </template>
    </b-table>
    <b-pagination v-model="currentPage" :total-rows="customers.length" :per-page="pageSize" aria-controls="customer-table"></b-pagination>
  </b-container>
</template>

<script>
import { BContainer, BTable, BPagination, BFormCheckbox } from 'bootstrap-vue';
import { API_ENDPOINT } from './apiConfig.js';
export default {
  components: {
    BContainer,
    BTable,
    BPagination,
    BFormCheckbox
  },
  props: {
    customers: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      currentPage: 1,
      pageSize: 10,
      fields: [
        { key: 'check', label: 'Select' },
        { key: 'id'},
        { key: 'status', label: 'Status' },
        { key: 'name', label: 'Client'},
        { key: 'total',},
        { key: 'issued_date'},
        { key: 'balance'},
        { key: 'action', label: 'Action' }, 
      ]
    };
  },
  computed: {
    displayedCustomers() {
      const startIndex = (this.currentPage - 1) * this.pageSize;
      const endIndex = startIndex + this.pageSize;
      return this.customers.slice(startIndex, endIndex);
    }
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
      })
      .catch(error => {
        console.error('Error deleting customer:', error);
      });
    },
    getStatusIcon(status) {
      const iconMap = {
        paid: require('@/assets/delivered.png'),
        pending: require('@/assets/pending.png'),
        overdue: require('@/assets/cancelled.png')
      };
      return iconMap[status.toLowerCase()] || '';
    }
  }
};
</script>

<style scoped>

.b-table th,
.b-table td {
  padding: 10px; 
}
.user-image-container {
  display: flex;
  align-items: center;
}
.user-image {
  width: 50px; 
  height: 50px; 
  border-radius: 50%;
  margin-right: 10px;
}
.user-email {
  font-size: 0.8rem; 
  color: #666; 
  margin-top: 5px;
}

.status-icon {
  width: 20px;
  height: auto;
}

.delete-icon {
  width: 20px;
  height: auto;
  cursor: pointer; 
}

.paid-icon {
  width: 40px;
  height: auto;
}
</style>
