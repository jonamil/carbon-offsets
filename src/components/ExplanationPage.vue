<template>
  <section>
    <div class="canvas">
      <div class="explanation" :class="{ left: page.textPosition === 'left' || page.cyclePosition === 'right', wide: 'cyclePosition' in page }">
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
        </template>
      </div>
    </div>
  </section>
</template>

<script>
import TextParagraph from './TextParagraph.vue';

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
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';

section {
  .explanation {
    position: absolute;
    right: 0;
    top: 50%;
    // bottom: -13px;
    width: 220px;
    padding: 8px 0 10px 15px;
    border-width: 0 0 0 2px;
    border-color: $color-lighter;
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
        margin-bottom: 0;;
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
  }
}
</style>