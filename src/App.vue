<template>
  <div id="app">
    <link
      href="https://unpkg.com/todomvc-app-css@2.1.0/index.css"
      rel="stylesheet"
    />

    <section class="todoapp">
      <header class="header">
        <h1>todos</h1>
        <input
          class="new-todo"
          autocomplete="off"
          placeholder="What needs to be done?"
          @keyup.enter="addTodo"
          v-model="newTodo"
        />
      </header>
      <section class="main">
        <input
          class="toggle-all"
          id="toggle-all"
          type="checkbox"
          v-model="allDone"
        />
        <label for="toggle-all">Mark all as complete</label>

        <ul class="todo-list">
          <li
            class="todo"
            :class="{ completed: todo.completed, editing: todo == editingTodo }"
            v-for="(todo, i) in filterdTodos"
            :key="i"
          >
            <div class="view">
              <input class="toggle" type="checkbox" v-model="todo.completed" />
              <label @dblclick="editTodo(todo)">{{ todo.title }}</label>
              <button class="destroy" @click="deleteTodo(i)"></button>
            </div>
            <input
              class="edit"
              type="text"
              v-model="todo.title"
              @keyup.enter="doneEditing()"
            />
          </li>
        </ul>
      </section>
      <footer class="footer">
        <span class="todo-count">
          <strong>
            {{ remaningTodos }}
            {{ remaningTodos > 1 ? "items left" : "item left" }}</strong
          >
          <!-- <span v-if="remaningTodos > 1"> items left </span>
          <span v-else> item left </span> -->
        </span>
        <ul class="filters">
          <li>
            <a
              href="#"
              @click.prevent="visability = 'all'"
              :class="{ selected: visability == 'all' }"
              >All</a
            >
          </li>
          <li>
            <a
              href="#"
              @click.prevent="visability = 'active'"
              :class="{ selected: visability == 'active' }"
              >Active</a
            >
          </li>
          <li>
            <a
              href="#"
              @click.prevent="visability = 'completed'"
              :class="{ selected: visability == 'completed' }"
              >Completed</a
            >
          </li>
        </ul>
        <button class="clear-completed" @click="removeCompletedTodos">
          Clear completed
        </button>
      </footer>
    </section>
    <footer class="info">
      <p>Double-click to edit a todo</p>
    </footer>

    <router-view />
  </div>
</template>
<script>
let filters = {
  all: function (todos) {
    return todos;
  },
  active: function (todos) {
    return todos.filter(function (todo) {
      return !todo.completed;
    });
  },
  completed: function (todos) {
    return todos.filter(function (todo) {
      return todo.completed;
    });
  },
};
export default {
  name: "app",
  data() {
    return {
      newTodo: "",
      editingTodo: null,
      visability: "all",
      todos: [
        { title: "test1", completed: false },
        { title: "test 2", completed: false },
        { title: "test 3", completed: false },
        { title: "test 4", completed: false },
      ],
    };
  },
  computed: {
    filterdTodos: function () {
      return filters[this.visability](this.todos);
    },
    remaningTodos: function () {
      return filters.active(this.todos).length;
    },
    allDone: {
      get: function () {
        return this.remaningTodos === 0;
      },
      set: function (value) {
        this.todos.forEach(function (todo) {
          todo.completed = value;
        });
      },
    },
  },
  methods: {
    deleteTodo: function (todo) {
      this.todos.splice(todo, 1);
    },
    addTodo: function () {
      let todo = this.newTodo.trim();
      if (todo) {
        this.todos.push({ title: todo, completed: false });
        this.newTodo = "";
      }
    },
    removeCompletedTodos() {
      this.todos = filters.active(this.todos);
    },
    editTodo(todo) {
      this.editingTodo = todo;
    },
    doneEditing() {
      if (this.editingTodo.title == "") this.deleteTodo(this.editingTodo);
      this.editingTodo = null;
    },
  },
};
</script>
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
