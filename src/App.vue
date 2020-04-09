<template>
  <div id="app" class="app">

    <TodoForm
      @add-item="addItem"
    />

    <TodoList 
      :list="list"
      @remove-item="removeItem"
    />

  </div>
</template>

<script>
// import data
import * as store from './data.js';
// import components
import TodoList from './components/TodoList';
import TodoForm from './components/TodoForm';

export default {
  name: 'App',

  components: {
    TodoForm,
    TodoList,
  },
  
  data () {
    return {
      list: store.data.list || [],
    }
  },

  methods: {
    removeItem(id) {
      this.list = this.list.filter(item => item.id !== id);
    },

    addItem(newItem) {
      if (newItem !== null) {
        this.list = [...this.list.reverse(), newItem].reverse();
      }
    },
  },

  // mounted() {
  //   console.debug('test mounted method')
  // },
}
</script>

<style lang="scss">
.app {
  height: 100vh;
  padding: 0 3rem;
}
</style>