<template>
  <div class="s-grid">
    <div class="s-random">
      <h3>Select randomly by computer</h3>
      <p>Cards amount:</p>
      <select v-model="randomAmount">
        <option disabled value="">Cards amount</option>
        <option v-for="o in options" :key="o" :value="o + 1">
          {{ o + 1 }}
        </option>
      </select>
      <button @click="randomClick">START</button>
      <p v-if="randomError" class="random-error">Please select cards amount!</p>
    </div>
    <div v-if="error" class="s-error">
      Make sure to fill in at least one card!
    </div>
    <button class="s-button" @click="start">START</button>
    <div v-for="s in sheet" :key="s.idx" class="s-card">
      <div>
        <input type="checkbox" :data-idx="s.idx" v-model="s.checked" />
        <input
          type="text"
          :data-idx="s.idx"
          v-model="s.content"
          @focus="contentFocus"
          @blur="contentBlur"
        />
      </div>
      <img class="card-image" :src="s.src" :alt="s.path" />
    </div>
    <button class="s-button" @click="start">START</button>
    <div v-if="error" class="s-error">
      Make sure to fill in at least one card!
    </div>
  </div>
</template>

<script>
export default {
  name: 'sheet',
  data() {
    return {
      options: [...Array(41).keys()],
      randomAmount: 0,
      randomError: false,
      error: false,
      sheet: [...Array(41).keys()].map((idx) => ({
        idx: idx,
        path: `../assets/images/${idx + 1}.png`,
        src: require(`../assets/images/${idx + 1}.png`),
        checked: false,
        content: '',
      })),
    };
  },
  methods: {
    randomClick() {
      if (this.randomAmount === 0) {
        this.randomError = true;
      } else {
        this.randomError = false;
        let randomArray = [...Array(100).keys()].sort(
          () => Math.random() - 0.5
        );
        let randomCards = JSON.parse(JSON.stringify(this.sheet))
          .sort(() => Math.random() - 0.5)
          .slice(0, this.randomAmount);
        this.$emit(
          'sheetfinish',
          randomCards.map((c, i) => ({
            idx: c.idx,
            content: randomArray[i],
            src: c.src,
            path: c.path,
          }))
        );
      }
    },
    contentFocus(e) {
      let idx = parseInt(e.currentTarget.dataset.idx);
      this.sheet[idx].checked = true;
    },
    contentBlur(e) {
      let idx = parseInt(e.currentTarget.dataset.idx);
      if (this.sheet[idx].content === '') {
        this.sheet[idx].checked = false;
      }
    },
    start() {
      let checkedCards = this.sheet.filter((s) => s.checked);
      if (checkedCards.length === 0) {
        this.error = true;
      } else {
        this.$emit(
          'sheetfinish',
          checkedCards.map((c) => ({
            idx: c.idx,
            content: c.content,
            src: c.src,
            path: c.path,
          }))
        );
      }
    },
  },
};
</script>

<style scoped lang="scss">
.s-grid {
  flex-grow: 1;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  column-gap: 16px;
  row-gap: 16px;
}
.s-card {
  display: flex;
  flex-direction: column;
}
.card-image {
  width: 100%;
}
.s-button {
  padding: 12px;
  font-size: 24px;
  justify-self: center;
  align-self: center;
}
.s-error {
  justify-self: center;
  align-self: center;
  border: solid 2px #cc0000;
  padding: 12px;
  color: #cc0000;
  font-size: 20px;
  text-align: center;
  display: flex;
  align-items: center;
}
.s-random {
  border: 1px solid #e6e6e6;
  border-radius: 4px;
}
.random-error {
  color: #cc0000;
}
@media (max-width: 1000px) {
  .s-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
@media (max-width: 700px) {
  .s-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
