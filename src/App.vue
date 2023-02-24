<script>
import StatusFilter from "./components/StatusFilter.vue";
import TodoItem from './components/TodoItem.vue';
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
  components: {
    StatusFilter,
    TodoItem
  },
  data() {
    return {
      todos: [],
      newTitle: "",
      status: "all",
    };
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
        case "active":
          return this.activeTodos;
        case "completed":
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
        write("todos", this.todos);
      },
    },
  },
  mounted() {
    this.todos = read("todos", []);
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: Date.now(),
        title: this.newTitle,
        completed: false,
      });
      this.newTitle = "";
    },
    deleteTodo(todoId) {
      const index = this.todos.findIndex(todo => todo.id === todoId);
      this.todos.splice(index, 1);
    },
    toggleTodo() {

    },
    renameTodo(newTitle) {

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
        <TodoItem
          v-for="todo, index of visibleTodos"
          :key="todo.id"
          :todo="todo"
          @delete="deleteTodo"
          @rename="todo.title = $event"
          @toggle="todo.completed = !todo.completed"
        />
      </section>

      <footer class="todoapp__footer">
        <span class="todo-count">
          {{ activeTodos.length }} items left
        </span>

        <StatusFilter
          :status="status"
          @update="status = $event"
        />

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
