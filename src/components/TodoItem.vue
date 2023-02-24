<script>
export default {
  name: 'TodoItem',
  props: {
    todo: Object,
  },
  emits: ['delete', 'rename', 'toggle'],
  data() {
    return {
      editing: false,
      newTitle: this.todo.title,
    };
  },
  methods: {
    rename() {
      this.editing = false;
      this.$emit('rename', this.newTitle);
    },
  }
}
</script>

<template>
  <div
    class="todo"
    :class="{ completed: todo.completed }"
  >
    <label class="todo__status-label">
      <input
        :value="todo.completed"
        @change="$emit('toggle')"
        type="checkbox"
        class="todo__status"
      />
    </label>

    <form v-if="editing" @submit.prevent="rename">
      <input
        type="text"
        class="todo__title-field"
        placeholder="Empty todo will be deleted"
        v-model.trim="newTitle"
        @keyup.esc="editing = false"
        @blur="rename"
      />
    </form>

    <template v-else>
      <span class="todo__title" @dblclick="editing = true">
        {{ todo.title }}
      </span>

      <button
        @click="$emit('delete', todo.id)"
        class="todo__remove"
      >
        x
      </button>
    </template>

    <div
      class="modal overlay"
      :class="{ 'is-active': false }"
    >
      <div class="modal-background has-background-white-ter"></div>
      <div class="loader"></div>
    </div>
  </div>
</template>