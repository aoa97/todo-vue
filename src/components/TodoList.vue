<template >
  <ul class="list">
    <li v-for="todo in ftodos" :key="todo" :class="todo.done && 'checked'">
      <input type="checkbox" v-model="todo.done" @change="onChecked()" />

      <span>{{ todo.title }}</span>

      <button class="delete-item" @click="deleteTodo(todo.id)">
        <span class="material-icons"> delete_outline </span>
      </button>
    </li>
  </ul>
</template>

<script>
export default {
  name: "TodoList",
  props: ["menuToggle", "todos"],
  emits: ["setTodos"],

  methods: {
    onChecked() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },

    deleteTodo(id) {
      const newTodos = this.todos.filter((t) => t.id !== id);
      this.$emit("setTodos", newTodos);
      localStorage.setItem("todos", JSON.stringify(newTodos));
    },
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

<style scope lang="scss">
ul.list {
  margin-top: 3.3rem;
  display: flex;
  flex-direction: column;
  row-gap: 2.7rem;

  li {
    font-size: 1.8rem;
    font-weight: 500;
    display: flex;
    align-items: center;
    column-gap: 0.7rem;

    &.checked {
      text-decoration: line-through;
    }

    input {
      cursor: pointer;
      width: 2.4rem;
      height: 2.4rem;
      border-radius: 0.4rem;
    }

    button.delete-item {
      margin-left: auto;

      &:hover span {
        color: #2f80ed;
      }

      span {
        color: #bdbdbd;
        transition: color 0.5s ease;
      }
    }
  }
}
</style>