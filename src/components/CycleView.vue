<template>
  <section>
    <div class="canvas" :class="currentCyclePosition">
      <div id="arrow-emissions" :class="currentClasses['arrow-emissions']">
        <InlineSvg :src="pathToImage('arrow-emissions')" />
      </div>
      <div id="arrow-offsets" :class="currentClasses['arrow-offsets']">
        <InlineSvg :src="pathToImage('arrow-offsets')" />
      </div>
      <div id="arrow-loop" :class="currentClasses['arrow-loop']">
        <InlineSvg :src="pathToImage('arrow-loop')" />
      </div>
      <div id="arrow-market" :class="currentClasses['arrow-market']">
        <InlineSvg :src="pathToImage('arrow-market')" />
      </div>
      <div id="clouds" :class="currentClasses['clouds']">
        <InlineSvg :src="pathToImage('clouds')" />
        <span>Greenhouse Gases</span>
      </div>
      <div id="emissions" :class="currentClasses['emissions']">
        <InlineSvg :src="pathToImage('emissions')" />
        <span>Companies and people cause emissions</span>
      </div>
      <div id="offsets" :class="currentClasses['offsets']">
        <InlineSvg :src="pathToImage('offsets')" />
        <InlineSvg class="detailed" :src="pathToImage('offsets-detailed')" />
        <span>Offset projects prevent or reduce emissions</span>
      </div>
      <div id="market" :class="currentClasses['market']">
        <InlineSvg class="money" :src="pathToImage('market-money')" />
        <InlineSvg class="money more" :src="pathToImage('market-money-more')" />
        <InlineSvg class="certificates" :src="pathToImage('market-certificates')" />
        <InlineSvg class="certificates more" :src="pathToImage('market-certificates-more')" />
        <InlineSvg :src="pathToImage('market-text')" />
      </div>
    </div>
  </section>
</template>

<script>
import InlineSvg from 'vue-inline-svg';

export default {
  name: 'CycleView',

  components: {
    InlineSvg
  },

  props: {
    currentPageIndex: {
      type: Number,
      default: 0
    },
    currentPageData: {
      type: Object,
      default: () => {}
    },
    transitionDuration: {
      type: Number,
      default: 500
    }
  },

  data() {
    return {
      elementClasses: {
        'arrow-emissions': {
          'visible': true,
          'active': [1, 2, 5, 6, 11],
          'thickest-dash': [10, 11, 12],
          'thickest': [11, 12]
        },
        'arrow-offsets': {
          'visible': [3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
          'active': [2, 3],
          'thin': [8],
          'thinner': [9],
          'thinnest': [10, 11, 12]
        },
        'arrow-loop': {
          'visible': [8, 9, 10, 11, 12],
          'active': [7, 8, 9, 10],
          'thicker': [9],
          'thickest': [10, 11, 12]
        },
        'arrow-market': {
          'visible': [4, 5, 6, 7, 8, 9, 10, 11, 12],
          'active': [3, 4, 11, 12],
          'thickest': [11, 12]
        },
        'clouds': {
          'visible': true,
          'one': [1, 2, 8, 9, 10, 11, 12],
          'two': [1, 2, 9, 10, 11, 12],
          'three': [1, 2, 10, 11, 12],
          'four': [1, 2, 11, 12]
        },
        'emissions': {
          'visible': true
        },
        'offsets': {
          'visible': [3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
          'detailed': [7, 8, 9, 10, 11, 12]
        },
        'market': {
          'visible': [4, 5, 6, 7, 8, 9, 10, 11, 12],
          'more': [11, 12]
        }
      },

      currentCyclePosition: '',
      currentClasses: {}
    }
  },

  methods: {
    pathToImage(filename) {
      try {
        return require('@/assets/cycle/' + filename + '.svg');
      } catch (error) {
        return '';
      }
    },
    refreshClasses() {
      for (const elementName in this.elementClasses) {
        this.currentClasses[elementName] = [];

        for (const className in this.elementClasses[elementName]) {
          const classIndexes = this.elementClasses[elementName][className];

          if (classIndexes === true || classIndexes.includes(this.currentPageIndex)) {
            this.currentClasses[elementName].push(className);
          }
        }
      }
    }
  },

  watch: {
    currentPageIndex: function() {
      setTimeout(() => {
        this.refreshClasses();
      }, this.transitionDuration / 2);
    },

    currentPageData: function(newPageData) {
      if (newPageData.type === 'explanation') this.currentCyclePosition = newPageData.cyclePosition;
    }
  }
}
</script>

<style lang="scss">
@import '~@/assets/variables.scss';

section.cycle {
  .canvas {
    &.left {
      transform: translateX(calc(-50% - 16.0rem)) $canvas-translate-y;
    }

    &.right {
      transform: translateX(calc(-50% + 16.0rem)) $canvas-translate-y;
    }

    > [id] {
      position: absolute;
      visibility: hidden;
      opacity: 0;
      transition: visibility $transition-duration ease-in-out, opacity $transition-duration ease-in-out;

      &.visible {
        visibility: visible;
        opacity: 1;
      }

      span {
        position: absolute;
        display: block;
        left: 0;
        right: 0;
        text-align: center;
        font-weight: $weight-semibold;
      }
    }
  }

  [id^='arrow'], #clouds {
    path {
      transition: all $transition-duration ease-in-out;
    }
  }

  #preview {
    display: block;
    top: 50%;
    left: 50%;
    transform: translateX(-50%) translateY(-50%);
  }

  #arrow-emissions {
    top: 11.0rem;
    left: 34.1rem;

    path.dashed {
      animation: arrow-large 1s linear infinite reverse;
    }

    &.active path.dashed {
      stroke-opacity: 0.75;
    }

    &.thickest-dash path.dashed {
      stroke-dasharray: 10 10;
      animation-name: arrow-large-thickest;
    }

    &.thickest path.solid {
      stroke-width: 10;
    }
  }

  #arrow-offsets {
    top: 11.2rem;
    left: 65.9rem;

    path.dashed {
      animation: arrow-large 1s linear infinite reverse;
    }

    &.active path.dashed {
      stroke-opacity: 0.75;
    }

    &.thin path.solid {
      stroke-width: 6;
    }

    &.thinner path.solid {
      stroke-width: 4;
    }

    &.thinnest path.solid {
      stroke-width: 2;
    }
  }

  #arrow-loop {
    top: 1.3rem;
    left: 65.0rem;

    path.dashed {
      animation: arrow-loop 0.9s linear infinite reverse;
    }

    &.active path.dashed {
      stroke-opacity: 0.75;
    }

    &.thicker path.solid {
      stroke-width: 4;
    }

    &.thickest path.solid {
      stroke-width: 6;
    }
  }

  #arrow-market {
    top: 41.5rem;
    left: 43.0rem;

    path.dashed {
      animation: arrow-market 0.8s linear infinite reverse;
    }

    path:nth-of-type(5) {
      animation-direction: normal;
    }

    &.active path.dashed {
      stroke-opacity: 0.75;
    }

    &.thickest path.solid {
      stroke-width: 4;
    }
  }

  #clouds {
    top: 5.2rem;
    left: 44.3rem;

    span {
      top: 3.2rem;
      font-size: 1.6rem;
      font-weight: 750;
    }

    path.solid {
      fill-opacity: 0.08;
    }

    path.stroke {
      stroke-opacity: 0.23;
    }

    &.one path:nth-of-type(1), &.two path:nth-of-type(2), &.three path:nth-of-type(3), &.four path:nth-of-type(4) {
      fill-opacity: 0.25;
    }

    &.one path:nth-of-type(5), &.two path:nth-of-type(6), &.three path:nth-of-type(7), &.four path:nth-of-type(8) {
      stroke-opacity: 0;
    }
  }

  #emissions {
    top: 20.8rem;
    left: 23.5rem;

    span {
      top: 17.7rem;
    }
  }

  #offsets {
    top: 20.8rem;
    left: 56.3rem;

    svg {
      transition: visibility $transition-duration ease-in-out, opacity $transition-duration ease-in-out;
    }

    svg.detailed {
      position: absolute;
      visibility: hidden;
      opacity: 0;
      top: 1.5rem;
      left: 0;
    }

    span {
      top: 17.7rem;
      transition: visibility $transition-duration ease-in-out, opacity $transition-duration ease-in-out;
    }

    &.detailed {
      svg, span {
        visibility: hidden;
        opacity: 0;
      }

      svg.detailed {
        visibility: visible;
        opacity: 1;
      }
    }
  }

  #market {
    top: 42.1rem;
    left: 42.4rem;

    svg[class] {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      transition: visibility $transition-duration ease-in-out, opacity $transition-duration ease-in-out;
    }

    svg.money {
      top: -1.2rem;
    }

    svg.certificates {
      top: 6.6rem;
    }

    svg.more {
      visibility: hidden;
      opacity: 0;
    }

    &.more {
      svg.money {
        visibility: hidden;
        opacity: 0;
      }

      svg.more {
        visibility: visible;
        opacity: 1;
      }
    }

    textPath {
      font-size: 1.2rem;
      font-weight: $weight-semibold;
      letter-spacing: 0.025rem;
      fill: $color-main;
    }
  }
}

@keyframes arrow-large {
  0%   { stroke-dashoffset: 0 }
  100% { stroke-dashoffset: 16 }
}

@keyframes arrow-large-thickest {
  0%   { stroke-dashoffset: 0 }
  100% { stroke-dashoffset: 20 }
}

@keyframes arrow-loop {
  0%   { stroke-dashoffset: 0 }
  100% { stroke-dashoffset: 12 }
}

@keyframes arrow-market {
  0%   { stroke-dashoffset: 0 }
  100% { stroke-dashoffset: 8 }
}
</style>