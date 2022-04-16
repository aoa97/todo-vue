<template>
  <h1>#todo</h1>

  <MenuLinks />
  <CreateTodo />
  <TodoList />
  <DeleteCompleted /> 
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

  watch: {
    todos: {
      handler(newTodos) {
        // Disable completed section if nothing completed
        const completed = newTodos.find((t) => t.done === true);

        if (!completed) {
          document.querySelector("#completed").classList.add("disabled");
        } else {
          document.querySelector("#completed").classList.remove("disabled");
        }

        // Disable active section if nothing active
        const active = newTodos.find((t) => t.done !== true);
        if (!active) {
          document.querySelector("#active").classList.add("disabled");
        } else {
          document.querySelector("#active").classList.remove("disabled");
        }
      },
      deep: true,
    },
  },
};
</script>

<style>
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
  padding: 3.2rem 0;
}

#app {
  width: 45%;
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

h1 {
  font-family: "Raleway", sans-serif;
  font-size: 3.6rem;
  font-weight: 700;
  margin-bottom: 5.8rem;
  text-align: center;
}

@media screen and (max-width: 1150px) {
  #app {
    width: 60%;
  }
}

@media screen and (max-width: 520px) {
  #app {
    width: 80%;
  }
}
</style>

