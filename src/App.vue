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
    <el-button type="primary" @click="fetchData">Fetch Data</el-button>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { useStore } from 'vuex';
import axios from 'axios';
import Mock from 'mockjs';

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

    function fetchData() {
      Mock.mock('https://api.example.com/data', {
        'data|5': [
          {
            'id|+1': 1,
            'title': '@sentence(5, 10)',
            'completed': '@boolean',
          },
        ],
      });

      axios.get('https://api.example.com/data')
        .then(response => {
          console.log(response.data);
        })
        .catch(error => {
          console.error(error);
        });
    }

    return {
      newTodo,
      todos,
      addTodo,
      removeTodo,
      fetchData,
    };
  },
};
</script>
