<template>
  <div>
    <h1>Todo List</h1>
    <input v-model="newTodo" placeholder="Add new todo">
    <button @click="addTodo">Add</button>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        {{ todo }}
        <button @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { useStore } from 'vuex';

export default {
  setup() {
    const newTodo = ref('');
    const store = useStore();
    const todos = computed(() => store.state.todos);

    function addTodo() {
      if (newTodo.value) {
        store.commit('addTodo', newTodo.value);
        newTodo.value = '';
      }
    }

    function removeTodo(index) {
      store.commit('removeTodo', index);
    }

    return {
      newTodo,
      todos,
      addTodo,
      removeTodo,
    };
  },
};
</script>
