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
          transform: numericTextOffset ? 'translateY(calc(-50% + ' + numericTextOffset + 'px))' : ''
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
          width: page.connectorWidth ? page.connectorWidth + 'px' : '',
          transform: page.connectorOffset ? 'translateY(calc(-1px + ' + page.connectorOffset + 'px))' : ''
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
    width: 220px;
    padding: 8px 0 10px 15px;
    border-width: 0 0 0 2px;
    border-color: rgba($color-main, 0.3);
    border-style: solid;
    transform: translateY(-50%);

    &.left {
      left: 0;
      right: unset;
      padding-left: 0;
      padding-right: 15px;
      border-width: 0 2px 0 0;
    }

    &.wide {
      width: 340px;
      margin-right: 20px;
    }

    &.left.wide {
      margin-left: 20px;
      margin-right: 0;
    }

    &.top, &.bottom {
      transform: none;
    }

    &.top {
      top: 41px;
    }

    &.bottom {
      top: unset;
      bottom: 30px;
    }

    h2 {
      margin-bottom: -2px;
      font-size: 18px;
    }

    p {
      margin: 8px 0 0;
      line-height: 1.3;
    }

    blockquote {
      position: relative;
      margin: 16px 0;
      font-size: 14px;
      font-weight: $weight-medium;
      line-height: 1.25;

      &:last-child {
        margin-bottom: 0;
      }

      &:before {
        position: absolute;
        display: block;
        content: '“';
        top: -28px;
        left: -8px;
        font-size: 116px;
        font-style: normal;
        font-weight: $weight-black;
        font-variation-settings: 'opsz' 38;
        color: rgba($color-main, 0.12);
      }
    }

    img {
      display: block;
      width: 100%;
      max-height: 140px;
      margin: 16px 0;

      &:last-child {
        margin-bottom: 0;
      }
    }
  }

  .connector {
    position: absolute;
    top: 50%;
    right: 237px;
    height: 2px;
    background: rgba($color-main, 0.3);
    transform: translateY(-1px);

    @at-root .explanation.left + .connector {
      left: 237px;
      right: unset;
    }

    @at-root .explanation.wide + .connector {
      right: 377px;
    }

    @at-root .explanation.left.wide + .connector {
      left: 377px;
      right: unset;
    }
  }
}
</style>