<template>
  <section :class="$props.class">
    <h1>Start</h1>
  </section>
</template>

<script>
export default {
  name: 'StartPage',
  inheritAttrs: false,

  props: {
    class: {
      type: String,
      default: ''
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';

section {
  h1 {
    position: relative;
    margin-top: 92.5vh;
    text-align: center;
    font-size: 32px;
    color: $color-lighter;
  }
}
</style>