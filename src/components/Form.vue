<template>
  <v-form
    class="form"
    ref="form"
    v-model="formValid"
    lazy-validation
  >
    <!-- name -->
    <v-text-field
      v-model="name"
      :rules="rules.name"
      label="Name"
      required
    />

    <!-- last_name -->
    <v-text-field
      v-model="last_name"
      :rules="rules.lastName"
      label="Last Name"
      required
    />

    <!-- phone_number -->
    <v-text-field
      v-model="phone_number"
      :rules="rules.phoneNumber"
      label="Phone Number"
      required
    />

    <!-- email -->
    <v-text-field
      v-model="email"
      :rules="rules.email"
      label="Email"
      required
    />

    <!-- country -->
    <v-text-field
      v-model="country"
      :rules="rules.country"
      label="Country"
      required
    />

    <!-- city -->
    <v-text-field
      v-model="city"
      :rules="rules.city"
      label="City"
      required
    />

    <!-- address -->
    <v-text-field
      v-model="address"
      :rules="rules.address"
      label="Address"
      required
    />

    <!-- buttons -->
    <v-btn
      color="green white--text"
      :disabled="!formValid"
      @click="addContact"
    >
      Add new item
    </v-btn>

    <v-btn 
      dark
      color="blue"
      @click="modifyContact"
    >
      Modify item
    </v-btn>
  </v-form>
</template>

<script>
export default {
  name: 'Form',
  data: () => ({
    formValid: true,
    name: '',
    last_name: '',
    phone_number: '',
    email: '',
    country: '',
    city: '',
    address: '',
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
        v => /\(?([0-9]+)\)?([ .-]?)([0-9]{3})\2([0-9]+)/.test(v) || 'Phone Number must be valid',
        v => (v && v.length <= 16) || 'Phone Number must be max 16 characters length'
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
  methods: {
    validate () {
      this.$refs.form.validate()
    },
    addContact() {
      this.validate();

      setTimeout(() => {
        console.log(this.formValid);
      }, 500);
    },
    modifyContact() {
      console.log('modify');
    }
  }
};
</script>

<style scoped>
</style>