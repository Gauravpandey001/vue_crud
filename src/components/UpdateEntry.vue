<template>
    <div>
        <b-icon icon="three-dots-vertical" @click="showModal = true" class="three-dots"></b-icon>
      <b-modal v-model="showModal" title="Modify Entry">
        <b-form-group label="Name">
          <b-form-input v-model="editedCustomer.name"></b-form-input>
        </b-form-group>
        <b-form-group label="Email">
          <b-form-input v-model="editedCustomer.email"></b-form-input>
        </b-form-group>
        <b-form-group label="Total">
          <b-form-input type="number" v-model="editedCustomer.total"></b-form-input>
        </b-form-group>
        <b-form-group label="Issued Date">
          <b-form-input type="date" v-model="editedCustomer.issued_date"></b-form-input>
        </b-form-group>
        <b-form-group label="Balance">
          <b-form-input type="number" v-model="editedCustomer.balance"></b-form-input>
        </b-form-group>
        <b-form-group label="Status">
          <b-form-select v-model="editedCustomer.status" :options="statusOptions"></b-form-select>
        </b-form-group>
        <b-button @click="saveChanges" class="save">Save Changes</b-button>
      </b-modal>
    </div>
  </template>
  
  <script>
  import { BModal, BFormInput, BButton, BFormGroup, BFormSelect } from 'bootstrap-vue';
  
  export default {
    components: {
      BModal,
      BFormInput,
      BButton,
      BFormGroup,
      BFormSelect
    },
    props: {
      customer: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        showModal: false,
        editedCustomer: { ...this.customer },
        statusOptions: ['Pending', 'Paid', 'Overdue']
      };
    },
    methods: {
      saveChanges() {
        this.$emit('save', this.editedCustomer);
        this.showModal = false;
      }
    }
  };
  </script>
  <style>
.three-dots {
    cursor: pointer;
}
.save {
    margin-top: 25px;
}
</style>
  