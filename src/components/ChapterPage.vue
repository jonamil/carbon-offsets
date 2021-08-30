<template>
  <section>
    <div>
      <h3>{{ page.preTitle }}</h3>
      <h1>{{ page.title }}</h1>
    </div>
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
@import '~@/assets/variables.scss';

section {
  div {
    position: absolute;
    top: 50%;
    left: 0;
    right: 0;
    padding-bottom: 20px;
    text-align: center;
    transform: translateY(-50%);

    h3 {
      margin-bottom: 6px;
      font-size: 16px;
      font-weight: $weight-semibold;
      letter-spacing: 1px;
      text-transform: uppercase;
      color: $color-lighter;
    }

    h1 {
      font-size: 40px;
    }
  }
}
</style>