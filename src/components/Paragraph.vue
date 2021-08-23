<template>
  <component
    :is="type === 'quote' ? 'blockquote' : 'p'"
    v-html="parsedContent"
  />
</template>

<script>
export default {
  name: 'Paragraph',

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

      content = content.replace(/\*\*(.*)\*\*/gim, '<strong>$1</strong>')
      content = content.replace(/\*(.*)\*/gim, '<em>$1</em>');

      return content;
    }
  }
}
</script>

<style lang="scss">
@import '../assets/variables.scss';

p, blockquote {
  sup {
    position: relative;
    top: -3px;
    margin-left: 0.5px;
    padding: 1px 3px 1px;
    font-size: 9px;
    font-weight: $weight-medium;
    border-radius: 2px;
    background: rgba($color-main, 0.15);
    cursor: pointer;
    transition: all $transition-duration ease-in-out;

    &:hover {
      color: #fff;
      background: $color-main;
    }

    + sup {
      margin-left: 2px;
    }

    cite {
      position: absolute;
      display: block;
      visibility: hidden;
      opacity: 0;
      bottom: 100%;
      left: -90px;
      right: -90px;
      margin-bottom: 2px;
      padding: 6px 9px 7px;
      font-size: 10px;
      font-weight: $weight-regular;
      font-style: normal;
      line-height: 1.25;
      border-radius: 2px;
      background: $color-main;
      color: #fff;
      cursor: e-resize;
      transition: all $transition-duration ease-in-out;

      @at-root sup:hover cite {
        visibility: visible;
        opacity: 1;
      }

      &:before {
        position: absolute;
        display: block;
        content: '';
        top: 100%;
        bottom: -2px;
        left: 0;
        right: 0;
      }
    }
  }
}
</style>