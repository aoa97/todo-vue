<template>
  <button
    class="delete-all"
    v-show="this.menuToggle() == 2"
    @click="deleteCompleted()"
  >
    <span class="material-icons"> delete_outline </span>
    delete all
  </button>
</template>

<script>
export default {
  name: "DeleteCompleted",
  inject: ["menuToggle", "todos", "setMenuToggle", "setTodos"],

  methods: {
    deleteCompleted() {
      const todos = this.todos().filter((t) => !t.done);
      this.setTodos(todos);
      localStorage.setItem("todos", JSON.stringify(todos));
      this.setMenuToggle(0); // toggle to all
    },
  },
};
</script>

<style scoped>
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
</style>