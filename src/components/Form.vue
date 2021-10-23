<template>
  <v-card class="px-4 py-4 elevation-5">
    <v-form
      class="form"
      ref="form"
      v-model="formValid"
      lazy-validation
    >
      <!-- name -->
      <v-text-field
        v-model="contact.name"
        :rules="rules.name"
        label="Name"
        required
      />

      <!-- last_name -->
      <v-text-field
        v-model="contact.last_name"
        :rules="rules.lastName"
        label="Last Name"
        required
      />

      <!-- phone_number -->
      <v-text-field
        v-model="contact.phone_number"
        :rules="rules.phoneNumber"
        label="Phone Number"
        required
      />

      <!-- email -->
      <v-text-field
        v-model="contact.email"
        :rules="rules.email"
        label="Email"
        required
      />

      <!-- country -->
      <v-text-field
        v-model="contact.country"
        :rules="rules.country"
        label="Country"
        required
      />

      <!-- city -->
      <v-text-field
        v-model="contact.city"
        :rules="rules.city"
        label="City"
        required
      />

      <!-- address -->
      <v-text-field
        v-model="contact.address"
        :rules="rules.address"
        label="Address"
        required
      />

      <!-- buttons -->
      <div class="text-center">
        <v-btn-toggle
          shaped
        >
          <v-btn
            class="green white--text font-weight-bold"
            :disabled="addButton"
            @click="addContact"
          >
            Add
          </v-btn>

          <v-btn
            class="orange white--text font-weight-bold"
            @click="clear"
          >
            Clear
          </v-btn>

          <v-btn
            class="blue white--text font-weight-bold"
            :disabled="!contact.id"
            @click="modifyContact"
          >
            Modify
          </v-btn>
        </v-btn-toggle>
      </div>

      <!-- <v-btn
        class="green white--text"
        :disabled="addButton"
        @click="addContact"
      >
        Add new item
      </v-btn>

      <v-btn
        class="yellow white--text"
        @click="clear"
      >
        Clear Form
      </v-btn>

      <v-btn 
        class="blue white--text"
        :disabled="!contact.id"
        @click="modifyContact"
      >
        Modify item
      </v-btn> -->
    </v-form>

    <v-dialog v-model="dialogModify" max-width="600">
      <v-card>
        <v-card-title v-if="contact.id" class="text-h5 justify-center text-center">
          Are you sure you want to modify {{ contact.name }} {{ contact.last_name }}?
        </v-card-title>
        <v-card-actions class="justify-center">
          <v-btn color="red darken-1" text @click="closeModify">Cancel</v-btn>
          <v-btn color="green" text @click="modifyContactConfirm">Yes</v-btn>
        </v-card-actions>
      </v-card> 
    </v-dialog>
  </v-card>
</template>

<script>
export default {
  name: 'Form',
  data: () => ({
    dialogModify: false,
    formValid: false,
    contact: {
      id: null,
      name: '',
      last_name: '',
      phone_number: '',
      email: '',
      country: '',
      city: '',
      address: '',
    },
    rules: {
      name: [
        v => !!v || 'Name is required',
        v => /^\w+$/u.test(v) || 'Name must be valid',
      ],
      lastName: [
        v => !!v || 'Last Name is required',
        v => /^\w+$/u.test(v) || 'Name must be valid',
      ],
      phoneNumber: [
        v => !!v || 'Phone Number is required',
        v => /(\d|\(|\)|\+|\s|\-|\.)+/.test(v) || 'Phone Number must be valid',
        v => (v && v.length <= 17) || 'Phone Number must be max 17 characters length'
      ],
      email: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/u.test(v) || 'E-mail must be valid',
      ],
      country: [
        v => !!v || 'Country is required',
      ],
      city: [
        v => !!v || 'City is required',
      ],
      address: [
        v => !!v || 'Address is required',
      ],
    }
  }),
  computed: {
    addButton: function() {
      return (!this.formValid || this.contact.id) ? true : false;
    }
  },
  props: ['currentContact'],
  watch: {
    currentContact: function(val) {
      this.contact = val;
    },
  },
  methods: {
    clear() {
      this.$refs.form.reset();
      this.contact = {};
    },
    validate () {
      this.$refs.form.validate()
    },
    addContact() {
      this.validate();

      setTimeout(() => {
        if (!this.formValid) return;

        this.$emit('addContact', this.contact);
      }, 100);
    },
    closeModify() {
      this.dialogModify = false;
    },
    modifyContact() {
      if (!this.contact.id) return;

      this.dialogModify = true;
    },
    modifyContactConfirm() {
      if (!this.contact.id) return;

      this.$emit('modifyContact', this.contact);
      this.closeModify();
    }
  }
};
</script>

<style scoped>
</style>