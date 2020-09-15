<template>
  <div class="simon">
    <section class="simon__main">
      <simon-button
        :color="1"
        class="simon__button"
        @clickButton="clickButton"
      />
      <simon-button
        :color="2"
        class="simon__button"
        @clickButton="clickButton"
      />
      <simon-button
        :color="3"
        class="simon__button"
        @clickButton="clickButton"
      />
      <simon-button
        :color="4"
        class="simon__button"
        @clickButton="clickButton"
      />
    </section>
    <simon-menu
      :round="round"
      @startRound="startRound"
      class="simon__menu"
      :active="activeStartButton"
      :message="message"
    />
  </div>
</template>

<script>
import SimonButton from "@/components/simonButton";
import SimonMenu from "@/components/simonMenu";

export default {
  name: "Home",
  data() {
    return {
      round: 1,
      sequence: [],
      userSequence: [],
      numberClick: 0,
      activeStartButton: true,
      message: "",
      sounds: [
        new Audio(require("../assets/sounds/1.mp3")),
        new Audio(require("../assets/sounds/2.mp3")),
        new Audio(require("../assets/sounds/3.mp3")),
        new Audio(require("../assets/sounds/4.mp3")),
        new Audio(require("../assets/sounds/fail.mp3")),
      ],
    };
  },
  components: { SimonMenu, SimonButton },
  methods: {
    clickButton(color) {
      this.numberClick += 1;
      this.userSequence.push(color - 1);
      this.sounds[color - 1].play();
      if (
        this.userSequence[this.numberClick - 1] !==
        this.sequence[this.numberClick - 1]
      ) {
        this.round = 1;
        this.activeStartButton = true;
        if (this.sequence.length > 0) {
          this.sounds[4].play();
          this.message = "you lose";
          this.sequence = [];
        }
      }
      if (this.numberClick === this.sequence.length) {
        this.round += 1;
        this.activeStartButton = true;
      }
    },
    startRound(level) {
      if (this.round % 2) {
        this.message = "fight like a lion";
      } else this.message = "so far so good";
      this.activeStartButton = false;
      this.numberClick = 0;
      this.userSequence = [];
      this.sequence.push(Math.floor(Math.random() * Math.floor(4)));
      let buttons = document.querySelectorAll("section button");
      let timeout = 0;
      let step;
      if (level === "1") {
        step = 1500;
      }
      if (level === "2") {
        step = 1000;
      }
      if (level === "3") {
        step = 400;
      }
      for (let i = 0; i < this.sequence.length; i++) {
        if (i === 0) {
          timeout = 0;
        } else timeout += step;
        this.signal(this.sequence[i], buttons, timeout, step);
      }
    },
    signal(i, buttons, timeout, step) {
      setTimeout(() => {
        this.sounds[i].play();
        buttons[i].className = buttons[i].className.replace(
          /\bbutton\b/,
          "button:active"
        );
      }, timeout);
      setTimeout(() => {
        buttons[i].className = buttons[i].className.replace(
          /\bbutton:active\b/,
          "button"
        );
      }, timeout + step / 2);
    },
  }
};
</script>

<style scoped>
.simon {
  display: flex;
  width: 100%;
  justify-content: center;
  flex-wrap: wrap-reverse;
}

.simon__main {
  width: 30%;
  min-width: 350px;
  min-height: 300px;
  display: flex;
  flex-wrap: wrap;
}

.simon__button {
  width: 50%;
  height: 50%;
  min-height: 100px;
  max-width: 200px;
}

.simon__menu {
  display: flex;
  width: 200px;
  flex-direction: column;
}
</style>
