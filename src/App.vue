<template>
  <div id="app">
    <h2>Simon Says</h2>

    <div class="flex-row">
      <div class="flex--item">
        <Simon @clicked="userTurn" :active="active" />
      </div>
      <div class="flex--item">
        <div class="round">Round {{ round }}</div>
        <br />
        <br />
        <button class="btn-start" @click="startSing()" :disabled="gameStarted">
          Старт
        </button>
        <br />
        <br />
        <div class="game-mode">
          <div class="game-mode--text">Уровн сложности:</div>
          <input type="radio" name="game-mode" id="mode-ease" checked />
          <label for="mode-ease">Легкий</label>
          <br />
          <input type="radio" name="game-mode" id="mode-normal" />
          <label for="mode-normal">Средний</label>
          <br />
          <input type="radio" name="game-mode" id="mode-hard" />
          <label for="mode-hard">Сложный</label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Simon from "./components/Simon";

import one from "./assets/sounds/1.mp3";
import two from "./assets/sounds/2.mp3";
import three from "./assets/sounds/3.mp3";
import four from "./assets/sounds/4.mp3";

export default {
  name: "App",
  components: { Simon },

  data() {
    return {
      active: 0,
      gameStarted: false,
      user: false,
      round: 0,
      history: [],
      current: 0,
    };
  },
  methods: {
    startSing() {
      this.gameStarted = true;
      const random = Math.floor(Math.random() * 4) + 1;
      this.history.push(random);
      this.current = 0;
      let idInterval = setInterval(() => {
        let current = this.current;
        if (current > this.history.length - 1) {
          this.current = 0;
          this.user = true;
          clearInterval(idInterval);
        } else {
          this.playSound(this.history[current]);
          this.setActive(this.history[current]);
          this.current++;
        }
      }, 500);
    },

    userTurn(index) {
      if (this.user) {
        this.playSound(index);
        if (index == this.history[this.current]) {
          if (this.current != this.history.length - 1) {
            console.log("right");
            this.current++;
          } else {
            this.round++;
            setTimeout(() => this.startSing(), 500);
          }
        } else {
          ///when Lose\
          this.whenLouse();
        }
      }
    },

    whenLouse() {
      alert(`You won ${this.round}`);

      this.current = 0;
      this.user = false;
      this.gameStarted = false;
      this.history = [];
      this.round = 0;
    },

    setActive(value) {
      this.active = value;
      setTimeout(() => {
        this.active = null;
      }, 200);
    },

    playSound(index) {
      let src = null;
      switch (index) {
        case 1:
          src = one;
          break;
        case 2:
          src = two;
          break;
        case 3:
          src = three;
          break;
        case 4:
          src = four;
          break;
      }

      let audio = new Audio(src).play();
      if (audio !== undefined) {
        audio
          .then(function() {})
          .catch(function(error) {
            console.error("error:", error);
          });
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.flex-row {
  display: flex;
  justify-content: center;
  align-items: center;
}

.flex--item {
  margin: 10px;
}
</style>
