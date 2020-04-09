<template>
  <section class="section">
    <div class="container">
      <ul class="list">
        <li 
          class="list-item"
          v-for="item in list"
          :key="item.id"
          :id="'item-' + item.id"
          :class="{ 'has-background-success has-text-light' : item.completed }"
        >
          <TodoItemContent
            @click.native="handleCheckItem(item)"
          >
            {{ item.title }}
          </TodoItemContent>

          <TodoButton
            :class="'delete'"
            @click.native="onClickRemove(item.id)"
          />
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import TodoItemContent from './TodoItemContent'
import TodoButton from './TodoButton'

export default {
  name: 'TodoList',

  components: {
    TodoItemContent,
    TodoButton,
  },

  props: {
    list: Array,
  },

  methods: {
    handleCheckItem(item) {
      item.completed = !item.completed;
    },

    onClickRemove(id) {
      this.$emit('remove-item', id);
    }
  }
}
</script>

<style lang="scss">
.list {
  &-item {
    display: grid;
    grid-template-columns: auto 20px;

    span {
      cursor: pointer;
    }
  }
}
</style>