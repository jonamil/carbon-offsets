<template>
  <section>
    <div class="canvas">
      <div id="preview" />
    </div>
  </section>
</template>

<script>
export default {
  name: 'Cycle',

  props: {
    currentPageIndex: {
      type: Number,
      default: 0
    },
    transitionDuration: {
      type: Number,
      default: 500
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';

section {
  #preview {
    width: 614px;
    height: 100%;
    margin: 0 auto;
    background: rgba(255,0,0,0.1);
  }
}
</style>