<template>
  <div class="container">
    <!-- HEADER -->
    <div class="header">
      <div>
        <!-- <span class="badge">{{ contacts.length }} contacts</span> -->
        <h1>Mes Contacts</h1>
        <p>Gérez vos contacts facilement.</p>
      </div>

      <button class="add-btn" @click="goToAdd">
        + Nouveau contact
      </button>
    </div>

    <!-- HOME -->
    <div v-if="page === 'home'"> 

      <p v-if="loading">Chargement...</p>
      <p v-if="error">{{ error }}</p>

      <!-- EMPTY -->
      <div v-if="!loading && contacts.length === 0" class="empty">
        <div class="icon">👤</div>
        <h3>Aucun contact</h3>
        <p>Commencez par ajouter votre premier contact</p>

        <button class="add-btn" @click="goToAdd">
          + Ajouter un contact
        </button>
      </div>

      <!-- LIST -->
      <ContactList
        v-if="!loading && contacts.length"
        :contacts="contacts"
        @delete="deleteContact"
        @edit="editContact"
      />
    </div>

    <!-- FORM -->
    <div v-if="page === 'form'" class="form-box">
      <ContactForm
        :contactToEdit="contactToEdit"
        :loading="formLoading"
        @save="handleSave"
        @cancel="goHome"
      />

      <p v-if="successMessage" class="success">
        {{ successMessage }}
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ContactList from "./components/ContactList.vue";
import ContactForm from "./components/ContactForm.vue";

export default {
  components: { ContactList, ContactForm },

  data() {
    return {
      contacts: [],
      loading: false,
      error: null,

      page: "home",

      formLoading: false,
      successMessage: "",

      contactToEdit: null,
    };
  },

  methods: {
    async fetchContacts() {
      this.loading = true;
      this.error = null;

      try {
        const res = await axios.get("http://localhost:3000/contacts");
        this.contacts = res.data;
      } catch (err) {
        this.error = "Erreur lors du chargement";
      } finally {
        this.loading = false;
      }
    },

    async handleSave(contact) {
      this.formLoading = true;

      try {
        if (contact.id) {
          // UPDATE
          await axios.put(
            `http://localhost:3000/contacts/${contact.id}`,
            contact
          );
          this.successMessage = "Contact modifié !";
        } else {
          // CREATE
          await axios.post("http://localhost:3000/contacts", contact);
          this.successMessage = "Contact ajouté !";
        }

        await this.fetchContacts();

        setTimeout(() => {
          this.goHome();
          this.successMessage = "";
        }, 1500);

      } catch {
        alert("Erreur");
      } finally {
        this.formLoading = false;
      }
    },

    editContact(contact) {
      this.contactToEdit = { ...contact };
      this.page = "form";
    },

    async deleteContact(id) {
      if (confirm("Supprimer ?")) {
        await axios.delete(`http://localhost:3000/contacts/${id}`);
        this.fetchContacts();
      }
    },

    goToAdd() {
      this.contactToEdit = null;
      this.page = "form";
    },

    goHome() {
      this.page = "home";
      this.contactToEdit = null;
    },
  },

  mounted() {
    this.fetchContacts();
  },
};
</script>


<style>
body {
  background: #f5f7fb;
  font-family: Arial;
}

.container {
  padding: 40px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.badge {
  background: #eee;
  padding: 5px 10px;
  border-radius: 20px;
  font-size: 12px;
}

.add-btn {
  background: linear-gradient(45deg, #4f6ef7, #7c5df9);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 10px;
  cursor: pointer;
}

.empty {
  text-align: center;
  background: white;
  padding: 50px;
  border-radius: 15px;
  margin-top: 30px;
}

.icon {
  font-size: 40px;
  margin-bottom: 10px;
}

.form-box {
  background: white;
  padding: 20px;
  border-radius: 15px;
  margin-top: 20px;
}

.success {
  color: green;
  margin-top: 10px;
}
</style>


