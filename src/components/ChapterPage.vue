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
  background-position: center $canvas-offset-y;
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
    }

    p {
      &:first-of-type {
        margin-top: 6.8rem;
      }

      &:deep(sup) {
        top: -0.4rem;
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
      width: 41.6rem;
    }

    blockquote {
      width: 38.4rem;
      text-align: center;
      font-size: 1.8rem;
      font-weight: $weight-black;
      font-style: normal;
      line-height: 1.2;
    }
  }
}
</style>