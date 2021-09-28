<template>
  <component
    :is="type === 'quote' ? 'blockquote' : 'p'"
    v-html="parsedContent"
  />
</template>

<script>
export default {
  name: 'TextParagraph',

  props: {
    content: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'paragraph'
    },
    sources: {
      type: Object,
      default: () => {}
    }
  },

  computed: {
    parsedContent() {
      let content = this.content;

      content = content.replace(/\[(\d+)\]/gm, (match, number) => {
        let cite = '';

        if (number in this.sources) {
          cite = '<cite>' + this.sources[number][0].replace(/\*(.*)\*/gim, '<em>$1</em>') + '</cite>';
        }

        return '<sup>' + number + cite + '</sup>';
      });

      content = content.replace(/\*\*(.*)\*\*/gim, '<strong>$1</strong>');
      content = content.replace(/\*(.*)\*/gim, '<em>$1</em>');

      return content;
    }
  }
}
</script>

<style lang="scss">
@import '~@/assets/variables.scss';

p, blockquote {
  sup {
    position: relative;
    top: -0.3rem;
    margin-left: 0.05rem;
    padding: 0.1rem 0.3rem 0.1rem;
    font-size: 0.9rem;
    font-weight: $weight-medium;
    border-radius: 0.2rem;
    background: rgba($color-main, 0.15);
    cursor: pointer;
    transition: color $transition-duration ease-in-out, background $transition-duration ease-in-out;

    &:hover {
      color: #fff;
      background: $color-main;
    }

    + sup {
      margin-left: 0.2rem;
    }

    cite {
      position: absolute;
      display: block;
      visibility: hidden;
      opacity: 0;
      bottom: 100%;
      left: -9.0rem;
      right: -9.0rem;
      margin-bottom: 0.2rem;
      padding: 0.6rem 0.9rem 0.7rem;
      font-size: 1.0rem;
      font-weight: $weight-regular;
      font-style: normal;
      line-height: 1.25;
      border-radius: 0.2rem;
      background: $color-main;
      color: #fff;
      cursor: e-resize;
      transition: visibility $transition-duration ease-in-out, opacity $transition-duration ease-in-out;

      @at-root sup:hover cite {
        visibility: visible;
        opacity: 1;
      }

      &:before {
        position: absolute;
        display: block;
        content: '';
        top: 100%;
        bottom: -0.2rem;
        left: 0;
        right: 0;
      }
    }
  }
}
</style>