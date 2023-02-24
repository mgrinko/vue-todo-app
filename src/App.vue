<script>
import todosFromAPI from "./data/todos";

function read(key, defaultValue) {
  const value = localStorage.getItem(key);

  if (value === null) {
    return defaultValue;
  }

  try {
    return JSON.parse(value);
  } catch (error) {
    return defaultValue;
  }
}

function write(key, value) {
  localStorage.setItem(key, JSON.stringify(value));
}

export default {
  data() {
    return {
      todos: [],
      newTitle: '',
      status: 'all',
    }
  },
  computed: {
    activeTodos() {
      return this.todos.filter(todo => !todo.completed);
    },
    completedTodos() {
      return this.todos.filter(todo => todo.completed);
    },
    visibleTodos() {
      switch (this.status) {
        case 'active':
          return this.activeTodos;
        case 'completed':
          return this.completedTodos;
        default:
          return this.todos;
      }
    },
  },
  watch: {
    todos: {
      deep: true,
      handler() {
        write('todos', this.todos);
      },
    },
  },
  mounted() {
    this.todos = read('todos', []);
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: Date.now(),
        title: this.newTitle,
        completed: false,
      });

      this.newTitle = '';
    },
    deleteTodo(todoId) {
      const index = this.todos.findIndex(todo => todo.id === todoId);

      this.todos.splice(index, 1);
    },
  },
}
</script>

<template>
  <div class="todoapp">
    <h1 class="todoapp__title">todos</h1>

    <div class="todoapp__content">
      <header class="todoapp__header">
        <button class="todoapp__toggle-all active"></button>

        <form @submit.prevent="addTodo">
          <input
            v-model="newTitle"
            type="text"
            class="todoapp__new-todo"
            placeholder="What needs to be done?"
          />
        </form>
      </header>

      <section class="todoapp__main">
        <div
          v-for="todo, index of visibleTodos"
          :key="todo.id"
          class="todo"
          :class="{ completed: todo.completed }"
        >
          <label class="todo__status-label">
            <input
              v-model="todo.completed"
              type="checkbox"
              class="todo__status"
            />
          </label>

          <form v-if="false">
            <input
              type="text"
              class="todo__title-field"
              placeholder="Empty todo will be deleted"
              value="Todo is being edited now"
            />
          </form>

          <template v-else>
            <span class="todo__title">{{ todo.title }}</span>

            <button
              @click="deleteTodo(todo.id)"
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
      </section>

      <footer class="todoapp__footer">
        <span class="todo-count">
          {{ activeTodos.length }} items left
        </span>

        <nav class="filter">
          <a
            href="#/"
            class="filter__link"
            :class="{ selected: status === 'all' }"
            @click="status = 'all'"
          >
            All
          </a>

          <a
            href="#/active"
            class="filter__link"
            :class="{ selected: status === 'active' }"
            @click="status = 'active'"
          >
            Active
          </a>

          <a
            href="#/completed"
            class="filter__link"
            :class="{ selected: status === 'completed' }"
            @click="status = 'completed'"
          >
            Completed
          </a>
        </nav>

        <button class="todoapp__clear-completed">
          Clear completed
        </button>
      </footer>
    </div>

    <article class="message is-danger message--hidden">
      <div class="message-header">
        <p>Error</p>
        <button class="delete"></button>
      </div>

      <div class="message-body">
        Unable to add a Todo
      </div>
    </article>
  </div>
</template>

<style></style>
