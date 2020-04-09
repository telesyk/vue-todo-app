<template>
  <section class="section">
    <form 
      class="field has-addons"
      @submit.prevent="handleSubmit"
    >
      <div class="control">
        <input 
          placeholder="Type here"
          class="input"
          type="text"
          :value="inputValue"
          :class="inputClass" 
          @input="handleChange"
        />
      </div>

      <div class="control">
        <TodoButton
          :class="'button is-info'"
          :type="'submit'"
        >
          Add
        </TodoButton>
      </div>
    </form>

    <Notification
      v-show="notification.show"
      :class="notification.class"
    >
      {{ notification.message }}
    </Notification>

  </section>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'
import Notification from './TodoNotification'
import TodoButton from './TodoButton'

export default {
  name: 'TodoForm',

  components: {
    TodoButton,
    Notification,
  },

  data() {
    return {
      inputValue: '',
      inputClass: '',
      notification: {
        show: false,
        message: '',
        class: 'is-warning',
      }
    }
  },

  methods: {
    handleSubmit() {
      let newItem = null;

      console.debug('[debug]1', this.inputValue.length);
      if (this.inputValue !== '' && this.inputValue.length > 2) {
        newItem = {
          id: uuidv4(),
          title: this.inputValue,
          completed: false
        };
        
        this.notification.show = false;
      } else {
        this.validation(this.inputValue);
        this.inputClass = 'is-danger';
      }

      this.$emit('add-item', newItem);
      this.inputValue = '';
    },

    handleChange(e) {
      this.inputClass = '';
      this.inputValue = e.target.value;
    },

    validation(value) {
      if (value === '') {
        // console.debug('[debug] empty value');
        this.notification.show = true;
        this.notification.class = 'is-danger';
        this.notification.message = 'Todo Item can`t be empty!';
        return;
      }
      if (value.length < 3) {
        // console.debug('[debug] too small text');
        this.notification.show = true;
        this.notification.class = 'is-warning';
        this.notification.message = 'Todo Item should not be less than 3 symbols and more than 100 symbols!';
        return;
      }
    }
  }
}
</script>

<style lang="scss">
  .field.has-addons {
    display: grid;
    grid-template-columns: auto 63px;
    justify-content: stretch;
  }
</style>
