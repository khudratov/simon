<template>
  <div class="game-mode">
    <div class="game-mode--text">Уровн сложности:</div>
    <div class="game-mode--input">
      <input
        type="radio"
        name="game-mode"
        id="mode-ease"
        checked
        @change="changed(1500)"
        :disabled="button"
      />
      <label for="mode-ease">Легкий</label>
    </div>
    <div class="game-mode--input">
      <input
        type="radio"
        name="game-mode"
        id="mode-normal"
        @change="changed(1000)"
        :disabled="button"
      />
      <label for="mode-normal">Средний</label>
    </div>

    <div class="game-mode--input">
      <input
        type="radio"
        name="game-mode"
        id="mode-hard"
        @change="changed(400)"
        :disabled="button"
      />
      <label for="mode-hard">Сложный</label>
    </div>
  </div>
</template>

<script>
export default {
  props: ["button"],
  methods: {
    changed(value) {
      this.$emit("changed", value);
    },
  },
};
</script>

<style>
.game-mode {
  display: flex;
  flex-direction: column;
}

.game-mode--input {
  padding: 5px 0;
  text-align: left;
}
</style>
