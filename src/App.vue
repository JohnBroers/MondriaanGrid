<template>
  <div id="app" class="wrapper">
    <div class="mondriaan">
      <span v-for="(block, index) in blocks"
        :key="index + 0"
        :class="`block block--r${block.rowSpan}-c${block.colSpan} block--${block.colorIndex}`">
      </span>
    </div>
    <button @click.prevent="regenerate" class="button">Generate new</button>
  </div>
</template>
<script>

export default {
  name: 'app',
  data () {
    return {
      blocks: [],
      colors: [
        {
          color: 'white',
          percentage: 70
        },
        {
          color: 'red',
          percentage: 10
        },
        {
          color: 'blue',
          percentage: 10
        },
        {
          color: 'yellow',
          percentage: 10
        }
      ]
    }
  },
  computed: {
    weightedColors () {
      let colorArray = []
      this.colors.forEach(color => {
        for (let i = 0; i < color.percentage; i++) {
          colorArray.push(color.color)
        }
      })
      return colorArray
    }
  },
  methods: {
    generateBlocks () {
      for (let i = 0; i < 20; i++) {
        this.blocks.push({
          colSpan: this.randomNumber(3),
          rowSpan: this.randomNumber(3),
          colorIndex: this.randomColor()
        })
      }
      for (let i = 0; i < 10; i++) {
        this.blocks.push({
          colSpan: this.randomNumber(2),
          rowSpan: this.randomNumber(2),
          colorIndex: this.randomColor()
        })
      }
    },
    randomNumber (max) {
      return Math.floor(Math.random() * max + 1)
    },
    randomColor () {
      const randomNumber = Math.floor(Math.random() * this.weightedColors.length)
      return this.weightedColors[randomNumber]
    },
    regenerate () {
      this.blocks = []
      this.generateBlocks()
    }
  },
  created () {
    this.generateBlocks()
  }
}
</script>

<style lang="scss">
$mondriaan-blue: #225095;
$mondriaan-yellow: #FAC901;
$mondriaan-red: #DD0100;
$mondriaan-black: #000002;
$mondriaan-white: #F9F9F9;

$block-colors: (
  white: $mondriaan-white,
  red: $mondriaan-red,
  blue: $mondriaan-blue,
  yellow: $mondriaan-yellow,
);

$block-size: 100px;
$mob-block-size: 75px;

body {
  margin: 0;
  padding: 0;
  background-color: $mondriaan-white;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  line-height: 1em;
}

.wrapper {
  display: flex;
  margin: 2em;
  align-items: center;
  flex-direction: column;
}

.mondriaan {
  display: grid;
  width: 100%;
  max-width: 760px;
  height: 760px;
  background-color: $mondriaan-black;
  grid-template: repeat(auto-fit, minmax($block-size, 1fr)) / repeat(auto-fit, minmax($block-size, 1fr));
  grid-auto-columns: $block-size;
  grid-auto-rows: $block-size;
  grid-auto-flow: dense;
  gap: 10px;
  overflow: hidden;
  border: 10px solid $mondriaan-black;
  .block {
    background-color: $mondriaan-white;
    display: inline-block;
    transform: scale(0);
    animation: zoomIn ease-in-out .5s;
    animation-fill-mode: forwards;

    @for $r from 0 to 4 {
      @for $c from 0 to 4 {
        &--r#{$r}-c#{$c} {
          animation-delay: $r * 0.15s;
          grid-column: $c span;
          grid-row: $r span;
        }
      }
    }

    @each $name, $var in $block-colors {
        &--#{$name} {
            background-color: #{$var};
        }
    }

  }
}

.button {
  margin-top: 1em;
  color: $mondriaan-black;
  border: 3px solid $mondriaan-black;
  background-color: $mondriaan-white;
  padding: .5em 3em;
  font-size: 1em;
  text-transform: uppercase;
  font-weight: 900;
  font-family: Arial, Helvetica, sans-serif;
  &:hover {
    background-color: $mondriaan-yellow;
    cursor: pointer;
  }
  &:focus, &:active {
    outline: none;
  }
}

@media screen and (max-width: 480px) {
  .mondriaan {
    grid-template: repeat(auto-fit, minmax($mob-block-size, 1fr)) / repeat(auto-fit, minmax($mob-block-size, 1fr));
  }
}

@keyframes zoomIn {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}
</style>
