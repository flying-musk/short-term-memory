<template>
  <div class="t-outer">
    <div class="t-top">
      <button class="t-button" @click="endClick">End</button>
      <h2>Click on image to show number!</h2>
    </div>
    <div class="t-main">
      <button class="t-button" v-if="showLeft" @click="leftClick">
        &lt;&lt;
      </button>
      <img
        v-if="!cardOpen"
        class="t-image"
        :src="sheet[currentIdx].src"
        :alt="sheet[currentIdx].path"
        @click="toggleClick"
      />
      <div v-if="cardOpen" class="t-content" @click="toggleClick">
        {{ sheet[currentIdx].content }}
      </div>
      <button class="t-button" v-if="showRight" @click="rightClick">
        &gt;&gt;
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'test',
  props: ['sheet'],
  data() {
    return { currentIdx: 0, cardOpen: false };
  },
  computed: {
    showLeft: function () {
      return this.currentIdx > 0;
    },
    showRight: function () {
      return this.currentIdx < this.sheet.length - 1;
    },
  },
  methods: {
    endClick() {
      this.$emit('end');
    },
    leftClick() {
      this.cardOpen = false;
      this.currentIdx--;
    },
    rightClick() {
      this.cardOpen = false;
      this.currentIdx++;
    },
    toggleClick() {
      this.cardOpen = !this.cardOpen;
    },
  },
};
</script>

<style scoped lang="scss">
.t-outer {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.t-top {
  display: flex;
  gap: 16px;
  align-items: center;
}
.t-main {
  flex-grow: 1;
  display: flex;
  align-items: center;
  gap: 16px;
}
.t-image {
  cursor: pointer;
  flex-grow: 1;
  width: 100%;
  max-height: 64vh;
}
.t-content {
  cursor: pointer;
  flex-grow: 1;
  align-self: stretch;
  font-size: 64px;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
}
.t-button {
  padding: 12px;
  font-size: 24px;
}
</style>
