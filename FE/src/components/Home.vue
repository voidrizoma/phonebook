<template>
  <div>
    <header>
      <h1><span>Phone</span>Book</h1>
    </header>
    <h2 v-on:click = "visible = !visible ">New Contact</h2>
    <contact-form v-if="visible" @add:contact="addContact" />
    <contact-table 
      :contacts="contacts"
      @delete:contact="deleteContact"
      @edit:contact="editContact"
    />
  </div>
</template>

<script>
import ContactTable from "@/components/ContatTable.vue";
import ContactForm from "@/components/ContactForm.vue";

export default {
  name: "app",
  components: {
    ContactTable,
    ContactForm,
  },
  data() {
    return {
      contacts: [],
      visible: false,
    };
  },

  mounted() {
    this.getcontacts();
  },

  methods: {
    async getcontacts() {
      try {
        const response = await fetch("http://localhost:3000/contacts");
        const data = await response.json();
        console.log(data)
        this.contacts = data;
      } catch (error) {
        console.error(error);
      }
    },

    async addContact(contact) {
      try {
        console.log(JSON.stringify(contact));
        const response = await fetch("http://localhost:3000/contacts", {
          method: "POST",
          body: JSON.stringify(contact),
          headers: { "Content-type": "application/json; charset=UTF-8" },
        });
        const data = await response.json();
        this.contacts = [...this.contacts, data];
        this.visible = false;
      } catch (error) {
        console.error(error);
      }
    },

    async editContact(id, updateContact) {
      try {
        const response = await fetch(`http://localhost:3000/contacts/${id}`, {
          method: "PUT",
          body: JSON.stringify(updateContact),
          headers: { "Content-type": "application/json; charset=UTF-8" },
        });
        const data = await response.json();
        this.contacts = this.contacts.map((contact) =>
          contact.id === id ? data : contact
        );
      } catch (error) {
        console.error(error);
      }
    },

    async deleteContact(id) {
      try {
        await fetch(`http://localhost:3000/contacts/${id}`, {
          method: "DELETE",
        });
        this.contacts = this.contacts.filter((contact) => contact.contact_id !== id);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style>
header {
  display: flex;
  justify-content: center;
  align-items: center;
}
img,
span {
  width: 48px;
}
h1 {
  font-size: 45px;
  color: var(--brand-blue);
}
h1 span {
  color: var(--brand-green);
}
h2{
  color: white;
    background: var(--brand-green);
    display: inline;
    padding: 7px;
    border-radius: 5px;
}
button {
  background: var(--brand-green);
  border: 1px solid #009435;
  padding: 12px;
  margin: 14px 0px;
  color: white;
  text-transform: uppercase;
  font-weight: bold;
   border-radius: 5px;
}

button:hover,
button:active,
button:focus {
  background: #32a95d;
  border: 1px solid #32a95d;
}

.small-container {
  max-width: 680px;
}
</style>
