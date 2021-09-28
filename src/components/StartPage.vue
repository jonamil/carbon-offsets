<template>
  <section :class="$props.class">
    <h4>A Project by Luka Saje<br>and Jona Pomerance</h4>
    <div class="title">
      <h1>Carbon Neutrality:<br>A Label For Sale?</h1>
      <h3>An introduction to carbon offsets:<br>how they work and where they fail</h3>
    </div>
    <div class="prompt">
      {{ touchInteractions ? 'Swipe' : 'Scroll' }} to begin
    </div>
    <InlineSvg :src="pathToImage('corner-left')" />
    <InlineSvg :src="pathToImage('corner-right')" />
  </section>
</template>

<script>
import InlineSvg from 'vue-inline-svg';

export default {
  name: 'StartPage',
  inheritAttrs: false,

  components: {
    InlineSvg
  },

  data() {
    return {
      touchInteractions: false
    }
  },

  props: {
    class: {
      type: String,
      default: ''
    }
  },

  methods: {
    pathToImage(filename) {
      try {
        return require('@/assets/start/' + filename + '.svg');
      } catch (error) {
        return '';
      }
    }
  },

  mounted() {
    if (('ontouchstart' in window) || (navigator.maxTouchPoints > 0) || (navigator.msMaxTouchPoints > 0)) {
      this.touchInteractions = true;
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~@/assets/variables.scss';

section {
  h4 {
    position: absolute;
    top: 1.3rem;
    right: 5.2rem;
    text-align: right;
    font-weight: $weight-semibold;
    line-height: 1.2;
    color: rgba($color-main, 0.35);
  }

  .title {
    position: absolute;
    top: 38%;
    left: 50%;
    width: 60.0rem;
    padding-top: 9.9rem;
    background: url('~@/assets/start/clouds.svg') top center no-repeat;
    background-size: 48.8rem;
    transform: translateX(-50%) translateY(-50%);

    h1 {
      text-align: center;
      font-size: 4.8rem;
      line-height: 1.1;
    }

    h3 {
      margin-top: 4.6rem;
      text-align: center;
      font-size: 2.2rem;
      font-weight: $weight-medium;
      line-height: 1.2;
    }
  }

  .prompt {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    padding: 1.6rem 0 5.0rem;
    text-align: center;
    font-size: 1.6rem;
    font-weight: $weight-semibold;
    letter-spacing: 0.1rem;
    text-transform: uppercase;
    color: $color-lighter;
    cursor: pointer;

    &:after {
      position: absolute;
      display: block;
      content: '';
      bottom: 1.3rem;
      left: 0;
      right: 0;
      height: 3.2rem;
      opacity: 0.25;
      background: url('~@/assets/icons/nav-down.svg') center no-repeat;
      background-size: 3.2rem;
      animation: arrow-down 0.8s ease-in-out infinite alternate;
    }
  }

  svg {
    position: absolute;
    bottom: 0;

    &:first-of-type {
      &:deep(path.smoke) {
        animation-timing-function: linear;
        animation-iteration-count: infinite;
        animation-direction: reverse;

        &.home { // 277.5
          stroke-dasharray: 377.5 5.25;
          animation-name: smoke-home;
          animation-duration: 7.655s;
          animation-delay: 2s;
        }

        &.highrise-front { // 298.5
          stroke-dasharray: 398.5 5.25;
          animation-name: smoke-highrise-front;
          animation-duration: 8.075s;
          animation-delay: 4.5s;
        }

        &.highrise-back { // 616.5
          stroke-dasharray: 716.5 5.25;
          animation-name: smoke-highrise-back;
          animation-duration: 14.435s;
          animation-delay: 0.5s;
        }

        &.plane { // 198
          stroke-dasharray: 298 5.25;
          animation-name: smoke-plane;
          animation-duration: 6.065s;
          animation-delay: 3s;
        }
      }
    }

    &:last-of-type {
      right: 0;

      &:deep(g) {
        transform-origin: center;
        transform-box: fill-box;
        animation: rotor 10s linear infinite;
      }
    }
  }
}

@keyframes arrow-down {
  0%   { transform: translateY(-0.2rem) }
  100% { transform: translateY(0.2rem) }
}

@keyframes smoke-home {
  0%   { stroke-dashoffset: 0 }
  100% { stroke-dashoffset: 382.75 }
}

@keyframes smoke-highrise-front {
  0%   { stroke-dashoffset: 0 }
  100% { stroke-dashoffset: 403.75 }
}

@keyframes smoke-highrise-back {
  0%   { stroke-dashoffset: 0 }
  100% { stroke-dashoffset: 721.75 }
}

@keyframes smoke-plane {
  0%   { stroke-dashoffset: 0 }
  100% { stroke-dashoffset: 303.25 }
}

@keyframes rotor {
  0%   { transform: rotate(0deg) }
  100% { transform: rotate(360deg) }
}
</style>