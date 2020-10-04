<template>
  <h2>Vue 3 Todo App</h2>
    <form @submit.prevent="addNewTodo">
      <label>New Todo List</label>
      <input v-model="newTodo" name="newTodo">
      <button>Add New Task</button>
    </form>
    <button @click="strikeAll">Strike All</button>
    <button @click="removeAll">Remove All</button>
    <div>   
      <ul>
        <li v-for="(todo, index ) in todos" v-bind:key="todo.id" class="todo"> <!-- iterates over unique elements in the array 'todos' -->
          <h4 @click="toggleDone(todo)" :class="{ done: todo.done }">{{todo.content}}</h4>    <!-- "{ done: todo.done }" Shorthand for "{ done: todo.done == true }", and applies the .done css -->
          <button @click="removeTodo(index)">Delete</button>
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

</style>
