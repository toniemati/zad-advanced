<template>
  <v-app>
    <v-app-bar
      app
      color="orange"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Crud logo"
          class="shrink mr-2"
          contain
          src="https://upload.wikimedia.org/wikipedia/commons/d/d5/Contacts_%28iOS%29.png"
          transition="scale-transition"
          width="50"
        />

        <div class="ml-2 font-weight-bold headline ">
          My contacts
        </div>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/toniemati"
        target="_blank"
        text
      >
        <span class="mr-2">toniemati</span>
        <v-icon>mdi-account</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-row class="mx-2 my-2">
        <v-col cols="12" lg="3">
          <Form :currentContact="currentContact" @addContact="addContact" @modifyContact="modifyContact" />
        </v-col>

        <v-col cols="12" lg="9">
          <Table :contacts="contacts" @contactChange="contactChange" @deleteContact="deleteContact" />
        </v-col>
      </v-row>
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
      this.currentContact = { ...contact };
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

<style>
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@400;700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Lato', sans-serif;
}
</style>
