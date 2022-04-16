<template>
  <TodoList />
</template>

<script>
import TodoList from "./components/TodoList.vue";

export default {
  name: "App",
  components: {
    TodoList,
  },
};
</script>

<style>
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
  min-height: 100vh
}

ul {
  list-style: none;
}

button {
  cursor: pointer;
  border: none;
}
</style>
