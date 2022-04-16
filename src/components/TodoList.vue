<template >
  <div class="container">
    <h1>#todo</h1>

    <!-- Menu -->
    <ul class="menu">
      <li :class="setActive(0)" @click="menuToggle = 0">All</li>
      <li ref="active" :class="setActive(1)" @click="menuToggle = 1">Active</li>
      <li ref="completed" :class="setActive(2)" @click="menuToggle = 2">
        Completed
      </li>
    </ul>

    <!-- Add todo form -->
    <form @submit.prevent="addTodo" v-show="menuToggle !== 2">
      <input placeholder="add details" v-model="newTodo" />
      <button :disabled="newTodo.length == 0">Add</button>
    </form>

    <!-- Todos -->
    <ul class="todos">
      <li v-for="todo in ftodos" :key="todo" :class="todo.done && 'checked'">
        <input type="checkbox" v-model="todo.done" @change="onChecked()" />

        <span>{{ todo.title }}</span>

        <button class="delete-item" @click="deleteTodo(todo.id)">
          <span class="material-icons"> delete_outline </span>
        </button>
      </li>
    </ul>

    <!-- Delete button (Completed) -->
    <button
      class="delete-all"
      v-show="menuToggle == 2"
      @click="deleteCompleted()"
    >
      <span class="material-icons"> delete_outline </span>
      delete all
    </button>
  </div>
</template>

<script>
let id = 0;

export default {
  data() {
    return {
      menuToggle: 0,
      newTodo: "",
      todos: [],
    };
  },

  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos")) || [];
  },

  computed: {
    ftodos() {
      return this.menuToggle === 1
        ? this.todos.filter((t) => !t.done)
        : this.menuToggle == 2
        ? this.todos.filter((t) => t.done)
        : this.todos;
    },
  },

  methods: {
    addTodo() {
      this.todos.push({ id: ++id, title: this.newTodo });
      localStorage.setItem("todos", JSON.stringify(this.todos));
      this.newTodo = "";
    },

    onChecked() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },

    setActive(op) {
      return this.menuToggle == op && "active";
    },

    deleteCompleted() {
      const todos = this.todos.filter((t) => !t.done);
      this.todos = todos;
      localStorage.setItem("todos", JSON.stringify(todos));
      this.menuToggle = 0; // toggle to all
    },

    deleteTodo(id) {
      const todos = this.todos.filter((t) => t.id !== id);
      this.todos = todos;
      localStorage.setItem("todos", JSON.stringify(todos));
    },
  },
  watch: {
    todos: {
      handler(newTodos) {
        // Disable completed section if nothing completed
        const completed = newTodos.find((t) => t.done === true);

        if (!completed) {
          this.$refs.completed.classList.add("disabled");
        } else {
          this.$refs.completed.classList.remove("disabled");
        }

        // Disable active section if nothing active
        const active = newTodos.find((t) => t.done !== true);
        if (!active) {
          this.$refs.active.classList.add("disabled");
        } else {
          this.$refs.active.classList.remove("disabled");
        }
      },
      deep: true,
    },
  },
};
</script>

<style>
.container {
  width: 45vw;
  padding: 3.2rem 0;
}

h1 {
  font-family: "Raleway", sans-serif;
  font-size: 3.6rem;
  font-weight: 700;
  margin-bottom: 5.8rem;
  text-align: center;
}

/* Start Menu */
ul.menu {
  border-bottom: 1px solid #bdbdbd;
  display: flex;
  justify-content: space-around;
  margin-bottom: 2rem;
}

ul.menu li {
  cursor: pointer;
  flex-basis: 8.9rem;
  text-align: center;
  font-weight: 600;
}

ul.menu li.active::after {
  content: "";
  margin-top: 1.5rem;
  display: block;
  transform: rotate(180deg);
  border-bottom: 0.4rem solid #2f80ed;
  border-bottom-left-radius: 0.4rem;
  border-bottom-right-radius: 0.4rem;
}

ul.menu li.disabled {
  pointer-events: none;
  opacity: 0.5;
}
/* End Menu */

/* Start Form */
form {
  display: flex;
  justify-content: space-between;
  column-gap: 2.5rem;
}

form input {
  outline: 0;
  width: 70%;
  padding: 2rem 1.2rem;
  border: 0.1rem solid #bdbdbd;
  border-radius: 1.2rem;
}

form button {
  text-align: center;
  padding: 2rem 4rem;
  background: #2f80ed;
  color: #fff;
  font-weight: 600;
  border-radius: 1.2rem;
  box-shadow: 0px 2px 6px rgba(127, 177, 243, 0.4);
  opacity: 0.8;
  transition: opacity 0.5s ease;
}

form button:disabled {
  pointer-events: none;
  opacity: 0.5;
}

form button:hover {
  opacity: 1;
}

/* End Form */

/* Start Todos */
ul.todos {
  margin-top: 3.3rem;
  display: flex;
  flex-direction: column;
  row-gap: 2.7rem;
}

ul.todos li {
  font-size: 1.8rem;
  font-weight: 500;
  display: flex;
  align-items: center;
  column-gap: 0.7rem;
}

ul.todos li.checked {
  text-decoration: line-through;
}

ul.todos li input {
  cursor: pointer;
  width: 2.4rem;
  height: 2.4rem;
  border-radius: 0.4rem;
}

ul.todos li button.delete-item {
  margin-left: auto;
}

ul.todos li button.delete-item:hover span {
  color: #2f80ed;
}

ul.todos li button.delete-item span {
  color: #bdbdbd;
  transition: color 0.5s ease;
}

button.delete-all {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 1rem;
  padding: 1.2rem 2.6rem;
  background: #eb5757;
  color: #fff;
  font-size: 1.2rem;
  font-weight: 600;
  border-radius: 0.4rem;
  opacity: 0.8;
  transition: opacity 0.5s ease;
  margin-left: auto;
}

button.delete-all span {
  font-size: 1.2rem;
  margin-right: 0.55rem;
}

button.delete-all:hover {
  opacity: 1;
}
/* End Todos */

/* Start Media Queries */

@media screen and (max-width: 1150px) {
  .container {
    width: 70vw;
  }
}

@media screen and (max-width: 520px) {
  .container {
    width: 80vw;
  }
}

@media screen and (max-width: 680px) {
  form input {
    align-self: stretch;
  }
}

@media screen and (max-width: 376px) {
  form {
    flex-direction: column;
    align-items: flex-start;
    row-gap: 1.5rem;
  }
}
/* End Media Queries */
</style>