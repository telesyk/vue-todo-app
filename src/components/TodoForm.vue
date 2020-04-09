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
  </section>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
import TodoButton from './TodoButton'

export default {
  name: 'TodoForm',

  components: {
    TodoButton,
  },

  data() {
    return {
      inputValue: '',
      inputClass: '',
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
        console.debug('[debug] empty value');
        return;
      }
      if (value.length < 3) {
        console.debug('[debug] too small text');
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
