<template>
  <div class="s-outer">
    <h1>Short Term Memory</h1>
    <Form />
    <Test />
  </div>
</template>

<script>
import Form from './Form.vue';
import Test from './Test.vue';
export default {
  name: 'short-term-memory',
  components: {
    Form,
    Test,
  },
};
</script>

<style scoped lang="scss">
.s-outer {
  height: 98vh;
  display: flex;
  flex-direction: column;
}
</style>
