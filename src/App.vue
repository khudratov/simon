<template>
  <div id="app">
    <h2 class="game-title">Simon Says</h2>
    <div class="flex-row">
      <div class="flex--item">
        <span class="helper-text">{{ helper }} </span>
        <Simon @clicked="userTurn" :active="active" />
      </div>
      <div class="flex--item">
        <div class="round">Round {{ round }}</div>

        <button class="btn-start" @click="startSing()" :disabled="button">
          Старт
        </button>

        <GameMode @changed="changeDifficulty" :button="button" />
      </div>
    </div>
  </div>
</template>

<script>
import Simon from "./components/Simon";
import GameMode from "./components/GameMode";

import one from "./assets/sounds/1.mp3";
import two from "./assets/sounds/2.mp3";
import three from "./assets/sounds/3.mp3";
import four from "./assets/sounds/4.mp3";

export default {
  name: "App",
  components: { Simon, GameMode },
  data() {
    return {
      ready: false,
      soundPlay: true,
      active: 0,
      button: false,
      user: false,
      round: 1,
      history: [],
      current: 0,
      difficulty: 1500,
      helper: "Нажмите старт!",
      sounds: [],
    };
  },
  mounted() {
    let arr = [one, two, three, four];
    for (let i = 0; i < 4; i++) {
      this.sounds.push(new Audio(arr[i]));
    }
  },

  methods: {
    startSing() {
      this.helper = "Слушайте ";
      this.button = true;
      const random = Math.floor(Math.random() * 4) + 1;
      this.history.push(random);
      this.current = 0;
      let idInterval = setInterval(() => {
        let current = this.current;
        if (current > this.history.length - 1) {
          this.current = 0;
          this.user = true;
          this.helper = "Повторяете";
          clearInterval(idInterval);
        } else {
          this.playSound(this.history[current]);
          this.current++;
        }
      }, this.difficulty);
    },

    userTurn(index) {
      if (this.user) {
        this.playSound(index);
        if (index == this.history[this.current]) {
          if (this.current != this.history.length - 1) {
            this.current++;
          } else {
            this.round++;
            this.user = false;
            setTimeout(() => this.startSing(), 500);
          }
        } else {
          //when Lose
          this.whenLouse();
        }
      }
    },

    whenLouse() {
      setTimeout(() => {
        alert(`Вы проиграли в раунде ${this.round}`);
      }, 200);
      this.helper = "Нажмите старт!";
      this.current = 0;
      this.user = false;
      this.button = false;
      this.history = [];
      this.round = 1;
    },

    light(value) {
      this.active = value;
      setTimeout(() => {
        this.active = null;
      }, 200);
    },

    playSound(index) {
      this.light(index);
      let clone = this.sounds[index - 1].cloneNode(true);
      clone.play();
    },

    changeDifficulty(value) {
      this.difficulty = value;
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

.game-title {
  font-size: 34px;
}

.helper-text {
  display: block;
  margin-bottom: 15px;
}

.flex-row {
  display: flex;
  justify-content: center;
  align-items: center;
}

.flex--item {
  margin: 10px 30px;
}

.btn-start {
  background: #6dabe8;
  box-sizing: border-box;
  width: 5em;
  border: none;
  padding: 0.3em 0.6em;
  font-size: 1.4em;
  -webkit-border-radius: 10px 10px 10px 10px;
  border-radius: 10px 10px 10px 10px;
  margin: 20px 0;
}

.round {
  font-size: 1.4em;
  font-weight: bold;
}
</style>
