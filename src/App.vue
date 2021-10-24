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
        href="#"
        text
      >
        <span class="mr-2">Guest</span>
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

    <v-dialog v-model="dialogMessage" max-width="fit-content">
      <v-card :color="message.color" dark>
        <v-card-title class="justify-space-between">
          <div class="d-flex align-self-center">
            <v-icon left>{{ message.icon }}</v-icon>
            <span>{{ message.content }}</span>
          </div>

          <v-btn 
            @click="dialogMessage = false" 
            dark 
            text
            class="ml-3"
          >
            Ok
          </v-btn>
        </v-card-title>
      </v-card>
    </v-dialog>
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
    dialogMessage: false,
    message: {
      icon: 'mdi-delete',
      color: 'orange darken-4',
      content: 'There was some problems with adding contact, try again later.'
    },
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
      
      window.scroll({
        top: 0,
        left: 0,
        behavior: 'smooth'
      });
    },
    async addContact(contact) {
      const { data: { message }, status } = await axios.post('http://test01.varid.pl:4080/api/contact', contact);

      if (status === 200) {
        this.message = {
          icon: 'mdi-plus-thick',
          color: 'green',
          content: message
        }
      } else {
        this.message = {
          icon: 'mdi-alert',
          color: 'red',
          content: 'There was some problems with adding contact, try again later.'
        }
      }
      this.dialogMessage = true;
      this.fetchContacts();
      this.currentContact = {};
    },
    async modifyContact(contact) {
      const { data: { message }, status } = await axios.put(`http://test01.varid.pl:4080/api/contact/${contact.id}`, contact);

      if (status === 200) {
        this.message = {
          icon: 'mdi-pencil',
          color: 'orange',
          content: message
        }
      } else {
        this.message = {
          icon: 'mdi-alert',
          color: 'red',
          content: 'There was some problems with updating contact, try again later.'
        }
      }
      this.dialogMessage = true;
      this.fetchContacts();
      this.currentContact = {};
    },
    async deleteContact(contact) {
      const { data: { message }, status } = await axios.delete(`http://test01.varid.pl:4080/api/contact/delete/${contact.id}`);

      if (status === 200) {
        this.message = {
          icon: 'mdi-delete',
          color: 'orange darken-4',
          content: message
        }
      } else {
        this.message = {
          icon: 'mdi-alert',
          color: 'red',
          content: 'There was some problems with updating contact, try again later.'
        }
      }
      this.dialogMessage = true;
      this.fetchContacts();
      this.currentContact = {};
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
