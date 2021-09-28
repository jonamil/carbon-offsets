<template>
  <section :class="slugifiedTitle">
    <div>
      <h3>{{ page.preTitle }}</h3>
      <h1>{{ page.title }}</h1>
      <TextParagraph
        v-for="(element, index) in page.elements"
        :key="index"
        :type="element.type"
        :content="element.content"
        :sources="sources"
      />
    </div>
  </section>
</template>

<script>
import TextParagraph from '@/components/TextParagraph.vue';

export default {
  name: 'ChapterPage',

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
    slugifiedTitle() {
      return this.page.title.toLowerCase().trim().replace(/[^a-z0-9 ]/g, '').replace(/\s+/g, '-');
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~@/assets/variables.scss';

section {
  background-position: center;
  background-repeat: no-repeat;
  background-size: 260.0rem;

  div {
    position: absolute;
    top: 50%;
    left: 0;
    right: 0;
    text-align: center;
    transform: $canvas-translate-y;

    h3 {
      margin-bottom: 0.6rem;
      font-size: 1.6rem;
      font-weight: $weight-semibold;
      letter-spacing: 0.1rem;
      text-transform: uppercase;
      color: $color-lighter;
    }

    h1 {
      font-size: 4.0rem;
    }

    p, blockquote {
      width: 34.0rem;
      margin: 2.2rem auto 0;
      text-align: left;
      font-size: 1.4rem;
      line-height: 1.3;

      &:deep(sup) {
        top: -0.4rem;
      }
    }

    p {
      &:first-of-type {
        margin-top: 6.8rem;
      }
    }
  }

  &.the-offsetting-cycle {
    background-image: url('~@/assets/chapters/chapter-1.svg');

    p:nth-of-type(1) {
      transform: translateX(-6.2rem);
    }

    p:nth-of-type(2) {
      transform: translateX(6.2rem);
    }
  }

  &.a-balanced-equation {
    background-image: url('~@/assets/chapters/chapter-2.svg');

    p {
      width: 41.2rem;
    }

    blockquote {
      width: 38.0rem;
      text-align: center;
      font-size: 1.8rem;
      font-weight: $weight-black;
      font-style: normal;
      line-height: 1.2;
    }
  }

  &.an-ineffective-system {
    background-image: url('~@/assets/chapters/chapter-3.svg');

    div {
      width: 83.6rem;
      margin: 0 auto;
      columns: 2;
      column-gap: 6.0rem;
    }

    h3, h1 {
      column-span: all;
    }

    p, blockquote {
      width: 100%;
      margin-top: 1.7rem;
      break-inside: avoid;
    }

    p:nth-of-type(3) {
      margin-top: 0;
      padding-top: 11.4rem;
      break-before: always;
    }

    blockquote {
      position: relative;
      font-size: 1.55rem;
      font-weight: $weight-medium;
      line-height: 1.25;

      &:before {
        position: absolute;
        display: block;
        content: '“';
        top: -2.5rem;
        left: -1.0rem;
        font-size: 11.8rem;
        font-style: normal;
        font-weight: $weight-black;
        font-variation-settings: 'opsz' 38;
        color: rgba($color-main, 0.12);
      }

      &:deep(sup):first-of-type {
        margin-left: 0.2rem;
      }
    }
  }
}
</style>