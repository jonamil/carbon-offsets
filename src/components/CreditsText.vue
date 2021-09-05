<template>
  <section>
    <h1>Carbon Neutrality:<br>A Label For Sale?</h1>
    <h3>An introduction to carbon offsets:<br>how they work and where they fail</h3>
    <h4>Team</h4>
    <p>Luka Saje<br>Jona Pomerance</p>
    <h4>Background</h4>
    <p>This project was created within the course “Friday for Future” taught by Prof. Klaus Keller at&nbsp;<a target="_blank" href="https://fh-potsdam.de/en/">Fachhochschule Potsdam</a> in Summer 2021.</p>
    <a target="_blank" href="https://fh-potsdam.de/en/">
      <img src="@/assets/fhp.svg" />
    </a>
    <h4>Publish Date</h4>
    <p>30 September 2021</p>
    <h4>References</h4>
    <ul>
      <li
        v-for="(reference, index) in sortedReferences"
        :key="index"
        v-html="reference"
      />
    </ul>
  </section>
</template>

<script>
export default {
  name: 'CreditsText',

  props: {
    sources: {
      type: Object,
      default: () => {}
    }
  },

  computed: {
    sortedReferences() {
      let references = [];

      for (const reference in this.sources) {
        if (this.sources[reference].length >= 1 && this.sources[reference] !== '') {
          references.push(this.parseReference(this.sources[reference][1]));
        }
      }

      references.sort();
      return references;
    }
  },

  methods: {
    parseReference(text) {
      text = text.replace(/(?:(?:https?):\/\/|www\.)(?:\([-A-Z0-9+&@#/%=~_|$?!:,.]*\)|[-A-Z0-9+&@#/%=~_|$?!:,.])*(?:\([-A-Z0-9+&@#/%=~_|$?!:,.]*\)|[A-Z0-9+&@#/%=~_|$])/igm, match => {
        return '<a target="_blank" href="' + match + '">' + match + '</a>';
      });
      text = text.replace(/\*(.*)\*/gim, '<em>$1</em>');

      return text;
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~@/assets/variables.scss';

section {
  h1 {
    font-size: 2.7rem;
    line-height: 1.125;
  }

  h3 {
    margin-top: 1.1rem;
    font-size: 1.6rem;
    font-weight: $weight-medium;
    line-height: 1.25;
  }

  h4 {
    margin: 2.4rem 0 0.6rem;
    font-size: 1.3rem;
    font-weight: $weight-semibold;
    line-height: 1.2;
    letter-spacing: 0.1rem;
    text-transform: uppercase;
    color: rgba(255,255,255,0.55);

    &:last-of-type {
      margin-top: 6.9rem;
    }
  }

  p, ul {
    margin: 0;
    font-size: 1.3rem;
    line-height: 1.35;
  }

  ul {
    padding-left: 1.5rem;
    list-style: none;

    li {
      margin-bottom: 1.1rem;
      text-indent: -1.5rem;

      &:deep(a) {
        word-break: break-all;
      }
    }
  }

  p, ul li {
    &:deep(a) {
      padding-bottom: 0.2rem;
      text-decoration: none;
      box-shadow: inset 0 -0.2rem rgba(255,255,255,0.3);
      color: inherit;
      transition: box-shadow $transition-duration ease-in-out;

      &:hover {
        box-shadow: inset 0 -0.2rem rgba(255,255,255,0.9);
      }
    }
  }

  > a {
    display: inline-block;
    vertical-align: top;

    img {
      display: block;
      opacity: 0.95;
      width: 16.0rem;
      margin: 1.4rem 0 -0.3rem 0.1rem;
      transition: opacity $transition-duration ease-in-out;

      &:hover {
        opacity: 0.6;
      }
    }
  }
}
</style>