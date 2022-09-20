<template>
  <div id="contact-form">
    <form @submit.prevent="handleSubmit">
      <label>Name</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="contact.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      >
      <label>Lastname</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidLastname }"
        v-model="contact.last_name"
        @focus="clearStatus"
      >
            <label>Phone Number</label>
      <input
        type="text"
        v-model="contact.phone_number"
        @focus="clearStatus"
      >
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Contact successfully added</p>
      <button>Add contact</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "contact-form",
  data() {
    return {
      error: false,
      submitting: false,
      success: false,
      contact: {
        name: "",
        last_name: "",
       phone_number: "",
      }
    };
  },
  computed: {
    invalidName() {
      return this.contact.name === "";
    },

    invalidLastname() {
      return this.contact.last_name === "";
    }
  },
  methods: {
    handleSubmit() {
      this.clearStatus();
      this.submitting = true;

      if (this.invalidName || this.invalidLastname ) {
        this.error = true;
        return;
      }

      this.$emit("add:contact", this.contact);
      this.$refs.first.focus();
      this.contact = {
        name: "",
        last_name: "",
        phone_number: "",
      };
      this.clearStatus();
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    }
  }
};
</script>

<style scoped>
form {
    display: flex;
    flex-direction: column;
    width: 100%;
    padding: 28px;
    font-size: 19px;
}
input{
    height: 45px;
    padding: 5px;
    font-size: 16px;
    margin: 10px 0px;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
