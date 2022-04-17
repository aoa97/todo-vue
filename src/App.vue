<template>
  <main>
    <h1>#todo</h1>

    <MenuLinks
      :todos="todos"
      :menuToggle="menuToggle"
      @setMenuToggle="(x) => (menuToggle = x)"
    />

    <CreateTodo
      :todos="todos"
      :menuToggle="menuToggle"
      @setTodos="(x) => (todos = x)"
    />

    <TodoList
      :todos="todos"
      :menuToggle="menuToggle"
      @setTodos="(x) => (todos = x)"
    />

    <DeleteCompleted
      :todos="todos"
      :setTodos="setTodos"
      :menuToggle="menuToggle"
      @setTodos="(x) => (todos = x)"
      @setMenuToggle="(x) => (menuToggle = x)"
    />
  </main>
</template>

<script>
import { MenuLinks, TodoList, CreateTodo, DeleteCompleted } from "./components";

export default {
  name: "App",

  components: {
    MenuLinks,
    TodoList,
    CreateTodo,
    DeleteCompleted,
  },

  data() {
    return {
      menuToggle: 0,
      todos: [],
    };
  },

  methods: {
    setMenuToggle(x) {
      this.menuToggle = x;
    },
    setTodos(todos) {
      this.todos = todos;
    },
  },

  provide() {
    return {
      menuToggle: () => this.menuToggle,
      todos: () => this.todos,
      setMenuToggle: (x) => {
        this.menuToggle = x;
      },
      setTodos: (todos) => {
        this.todos = todos;
      },
    };
  },

  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos")) || [];
  },
};
</script>

<style lang="scss">
/* Start Global */
* {
  margin: 0;
  padding: 0;
  background: border-box;
}

html {
  font-family: "Montserrat", sans-serif;
  font-size: 10px;
}

body {
  font-size: 1.4rem;
  color: #333;
  display: flex;
  justify-content: center;
  min-height: 100vh;
}
/* End Global */

/* Start Resets */
ul {
  list-style: none;
}

button {
  cursor: pointer;
  border: none;
}
/* End Resets */

#app {
  width: 45%;

  main {
    padding: 3.2rem 0;
  }

  h1 {
    font-family: "Raleway", sans-serif;
    font-size: 3.6rem;
    font-weight: 700;
    margin-bottom: 5.8rem;
    text-align: center;
  }

  @media screen and (max-width: 1150px) {
    width: 60%;
  }

  @media screen and (max-width: 520px) {
    width: 80%;
  }
}
</style>

