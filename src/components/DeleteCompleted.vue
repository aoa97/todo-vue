<template>
  <button v-show="menuToggle == 2" @click="deleteCompleted()">
    <span class="material-icons"> delete_outline </span>
    delete all
  </button>
</template>

<script>
export default {
  name: "DeleteCompleted",
  props: ["todos", "menuToggle"],
  emits: ["setTodos", "setMenuToggle"],

  methods: {
    deleteCompleted() {
      const todos = this.todos.filter((t) => !t.done);
      this.$emit("setTodos", todos);
      localStorage.setItem("todos", JSON.stringify(this.todos));
      this.$emit("setMenuToggle", 0);
    },
  },
};
</script>

<style scoped lang="scss">
button {
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

  &:hover {
    opacity: 1;
  }

  span {
    font-size: 1.2rem;
    margin-right: 0.55rem;
  }
}
</style>