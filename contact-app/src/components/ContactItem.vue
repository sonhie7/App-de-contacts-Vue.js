<template>
  <div class="card">
    <div class="top">
      <div class="avatar">
        {{ contact.name.charAt(0).toUpperCase() }}
      </div>

      <div>
        <h3>{{ contact.name }}</h3>
        <p><i class="fa-solid fa-envelope"></i> {{ contact.email }}</p>
        <p><i class="fa-solid fa-phone"></i> {{ contact.phone }}</p>
      </div>
    </div>

    <hr />

    <div class="actions">
      <button class="edit-btn" @click="$emit('edit')">
        <i class="fa-solid fa-pen"></i> Modifier
      </button>

      <button class="delete-btn" @click="$emit('delete')">
        <i class="fa-solid fa-trash"></i> Supprimer
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["contact"],
};
</script>


<style>
.card {
  background: white;
  padding: 20px;
  border-radius: 15px;
  width: 300px;
}

.top {
  display: flex;
  gap: 15px;
  align-items: center;
}

.avatar {
  width: 50px;
  height: 50px;
  background: #6c63ff;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  font-weight: bold;
}

.actions {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.edit-btn {
  background: #eee;
  border: none;
  padding: 8px;
  border-radius: 8px;
}

.delete-btn {
  background: red;
  color: white;
  border: none;
  padding: 8px;
  border-radius: 8px;
}
</style>



