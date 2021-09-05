<template>
  <main>
    <CycleView
      :currentPageIndex="currentPageIndex"
      :currentPageData="currentPageData"
      :transitionDuration="transitionDuration"
      class="cycle"
    />
    <component
      v-for="(page, index) in pages"
      :key="index"
      :is="componentNameForPageType(page.type)"
      :page="page"
      :sources="sources"
      :class="{ visible: currentVisiblePageIndex === index }"
    />
  </main>
  <nav :class="{ visible: currentPageIndex > 0 }">
    <button @click="setPageIndex(currentPageIndex - 1)" />
    <ul>
      <li
        v-for="(page, index) in pages"
        :key="index"
        :class="[pageStatusForIndex(index), page.type]"
        :data-title="page.title"
        @click="setPageIndex(index)"
      />
    </ul>
    <button @click="setPageIndex(currentPageIndex + 1)" />
  </nav>
  <aside :class="{ open: infoDrawerOpen }" @click.self="closeInfoDrawer()">
    <CreditsText ref="credits" :sources="sources" />
    <button @click="toggleInfoDrawer()" />
  </aside>
</template>

<script>
import StartPage from '@/components/StartPage.vue';
import ChapterPage from '@/components/ChapterPage.vue';
import ExplanationPage from '@/components/ExplanationPage.vue';
import CycleView from '@/components/CycleView.vue';
import CreditsText from '@/components/CreditsText.vue';

import pages from '@/data/pages.json';
import sources from '@/data/sources.json';

export default {
  name: 'App',

  components: {
    StartPage,
    ChapterPage,
    ExplanationPage,
    CycleView,
    CreditsText
  },

  data() {
    return {
      pages,
      sources,

      currentPageIndex: 0,
      currentVisiblePageIndex: 0,
      infoDrawerOpen: false,

      transitionDuration: 400,
      transitionInProgress: false,
      
      lastScrollDelta: 0,
      lastScrollTimestamp: 0,

      referencesOffset: 0
    }
  },

  computed: {
    currentPageData() {
      return this.pages[Math.max(Math.min(this.currentPageIndex, this.pages.length - 1), 0)];
    }
  },

  methods: {
    pageStatusForIndex(index) {
      if (index < this.currentPageIndex) return 'past';
      else if (index === this.currentPageIndex) return 'active';
      else return 'future';
    },
    componentNameForPageType(type) {
      const pageTypes = {
        'start': 'StartPage',
        'chapter': 'ChapterPage',
        'explanation': 'ExplanationPage'
      };
      return (type in pageTypes) ? pageTypes[type] : 'span';
    },
    setPageIndex(index) {
      if (!this.transitionInProgress && index !== this.currentPageIndex && index >= 0) {
        if (index <= this.pages.length - 1) {
          this.currentPageIndex = index;
          this.startTransition();
        } else if (index === this.pages.length) {
          this.openInfoDrawer(true);
        }
      }
    },
    openInfoDrawer(transition = false) {
      if (!this.transitionInProgress) {
        this.infoDrawerOpen = true;
        if (transition) this.startTransition();
      }
    },
    closeInfoDrawer(transition = false) {
      if (!this.transitionInProgress) {
        this.infoDrawerOpen = false;
        if (transition) this.startTransition();
      }
    },
    toggleInfoDrawer() {
      this.infoDrawerOpen = !this.infoDrawerOpen;
    },
    openReferences() {
      this.$refs.credits.$el.scrollTop = this.referencesOffset;
      this.openInfoDrawer();
    },
    startTransition() {
      this.transitionInProgress = true;
      setTimeout(() => {
        this.transitionInProgress = false;
      }, this.transitionDuration);
    },
    handleWheelEvent(event) {
      if (!event.ctrlKey && !event.target.matches('aside *')) {
        if (event.deltaY && event.deltaY !== 0) {
          const currentDelta = event.deltaY;
          const currentTimestamp = event.timeStamp;
          const timeSinceLastEvent = currentTimestamp - this.lastScrollTimestamp;

          let stillDecelerating = false;
          if (Math.sign(this.lastScrollDelta) === Math.sign(currentDelta)) {
            stillDecelerating = Math.sign(currentDelta) * (this.lastScrollDelta - currentDelta) >= 0;
          }

          if (!(timeSinceLastEvent < 200 && stillDecelerating)) {
            if (this.infoDrawerOpen) {
              this.closeInfoDrawer(true);
            } else {
              this.setPageIndex(this.currentPageIndex + Math.sign(currentDelta));
            }
          }

          this.lastScrollDelta = currentDelta;
          this.lastScrollTimestamp = currentTimestamp;
        }

        event.preventDefault();
        return false;
      } else {
        return event;
      }
    },
    handleKeypressEvent(event) {
      if (!event.target.matches('aside *')) {
        const keyCode = Number.parseInt(event.keyCode);

        if ([37, 38, 39, 40].includes(keyCode)) {
          if (this.infoDrawerOpen) {
            this.closeInfoDrawer();
          } else {
            if ([37, 38].includes(keyCode)) {
              this.setPageIndex(this.currentPageIndex - 1);
            } else {
              this.setPageIndex(this.currentPageIndex + 1);
            }
          }

          event.preventDefault();
          return false;
        } else {
          return event;
        }
      } else {
        return event;
      }
    }
  },

  watch: {
    currentPageIndex: function(newPageIndex) {
      this.currentVisiblePageIndex = false;

      setTimeout(() => {
        this.currentVisiblePageIndex = newPageIndex;
      }, this.transitionDuration / 2);
    }
  },

  mounted () {
    window.addEventListener('wheel', this.handleWheelEvent, { passive: false });
    window.addEventListener('keydown', this.handleKeypressEvent);

    this.referencesOffset = this.$refs.credits.$el.querySelector('h4:last-of-type').offsetTop;

    document.getElementsByTagName('cite').forEach(element => {
      element.addEventListener('click', this.openReferences);
    });
  },

  unmounted () {
    window.removeEventListener('wheel', this.handleWheelEvent);
    window.removeEventListener('keydown', this.handleKeypressEvent);
  }
}
</script>

<style lang="scss">
@import '~@/assets/normalize.css';
@import '~@/assets/fonts.css';
@import '~@/assets/variables.scss';

html, body {
  overflow: hidden;
}

html {
  font-size: 10px;
}

@media (max-width: 1199px) {
  html {
    font-size: calc(100vw / 1200 * 10);
  }
}

@media (max-width: 600px) {
  html {
    font-size: 5px;
  }
}

#app {
  width: 100%;
  height: 100%;
  font-family: $font-default;
  font-size: 1.2rem;
  font-weight: $weight-regular;
  color: $color-main;
}

.canvas {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 112.0rem;
  height: 56.0rem;
  transform: translateX(-50%) $canvas-translate-y;
  transition: all $transition-duration ease-in-out;
}

h1, h2, h3, h4 {
  margin: 0;
}

h1, h2, blockquote {
  font-family: Fraunces, 'Source Serif Pro', Charter, Georgia, serif;
  font-variation-settings: 'SOFT' 20, 'WONK' 0;
}

h1, h2 {
 font-weight: $weight-black;
}

blockquote {
  font-style: italic;

  sup {
    font-family: $font-default;
    font-style: normal;
  }
}

button {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  box-sizing: border-box;
  padding: 0;
  border: none;
  background-color: transparent;
  cursor: pointer;

  &:focus {
    outline: none;
  }

  &:focus-visible {
    box-shadow: 0 0 0 0.2rem #fff, 0 0 0 0.4rem $color-lighter;
  }
}

main section, nav, aside {
  transform: translate3d(0,0,0);
}

main  {
  section {
    position: absolute;
    visibility: hidden;
    opacity: 0;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    transition: all $transition-duration ease-in-out;
    z-index: 1;

    &.visible, &.cycle {
      visibility: visible;
      opacity: 1;
    }

    &.cycle {
      pointer-events: none;
    }
  }
}

nav {
  position: absolute;
  visibility: hidden;
  opacity: 0;
  top: -2.0rem;
  left: 0;
  right: 0;
  min-width: 75.0rem;
  text-align: center;
  transition: all $transition-duration ease-in-out;
  z-index: 3;

  &.visible {
    visibility: visible;
    opacity: 1;
    top: 0;
  }

  button {
    position: relative;
    display: inline-block;
    vertical-align: top;
    width: 2.4rem;
    height: 3.4rem;
    margin: 1.0rem 1.0rem 0;
    padding: 0;
    border-radius: 0.2rem;
    background: url('~@/assets/icons/nav-left-stroke.svg') center no-repeat transparent;
    background-size: 2.0rem;
    box-shadow: inset 0 0 0 0.2rem transparent;
    transition: all $transition-duration ease-in-out;

    &:last-child {
      background-image: url('~@/assets/icons/nav-right-stroke.svg');
    }

    &:focus-visible {
      box-shadow: inset 0 0 0 0.2rem $color-lighter;
    }

    &:after {
      position: absolute;
      display: block;
      content: '';
      opacity: 0.3;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      background: url('~@/assets/icons/nav-left.svg') center no-repeat transparent;
      background-size: 1.6rem;
      transition: all $transition-duration ease-in-out;
    }

    &:last-child:after {
      background-image: url('~@/assets/icons/nav-right.svg');
    }

    &:hover:after {
      opacity: 1;
    }

    &:focus-visible:after {
      opacity: 1;
    }
  }

  ul {
    position: relative;
    display: inline-block;
    vertical-align: top;
    margin: 1.9rem 0 0;
    padding: 0;
    list-style: none;
    font-size: 1.6rem;

    &:before {
      position: absolute;
      display: block;
      content: '';
      top: 0.7rem;
      left: 0.7rem;
      right: 0.7rem;
      height: 0.2rem;
      background: $color-lighter;
      box-shadow: 0 0 0 0.2rem #fff;
      z-index: -1;
    }

    li {
      position: relative;
      display: inline-block;
      box-sizing: border-box;
      width: 1.4rem;
      height: 1.4rem;
      margin: 0 1.0rem;
      border: 0.2rem solid rgba($color-main, 0.3);
      border-radius: 50%;
      background: #fff;
      box-shadow: 0 0 0 0.2rem #fff;
      transition: all $transition-duration ease-in-out;
      cursor: pointer;

      &:first-child {
        margin-left: 0;
        background: $color-lighter !important;
      }

      &:first-child:hover {
        background: $color-main !important;
      }

      &:last-child {
        margin-right: 0;
      }

      &.past {
        border-color: transparent;
        background: $color-lighter;
      }

      &.active, &:hover {
        border-color: transparent;
        background: $color-main;
      }

      &.chapter {
        margin-left: 5.0rem;
      }

      &:before {
        position: absolute;
        display: block;
        content: '';
        top: -1.2rem;
        bottom: -1.2rem;
        left: -1.2rem;
        right: -3.2rem;
      }

      &:last-child:before {
        right: -1.2rem;
      }

      &.chapter:before {
        left: -3.2rem;
      }

      &:after {
        position: absolute;
        display: block;
        content: attr(data-title);
        visibility: hidden;
        opacity: 0;
        top: 2.2rem;
        left: -8.0rem;
        right: -8.0rem;
        text-align: center;
        font-size: 1.2rem;
        font-weight: $weight-bold;
        line-height: 1.0rem;
        text-shadow:
          0 0.1rem 0 #fff,
          0 0.2rem 0 #fff,
          0 -0.1rem 0 #fff,
          0 -0.2rem 0 #fff,
          0.1rem 0 0 #fff,
          0.2rem 0 0 #fff,
          -0.1rem 0 0 #fff,
          -0.2rem 0 0 #fff,
          0.1rem 0.1rem 0 #fff,
          0.2rem 0.2rem 0 #fff,
          -0.1rem -0.1rem 0 #fff,
          -0.2rem -0.2rem 0 #fff,
          -0.1rem 0.1rem 0 #fff,
          -0.2rem 0.2rem 0 #fff,
          0.1rem -0.1rem 0 #fff,
          0.2rem -0.2rem 0 #fff;
        transition: all $transition-duration ease-in-out;
      }

      &.active:after {
        visibility: visible;
        opacity: 1;
      }

      &:first-child:after {
        visibility: hidden;
        opacity: 0;
      }

      @at-root nav ul:hover li.active:after {
        visibility: hidden;
        opacity: 0;
      }

      &:hover:after {
        visibility: visible !important;
        opacity: 1 !important;
      }
    }
  }
}

aside {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  pointer-events: none;
  transition: all $transition-duration ease-in-out;
  z-index: 10;

  &.open {
    background: rgba(255,255,255,0.4);
    pointer-events: all;
  }

  &:after {
    position: absolute;
    display: block;
    content: '';
    top: 1.2rem;
    right: 1.2rem;
    width: 3.0rem;
    height: 3.0rem;
    border-radius: 50%;
    background: #fff;
    pointer-events: none;
    z-index: 11;
  }

  * {
    pointer-events: all;
    z-index: 12;
  }

  button {
    position: absolute;
    opacity: 0.33;
    top: 1.4rem;
    right: 1.4rem;
    width: 2.6rem;
    height: 2.6rem;
    border: 0.2rem solid $color-main;
    border-radius: 50%;
    box-shadow: 0 0 0 0.2rem transparent, 0 0 0 0.4rem transparent;
    transition: all $transition-duration ease-in-out;
    z-index: 13;

    &:hover {
      opacity: 1 !important;
    }

    &:focus-visible {
      opacity: 1 !important;
    }

    @at-root aside.open button {
      opacity: 0.65;
      border: 0.2rem solid #fff;
      transform: rotate(45deg);    
    }

    @at-root aside.open button:focus-visible {
      box-shadow: 0 0 0 0.2rem #3C4250, 0 0 0 0.4rem rgba(255,255,255,0.45);
    }

    &:before, &:after {
      position: absolute;
      display: block;
      content: '';
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      background: url('~@/assets/icons/info.svg') center no-repeat;
      background-size: 1.6rem;
      pointer-events: none;
      transition: all $transition-duration ease-in-out;
    }

    @at-root aside.open button:before {
      opacity: 0;
    }

    &:after {
      opacity: 0;
      background-image: url('~@/assets/icons/close.svg');
      transform: rotate(45deg);
    }

    @at-root aside.open button:after {
      opacity: 1;
    }
  }

  section {
    position: absolute;
    overflow-y: auto;
    visibility: hidden;
    opacity: 0;
    box-sizing: border-box;
    top: 0;
    bottom: 0;
    right: 0;
    width: 36.0rem;
    padding: 2.6rem 3.0rem 3.0rem;
    color: #fff;
    background: #3C4250;
    transform: translateX(1.5rem);
    transition: all $transition-duration ease-in-out;

    @at-root aside.open section {
      visibility: visible;
      opacity: 1;
      transform: translateX(0);
    }
  }
}
</style>
