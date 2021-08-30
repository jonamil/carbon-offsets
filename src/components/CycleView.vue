<template>
  <section>
    <div class="canvas" :class="currentCyclePosition">
      <img id="preview" :class="currentClasses['preview']" src="@/assets/cycle.svg" />
    </div>
  </section>
</template>

<script>
export default {
  name: 'CycleView',

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
        'preview': {
          'visible': [2, 3, 4, 6, 7, 8, 9, 10, 11]
        }
      },

      currentCyclePosition: '',
      currentClasses: {}
    }
  },

  methods: {
    transition(direction = 'out') {
      for (const elementName in this.elementClasses) {
        if (!(elementName in this.currentClasses)) this.currentClasses[elementName] = [];

        for (const className in this.elementClasses[elementName]) {
          const classIndexes = this.elementClasses[elementName][className];

          if (direction === 'out') {
            if (!classIndexes.includes(this.currentPageIndex)) {
              this.currentClasses[elementName] = this.currentClasses[elementName].filter(item => item !== className);
            }
          } else if (direction === 'in') {
            if (classIndexes.includes(this.currentPageIndex) && !this.currentClasses[elementName].includes(className)) {
              this.currentClasses[elementName].push(className);
            }
          }
        }
      }
    }
  },

  watch: {
    currentPageIndex: function() {
      this.transition('out');

      setTimeout(() => {
        this.transition('in');
      }, this.transitionDuration / 2);
    },

    currentPageData: function(newPageData) {
      if (newPageData.type === 'explanation') this.currentCyclePosition = newPageData.cyclePosition;
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~@/assets/variables.scss';

section {
  .canvas {
    &.left {
      transform: translateX(calc(-50% - 160px)) $canvas-translate-y;
    }

    &.right {
      transform: translateX(calc(-50% + 160px)) $canvas-translate-y;
    }
  }

  *[id] {
    visibility: hidden;
    opacity: 0;
    transition: all $transition-duration ease-in-out;
  }

  *[id].visible {
    visibility: visible;
    opacity: 1;
  }

  #preview {
    display: block;
    margin: 0 auto;
  }
}
</style>