<template>
    <div class="d-flex justify-content-end">
      <b-button @click="showSearchModal = true">Search User</b-button>
      <b-modal v-model="showSearchModal" title="Search User">
        <form @submit.prevent="searchUser">
          <label for="searchTerm">Search Term:</label>
          <input type="text" id="searchTerm" v-model="searchTerm" required />
          <b-button type="submit">Search</b-button>
        </form>
      </b-modal>
    </div>
  </template>
  
  <script>
  import { BButton, BModal } from 'bootstrap-vue';
  
  export default {
    components: {
      BButton,
      BModal
    },
    data() {
      return {
        showSearchModal: false,
        searchTerm: ''
      };
    },
    methods: {
      searchUser() {
        this.$emit('search', this.searchTerm);
        this.showSearchModal = false;
      }
    }
  };
  </script>
  