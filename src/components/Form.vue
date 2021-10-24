<template>
  <v-card class="pa-6 elevation-5" @keypress.13="handleSubmit">
    <v-form
      ref="form"
      v-model="formValid"
      lazy-validation
    >
      <v-text-field
        v-model="contact.name"
        :rules="rules.name"
        label="Name"
        required
      />

      <v-text-field
        v-model="contact.last_name"
        :rules="rules.lastName"
        label="Last Name"
        required
      />

      <v-text-field
        v-model="contact.phone_number"
        :rules="rules.phoneNumber"
        label="Phone Number"
        required
      />

      <v-text-field
        v-model="contact.email"
        :rules="rules.email"
        label="Email"
        required
      />

      <v-text-field
        v-model="contact.country"
        :rules="rules.country"
        label="Country"
        required
      />

      <v-text-field
        v-model="contact.city"
        :rules="rules.city"
        label="City"
        required
      />

      <v-text-field
        v-model="contact.address"
        :rules="rules.address"
        label="Address"
        required
      />

      <div class="text-center">
        <v-btn-toggle
          shaped
        >
          <v-btn
            class="green white--text font-weight-bold"
            :disabled="disabledAddButton"
            @click="addContact"
          >
            Add
          </v-btn>

          <v-btn
            class="orange white--text font-weight-bold"
            @click="reset"
          >
            Clear
          </v-btn>

          <v-btn
            class="blue white--text font-weight-bold"
            :disabled="disabledModifyButton"
            @click="modifyContact"
          >
            Modify
          </v-btn>
        </v-btn-toggle>
      </div>
    </v-form>

    <v-dialog v-model="dialogModify" max-width="fit-content">
      <v-card class="pa-6">
        <v-card-title v-if="contact.id" class="text-h5 justify-center text-center">
          Are you sure you want to modify {{ contact.name }} {{ contact.last_name }}?
        </v-card-title>
        <v-card-actions class="justify-center">
          <v-btn rounded color="blue darken-1 white--text" @click="closeModify">No</v-btn>
          <v-btn rounded color="green darken-1 white--text" @click="modifyContactConfirm">Yes</v-btn>
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
    disabledAddButton: function() {
      return (!this.formValid || this.contact.id) ? true : false;
    },
    disabledModifyButton: function() {
      return (!this.contact.id || !this.formValid) ? true : false;
    }
  },
  props: ['currentContact'],
  watch: {
    currentContact: function(val) {
      this.contact = val;

      if (!val.name) this.reset();
    },
  },
  methods: {
    reset() {
      this.$refs.form.reset();
    },
    validate () {
      this.$refs.form.validate()
    },
    handleSubmit() {
      if (this.contact.id) this.modifyContact();
      else this.addContact();
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
