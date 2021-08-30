<template>
  <section :class="$props.class">
    <h4>A Project by Luka Saje<br>and Jona Pomerance</h4>
    <div class="title">
      <h1>Carbon Neutrality:<br>A Label For Sale?</h1>
      <h3>An introduction to carbon offsets:<br>how they work and where they fail</h3>
    </div>
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
@import '~@/assets/variables.scss';

section {
  h4 {
    position: absolute;
    top: 13px;
    right: 52px;
    text-align: right;
    font-weight: $weight-semibold;
    line-height: 1.2;
    color: rgba($color-main, 0.35);
  }

  .title {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 600px;
    padding-bottom: 30px;
    transform: translateX(-50%) translateY(-50%);

    h1 {
      text-align: center;
      font-size: 48px;
      line-height: 1.1;
    }

    h3 {
      margin-top: 46px;
      text-align: center;
      font-size: 22px;
      font-weight: $weight-medium;
      line-height: 1.2;
    }
  }
}
</style>