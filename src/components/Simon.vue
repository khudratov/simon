<template>
  <div class="simon">
    <div
      :class="[active === 1 ? 'click' : '', 'red']"
      @click="giveIndex(1)"
    ></div>
    <div
      :class="[active === 2 ? 'click' : '', 'blue']"
      @click="giveIndex(2)"
    ></div>
    <div
      :class="[active === 3 ? 'click' : '', 'yellow']"
      @click="giveIndex(3)"
    ></div>
    <div
      :class="[active === 4 ? 'click' : '', 'green']"
      @click="giveIndex(4)"
    ></div>
  </div>
</template>

<script>
export default {
  props: ["active"],
  methods: {
    giveIndex(index) {
      this.$emit("clicked", index);
    },
  },
};
</script>

<style scoped>
.simon {
  width: 330px;
  height: 330px;
  box-sizing: border-box;
  border-radius: 170px;
  background: white;
  box-shadow: 2px 1px 12px #aaa;
  display: flex;
  flex-wrap: wrap;
  border: 5px solid white;
}

.red,
.blue,
.yellow,
.green {
  height: 160px;
  width: 160px;
  cursor: pointer;
  opacity: 0.6;
  box-sizing: border-box;
  border: 1px solid white;
}

.red:hover,
.blue:hover,
.yellow:hover,
.green:hover {
  box-shadow: 1px 1px 4px 4px rgba(0, 0, 0, 0.164);
  opacity: 0.5;
}

.red {
  background: #ff5643;
  border-top-left-radius: 160px;
}

.blue {
  background: dodgerblue;
  border-top-right-radius: 160px;
}
.yellow {
  background: #feef33;
  border-bottom-left-radius: 160px;
}
.green {
  background: #bede15;
  border-bottom-right-radius: 160px;
}

.red:active,
.blue:active,
.yellow:active,
.green:active,
.click {
  opacity: 0.8;
}
</style>
