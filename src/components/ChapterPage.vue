<template>
  <section>
    <h1>{{ page.title }}</h1>
  </section>
</template>

<script>
export default {
  name: 'ChapterPage',

  props: {
    page: {
      type: Object,
      default: () => {}
    },
    sources: {
      type: Object,
      default: () => {}
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