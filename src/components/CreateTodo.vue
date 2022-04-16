<template>
  <form @submit.prevent="addTodo" v-show="menuToggle !== 2">
    <input placeholder="add details" v-model="newTodo" />
    <button :disabled="newTodo.length == 0">Add</button>
  </form>
</template>

<script>
let id = 0;

export default {
  name: "CreateTodo",
  inject: ["menuToggle", "todos"],

  data() {
    return {
      newTodo: "",
    };
  },

  methods: {
    addTodo() {
      this.todos().push({ id: ++id, title: this.newTodo });
      localStorage.setItem("todos", JSON.stringify(this.todos()));
      this.newTodo = "";
    },
  },
};
</script>

<style scoped>
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
</style>