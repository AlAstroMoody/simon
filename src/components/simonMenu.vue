<template>
  <div class="menu">
    <H2>Round: {{ round }}</H2>
    <span>{{ message }}</span>
    <button @click="startRound" :disabled="!active">{{ buttonTitle }}</button>
    <span>Game options: </span>
    <div class="menu__radio">
      <label for="easy"
        >Easy
        <input type="radio" id="easy" value="1" v-model="picked" />
      </label>
      <label for="medium"
        >Medium
        <input type="radio" id="medium" value="2" v-model="picked" />
      </label>
      <label for="hard"
        >Hard
        <input type="radio" id="hard" value="3" v-model="picked" />
      </label>
    </div>
  </div>
</template>

<script>
export default {
  name: "simonMenu",
  props: {
    round: {
      type: Number,
      default: 1,
    },
    active: {
      type: Boolean,
      default: true,
    },
    message: {
      type: String,
      default: "new game",
    },
  },
  data() {
    return {
      choice: 2,
    };
  },
  computed: {
    picked: {
      get() {
        return this.choice;
      },
      set(value) {
        this.choice = value;
      },
    },
    buttonTitle() {
      if (this.round === 1) {
        return "Start";
      } else return "Continue";
    },
  },
  methods: {
    startRound() {
      this.$emit("startRound", this.picked);
    },
  },
};
</script>

<style scoped>
.menu__radio {
  display: flex;
  text-align: right;
  flex-direction: column;
}
</style>
