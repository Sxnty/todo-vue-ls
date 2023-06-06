<script setup>
import { ref, onMounted, computed, watch } from 'vue';
import { MdDeleteforever } from 'oh-vue-icons/icons'
import { OhVueIcon, addIcons } from "oh-vue-icons";

let todos = ref([]);
const name = ref('');

const input_title = ref('');
const input_description = ref('');
const input_category = ref(null);

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  if (todos.value.length === 0) {
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  }

})

const addTodo = () => {
  if (input_title.value === '' || !input_category.value) {
    return
  }
  todos.value.push({
    title: input_title.value,
    category: input_category.value,
    description: input_description.value,
    done: false,
  })
}
watch(name, (newValue) => {
  localStorage.setItem('name', newValue);
})
watch(todos, (newVal) => {
  console.log(todos)
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

const deleteTask = (id) => {
  let updatedTodos = todos.value.filter((e) => e.title !== id)
  todos.value = updatedTodos;
  localStorage.setItem('todos', JSON.stringify(todos))
}
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
        <input type="text" placeholder="Enter task title" v-model="input_title">
        <textarea type="text" placeholder="Task description" v-model="input_description" />
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

    <div>
      <section class="todo-list" v-for="todo in todos" :key="todo.title">
        <div class="todo-item">
          <div class="todo-heading">
            <h1>{{ todo.title }}</h1>
            <button @click="deleteTask(todo.title)">Delete</button>
          </div>
          <label>{{ todo.description }}</label>
          <br>
          <p>Category: {{ todo.category }}</p>
          <span>
          </span>
        </div>
      </section>
    </div>




  </main>
</template>

<style scoped></style>
