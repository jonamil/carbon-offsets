<template>
  <section>
    <div class="canvas">
      <div
        class="explanation"
        :class="{
          left: page.textPosition === 'left' || page.cyclePosition === 'right',
          wide: 'cyclePosition' in page,
          top: page.textOffset === 'top',
          bottom: page.textOffset === 'bottom'
        }"
        :style="{
          transform: numericTextOffset ? 'translateY(calc(-50% + ' + numericTextOffset / 10 + 'rem))' : ''
        }"
      >
        <h2>{{ page.title }}</h2>
        <template
          v-for="(element, index) in page.elements"
          :key="index"
        >
          <TextParagraph
            v-if="element.type === 'paragraph' || element.type === 'quote'"
            :type="element.type"
            :content="element.content"
            :sources="sources"
          />
          <img
            v-else-if="element.type === 'image' && pathToImage(element.content)"
            :src="pathToImage(element.content)"
          />
        </template>
      </div>
      <div
        class="connector"
        :style="{
          width: page.connectorWidth ? page.connectorWidth / 10 + 'rem' : '',
          transform: page.connectorOffset ? 'translateY(calc(-0.1rem + ' + page.connectorOffset / 10 + 'rem))' : ''
        }"
      />
    </div>
  </section>
</template>

<script>
import TextParagraph from '@/components/TextParagraph.vue';

export default {
  name: 'ExplanationPage',

  components: {
    TextParagraph
  },

  props: {
    page: {
      type: Object,
      default: () => {}
    },
    sources: {
      type: Object,
      default: () => {}
    }
  },

  computed: {
    numericTextOffset() {
      if (typeof this.page.textOffset === 'number') return this.page.textOffset;
      else if (!('textOffset' in this.page) && this.page.connectorOffset) return this.page.connectorOffset;
      else return false;
    }
  },

  methods: {
    pathToImage(filename) {
      try {
        return require('@/assets/explanations/' + filename + '.svg');
      } catch (error) {
        return '';
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~@/assets/variables.scss';

section {
  .explanation {
    position: absolute;
    right: 0;
    top: 50%;
    width: 22.0rem;
    padding: 0.8rem 0 1.0rem 1.5rem;
    border-width: 0 0 0 0.2rem;
    border-color: rgba($color-main, 0.3);
    border-style: solid;
    transform: translateY(-50%);

    &.left {
      left: 0;
      right: unset;
      padding-left: 0;
      padding-right: 1.5rem;
      border-width: 0 0.2rem 0 0;
    }

    &.wide {
      width: 34.0rem;
      margin-right: 2.0rem;
    }

    &.left.wide {
      margin-left: 2.0rem;
      margin-right: 0;
    }

    &.top, &.bottom {
      transform: none;
    }

    &.top {
      top: 4.1rem;
    }

    &.bottom {
      top: unset;
      bottom: 3.0rem;
    }

    h2 {
      margin-bottom: -0.2rem;
      font-size: 1.8rem;
    }

    p {
      margin: 0.8rem 0 0;
      line-height: 1.3;
    }

    blockquote {
      position: relative;
      margin: 1.6rem 0;
      font-size: 1.4rem;
      font-weight: $weight-medium;
      line-height: 1.25;

      &:last-child {
        margin-bottom: 0;
      }

      &:before {
        position: absolute;
        display: block;
        content: '“';
        top: -2.8rem;
        left: -0.8rem;
        font-size: 11.6rem;
        font-style: normal;
        font-weight: $weight-black;
        font-variation-settings: 'opsz' 38;
        color: rgba($color-main, 0.12);
      }
    }

    img {
      display: block;
      width: 100%;
      max-height: 14.0rem;
      margin: 1.6rem 0;

      &:last-child {
        margin-bottom: 0;
      }
    }
  }

  .connector {
    position: absolute;
    top: 50%;
    right: 23.7rem;
    height: 0.2rem;
    background: rgba($color-main, 0.3);
    transform: translateY(-0.1rem);

    @at-root .explanation.left + .connector {
      left: 23.7rem;
      right: unset;
    }

    @at-root .explanation.wide + .connector {
      right: 37.7rem;
    }

    @at-root .explanation.left.wide + .connector {
      left: 37.7rem;
      right: unset;
    }
  }
}
</style>