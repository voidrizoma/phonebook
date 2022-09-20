<template>
  <div id="contact-table">
    <p
      v-if="contacts.length < 1"
      class="empty-table"
    >
      No contacts
    </p>
    <table v-else>
      <thead>
        <tr>
          <th>Name</th>
          <th>Lastname</th>
          <th>Phone</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr
          :key="contact.contact_id"
          v-for="contact in contacts"
        >
          <td v-if="editing === contact.contact_id">
            <input
              type="text"
              v-model="contact.name"
            >
          </td>
          <td v-else>{{contact.name}}</td>
          <td v-if="editing === contact.contact_id">
            <input
              type="text"
              v-model="contact.last_name"
            >
          </td>
          <td v-else>{{contact.last_name}}</td>
                    <td v-if="editing === contact.contact_id">
            <input
              type="text"
              v-model="contact.phone_number"
            >
          </td>
          <td v-else>{{contact.phone_number}}</td>
          <td v-if="editing === contact.contact_id">
            <button @click="editContact(contact)">Save</button>
            <button
              class="muted-button"
              @click="editing = null"
            >Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(contact.contact_id)">EDIT</button>
            <button @click="$emit('delete:contact', contact.contact_id)">DELETE</button>
          </td>

        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'contact-table',
  props: {
    contacts: Array,
  },
  data() {
    return {
      editing: null,
    }
  },
  methods: {
    editMode(id) {
      this.editing = id
    },

    editContact(contact) {
      if (contact.name === '' || contact.last_name === '') return
      this.$emit('edit:contact', contact.contact_id, contact)
      this.editing = null
    }
  }
}
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
table{
  margin-top:30px;
  width:100%;
  font-size:18px;
}
thead {
    color: var(--brand-blue);
    font-size: 18px;
    font-weight: bolder;
    text-transform: uppercase;
}
td{
  text-align:center;
}

input {
  margin: 0;
}

.empty-table {
  text-align: center;
}
</style>
