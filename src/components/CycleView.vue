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
          'visible': true
        }
      },

      currentCyclePosition: '',
      currentClasses: {}
    }
  },

  methods: {
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

<style lang="scss" scoped>
@import '~@/assets/variables.scss';

section {
  .canvas {
    &.left {
      transform: translateX(calc(-50% - 16.0rem)) $canvas-translate-y;
    }

    &.right {
      transform: translateX(calc(-50% + 16.0rem)) $canvas-translate-y;
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
    width: 61.4rem;
  }
}
</style>