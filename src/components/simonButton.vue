<template>
  <button class="button" :class="choiceColor" @click="clickButton" />
</template>

<script>
export default {
  name: "simonButton",
  props: {
    color: {
      type: Number,
      default: 1,
    },
    active: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    choiceColor() {
      if (this.color === 1) {
        return "red";
      }
      if (this.color === 2) {
        return "green";
      }
      if (this.color === 3) {
        return "blue";
      } else return "black";
    },
  },
  methods: {
    clickButton() {
      this.$emit("clickButton", this.color);
    },
  },
};
</script>

<style scoped>
.button {
  opacity: 0.5;
  transition: opacity 0.2s;
}

.button:active {
  opacity: 1;
  transition: background 0.2s;
}

.green {
  background: green;
}

.red {
  background: red;
}

.blue {
  background: blue;
}

.black {
  background: black;
}
</style>
