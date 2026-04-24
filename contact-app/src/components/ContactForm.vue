<template>
  <form @submit.prevent="submitForm" class="form">
    <h2>
      {{ contact.id ? "Modifier" : "Ajouter" }} un contact
    </h2>

    <!-- INPUT NOM -->
    <div class="input-group">
      <label>Nom</label>
      <input v-model="contact.name" placeholder="Entrer le nom" />
    </div>

    <!-- INPUT EMAIL -->
    <div class="input-group">
      <label>Email</label>
      <input v-model="contact.email" placeholder="Entrer l'email" />
    </div>

    <!-- INPUT PHONE -->
    <div class="input-group">
      <label>Téléphone</label>
      <input v-model="contact.phone" placeholder="Entrer le téléphone" />
    </div>

    <!-- ERREUR -->
    <p v-if="error" class="error">{{ error }}</p>


    <!-- BUTTONS -->
    <div class="actions">
      <button type="submit" :disabled="loading" class="save-btn">
      <i class="fa-solid fa-floppy-disk"></i>
      {{ loading ? "Envoi..." : "Enregistrer" }}
      </button>

      <button type="button" @click="$emit('cancel')" class="cancel-btn">
        Annuler
      </button>
    </div>
  </form>
</template>

<script>
export default {
  props: ["contactToEdit", "loading"],

  data() {
    return {
      contact: {
        id: null,
        name: "",
        email: "",
        phone: "",
      },
      error: ""
    };
  },

  watch: {
    contactToEdit: {
      immediate: true,
      handler(val) {
        if (val) {
          this.contact = { ...val };
        } else {
          this.reset();
        }
      },
    },
  },

  methods: {
    submitForm() {
      if (!this.contact.name || !this.contact.email || !this.contact.phone) {
        alert("Tous les champs sont obligatoires");
        return;
      }

      // validation email
      const emailRegex = /\S+@\S+\.\S+/
      if (!emailRegex.test(this.contact.email)) {
        this.error = "Email invalide"
        return
      }

      this.error = ""

      this.$emit("save", this.contact);
    },

    reset() {
      this.contact = {
        id: null,
        name: "",
        email: "",
        phone: "",
      };
      this.error = ""
    },
  },
};
</script>


<style>
.form {
  background: white;
  padding: 25px;
  border-radius: 15px;
  max-width: 400px;
}

h2 {
  margin-bottom: 20px;
}

.input-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
}

label {
  font-size: 14px;
  margin-bottom: 5px;
}

input {
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

.actions {
  display: flex;
  gap: 10px;
  margin-top: 15px;
}

.save-btn {
  background: linear-gradient(45deg, #4f6ef7, #7c5df9);
  color: white;
  border: none;
  padding: 10px;
  border-radius: 8px;
  cursor: pointer;
}

.cancel-btn {
  background: #eee;
  border: none;
  padding: 10px;
  border-radius: 8px;
}

.error {
  color: red;
  font-size: 14px;
}
</style>

