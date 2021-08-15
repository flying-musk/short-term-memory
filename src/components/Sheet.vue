<template>
  <div class="s-grid">
    <div v-if="error" class="s-error">
      Make sure to fill in content for all checked cards!
    </div>
    <button @click="start">START</button>
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
    <button @click="start">START</button>
    <div v-if="error" class="s-error">
      Make sure to fill in content for all checked cards!
    </div>
  </div>
</template>

<script>
export default {
  name: 'sheet',
  data() {
    return {
      error: false,
      sheet: [...Array(26).keys()].map((idx) => ({
        idx: idx,
        path: `../assets/images/${idx + 1}.png`,
        src: require(`../assets/images/${idx + 1}.png`),
        checked: false,
        content: '',
      })),
    };
  },
  methods: {
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
      if (checkedCards.filter((c) => c.content === '').length > 0) {
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
.s-error {
  border: solid 2px #cc0000;
  color: #cc0000;
  font-size: 20px;
  text-align: center;
  display: flex;
  align-items: center;
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
