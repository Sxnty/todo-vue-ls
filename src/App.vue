<script setup>
import { ref, onMounted, computed, watch } from 'vue';
const todo = ref([]);
const name = ref('');

const input_content = ref('');
const input_category = ref(null);

watch(name, (newValue) => {
  localStorage.setItem('name', newValue);
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})



</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Whats up, <input type="text" placeholder="Name here" v-model="name">
      </h2>
    </section>

    <section class="create-todo">
      <h3>Create a task.</h3>
      <form @submit.prevent="addTodo">
        <h4>Task title</h4>
        <input type="text" placeholder="leave the dog" v-model="input_content">
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" value="bussines" v-model="input_category">
            <span class="bubble bussiness"></span>
            <div>Bussines</div>
          </label>

          <label>
            <input type="radio" name="category" value="personal" v-model="input_category">
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add task.">
      </form>
    </section>

  </main>
</template>

<style scoped></style>
