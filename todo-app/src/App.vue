<template>
  <h1 class="header">TODO App</h1>
  <form @submit.prevent="addNewTodo">
    <label>New todo:</label>
    <input v-model="newTodo" name="newTodo">
    <button>Add</button>
  </form>
  <ul>
    <li v-for="(todo, index) in todos" :key="todo.id" class="todo">
      <h3 :class="{ done: todo.done }" @click="toggleTodo(todo)">{{ todo.content }}</h3>
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>
  <button @click="markAllAsDone">All done</button>
  <button @click="removeAll">Remove all</button>
</template>

<script>
import { ref } from 'vue';

export default {
  setup(){
    const newTodo = ref('');
    const todos = ref([]);

    function addNewTodo(){
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value
      });
      newTodo.value = '';
    }

    function toggleTodo(todo){
      todo.done = !todo.done;
    }

    function removeTodo(index){
      todos.value.splice(index, 1);
    }

    function markAllAsDone(){
      todos.value.forEach( (todo) => todo.done = true);
    }

    function removeAll() {
      todos.value = [];
    }

    return {
      removeTodo,
      toggleTodo,
      todos,
      newTodo,
      addNewTodo,
      markAllAsDone,
      removeAll
    }
  }
}
</script>

<style>
body {
  font-family: sans-serif;
  padding-top: 1em;
  padding-bottom: 1em;
  font-size: 2em;
  width: 80%;
  margin: 0 auto;
  background-color: lightgrey;
}
input, textarea, button, p, div, section, article, select {
  display: block;
  width: 100%;
  font-family: sans-serif;
  font-size: 1em;
  margin: 0.5em;
}
.done {
  text-decoration: line-through;
  color: lightslategray;
}
.todo {
  cursor: pointer;
  color: steelblue;
}
</style>
