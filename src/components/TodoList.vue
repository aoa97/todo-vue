<template >
  <ul class="todos">
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
  inject: ["menuToggle", "todos", "setTodos"],

  methods: {
    onChecked() {
      localStorage.setItem("todos", JSON.stringify(this.todos()));
    },

    deleteTodo(id) {
      const todos = this.todos().filter((t) => t.id !== id);
      this.setTodos(todos);
      localStorage.setItem("todos", JSON.stringify(todos));
    },
  },

  computed: {
    ftodos() {
      return this.menuToggle() === 1
        ? this.todos().filter((t) => !t.done)
        : this.menuToggle() == 2
        ? this.todos().filter((t) => t.done)
        : this.todos();
    },
  },
};
</script>

<style scoped>
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
</style>