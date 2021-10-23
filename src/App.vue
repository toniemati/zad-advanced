<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <Form :currentContact="currentContact" @addContact="addContact" @modifyContact="modifyContact" />
      <Table :contacts="contacts" @contactChange="contactChange" @deleteContact="deleteContact" />
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';
import Form from './components/Form.vue';
import Table from './components/Table.vue';

export default {
  name: 'App',
  components: { Form, Table },
  data: () => ({
    contacts: [],
    currentContact: null,
  }),
  created: function() {
    this.fetchContacts();
  },
  methods: {
    async fetchContacts() {
      const { data } = await axios.get('http://test01.varid.pl:4080/api/contacts');
      this.contacts = data;
    },
    contactChange(contact) {
      this.currentContact = contact;
      window.scrollTo(0, 0);
    },
    addContact(contact) {
      //todo post request
      console.log('adding from app', contact);
    },
    modifyContact(contact) {
      //todo put request
      console.log('editing from app', contact);
    },
    deleteContact(contact) {
      //todo delete request
      console.log('delete from app', contact);
    }
  }
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
