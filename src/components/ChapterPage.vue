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

  div {
    position: absolute;
    top: 50%;
    left: 0;
    right: 0;
    text-align: center;
    transform: $canvas-translate-y;

    h3 {
      margin-bottom: 6px;
      font-size: 16px;
      font-weight: $weight-semibold;
      letter-spacing: 1px;
      text-transform: uppercase;
      color: $color-lighter;
    }

    h1 {
      font-size: 40px;
    }

    p, blockquote {
      width: 340px;
      margin: 22px auto 0;
      text-align: left;
      font-size: 14px;
      line-height: 1.3;
    }

    p {
      &:first-of-type {
        margin-top: 68px;
      }

      &:deep(sup) {
        top: -4px;
      }
    }
  }

  &.the-offsetting-cycle {
    background-image: url('~@/assets/chapters/chapter-1.svg');

    p:nth-of-type(1) {
      transform: translateX(-62px);
    }

    p:nth-of-type(2) {
      transform: translateX(62px);
    }
  }

  &.a-balanced-equation {
    background-image: url('~@/assets/chapters/chapter-2.svg');

    p {
      width: 416px;
    }

    blockquote {
      width: 384px;
      text-align: center;
      font-size: 18px;
      font-weight: $weight-black;
      font-style: normal;
      line-height: 1.2;
    }
  }
}
</style>