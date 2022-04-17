<template >
  <ul class="menu">
    <li id="all" :class="setActive(0)" @click="$emit('setMenuToggle', 0)">
      All
    </li>

    <li id="active" :class="setActive(1)" @click="$emit('setMenuToggle', 1)">
      Active
    </li>

    <li id="completed" :class="setActive(2)" @click="$emit('setMenuToggle', 2)">
      Completed
    </li>
  </ul>
</template>

<script>
export default {
  name: "MenuLinks",
  props: ["menuToggle", "todos"],
  emits: ["setMenuToggle"],

  methods: {
    setActive(op) {
      return this.menuToggle == op && "active";
    },
  },
};
</script>

<style scoped lang="scss">
ul.menu {
  border-bottom: 1px solid #bdbdbd;
  display: flex;
  justify-content: space-around;
  margin-bottom: 2rem;

  li {
    cursor: pointer;
    flex-basis: 8.9rem;
    text-align: center;
    font-weight: 600;

    &.active::after {
      content: "";
      margin-top: 1.5rem;
      display: block;
      transform: rotate(180deg);
      border-bottom: 0.4rem solid #2f80ed;
      border-bottom-left-radius: 0.4rem;
      border-bottom-right-radius: 0.4rem;
    }

    &.disabled {
      pointer-events: none;
      opacity: 0.5;
    }
  }
}
</style>