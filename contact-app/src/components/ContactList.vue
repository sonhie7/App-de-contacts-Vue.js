<template>
  <p v-if="loading">Chargement...</p>
  <div class="grid">
    <ContactItem
      v-for="contact in contacts"
      :key="contact.id"
      :contact="contact"
      @delete="$emit('delete', contact.id)"
      @edit="$emit('edit', contact)"
    />
  </div>
</template>

<script>
import ContactItem from "./ContactItem.vue";

export default {
  props: ["contacts"],
  components: { ContactItem },
};
</script>

<style>
.grid {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  margin-top: 30px;
}
</style>


