<template>
  <h2>Vue 3 Todo App</h2>
    <form @submit.prevent="addNewTodo">
      <label>New Todo List</label>
      <input v-model="newTodo" name="newTodo">
      <button class="btn btn-outline-primary">Add New Task</button>
    </form>
    <button class="btn btn-outline-success" @click="strikeAll">Strike All</button>
    <button class="btn btn-outline-danger" @click="removeAll">Remove All</button>
    <div>   
      <ul>
        <li v-for="(todo, index ) in todos" v-bind:key="todo.id" class="todo"> <!-- iterates over unique elements in the array 'todos' -->
          <h4 @click="toggleDone(todo)" :class="{ done: todo.done }">{{todo.content}}</h4>    <!-- "{ done: todo.done }" Shorthand for "{ done: todo.done == true }", and applies the .done css -->
          <button class="btn btn-outline-danger btn-sm" @click="removeTodo(index)"><svg width="1em" height="1em" viewBox="0 0 16 16" class="bi bi-trash-fill" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd" d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5a.5.5 0 0 0-1 0v7a.5.5 0 0 0 1 0v-7z"/>
          </svg></button>
        </li>
      </ul>
    </div>

</template>

<script>
import { ref } from 'vue';              // Imports 'ref' library

export default {
  setup(){
    const newTodo = ref('');            // Creates a v-model with empty reactive element
    const todos = ref([]);

    function addNewTodo(){
      console.log(newTodo.value);
      todos.value.push({
        id: Date.now(),                 // Needed for unique keys in iteration
        done: false,
        content: newTodo.value,
      });
      newTodo.value = '';                // Changes to the v-model also changes the field, & vis a vis (similar to "2-way binding")
    }
    
    function toggleDone(todo){
      todo.done = !todo.done;
    }

    function removeTodo(index){
      todos.value.splice(index, 1)
    }

    function strikeAll(){
      todos.value.forEach((todo) => todo.done = true);
    }

    function removeAll(){
      todos.value = [];
    }


    return {
      addNewTodo,                     // Exposes these variables to the v-model
      newTodo,
      todos,
      toggleDone,
      removeTodo,
      strikeAll,
      removeAll,
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
}
input, textarea, button, p, div, section, article, select {
  display: 'block';
  width: 100%;
  font-family: sans-serif;
  font-size: 1em;
  margin: 0.5em;
}
.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}

.btn-sm {
  width: 2.5rem;
}
</style>
