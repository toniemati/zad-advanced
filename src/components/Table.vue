<template>
  <v-card class="pa-6 elevation-5">
    <v-text-field
      v-model="search"
      label="Search"
      class="mx-4"
    />

    <v-data-table
      :headers="headers"
      :items="contacts"
      :search="search"
    >
      <template v-slot:top>
        <v-dialog v-model="dialogDelete" max-width="fit-content">
          <v-card class="pa-6">
            <v-card-title v-if="toDelete" class="text-h5 justify-center text-center">
              Are you sure you want to delete {{ toDelete.name }} {{ toDelete.last_name }}?
            </v-card-title>
            <v-card-actions class="justify-center">
              <v-btn rounded color="blue darken-1 white--text" @click="closeDelete">No</v-btn>
              <v-btn rounded color="red darken-1 white--text" @click="deleteContactConfirm">Yes</v-btn>
            </v-card-actions>
          </v-card> 
        </v-dialog>
      </template>

      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" color="yellow darken-2" @click="editContact(item)">
          mdi-pencil
        </v-icon>
        <v-icon small color="red darken-3" @click="deleteContact(item)">
          mdi-delete
        </v-icon>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  name: 'Table',
  props: ['contacts'],
  data: () => ({
    search: '',
    dialogDelete: false,
    toDelete: null,
    headers: [
      { text: 'Name', value: 'name' },
      { text: 'Last Name', value: 'last_name' },
      { text: 'Phone Number', value: 'phone_number' },
      { text: 'Email', value: 'email' },
      { text: 'Country', value: 'country' },
      { text: 'City', value: 'city' },
      { text: 'Address', value: 'address' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
  }),
  methods: {
    closeDelete() {
      this.dialogDelete = false;
      this.toDelete = null;
    },
    editContact(contact) {
      this.$emit('contactChange', contact);
    },
    deleteContact(contact) {
      this.toDelete = contact;
      this.dialogDelete = true;
    },
    deleteContactConfirm() {
      this.$emit('deleteContact', this.toDelete);
      this.closeDelete();
    }
  }
}
</script>
