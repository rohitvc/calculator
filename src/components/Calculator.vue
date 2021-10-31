<template>
  <div class="container">
    <div class="output">{{ output }}</div>
    <div class="layout">
      <div class="block digit-0" @click="onDigitClick(0)">0</div>
      <div class="block digit-1" @click="onDigitClick(1)">1</div>
      <div class="block digit-2" @click="onDigitClick(2)">2</div>
      <div class="block digit-3" @click="onDigitClick(3)">3</div>
      <div class="block digit-4" @click="onDigitClick(4)">4</div>
      <div class="block digit-5" @click="onDigitClick(5)">5</div>
      <div class="block digit-6" @click="onDigitClick(6)">6</div>
      <div class="block digit-7" @click="onDigitClick(7)">7</div>
      <div class="block digit-8" @click="onDigitClick(8)">8</div>
      <div class="block digit-9" @click="onDigitClick(9)">9</div>
      <div class="block op-add" @click="onOperatorClick('+')">+</div>
      <div class="block op-sub" @click="onOperatorClick('-')">-</div>
      <div class="block op-mul" @click="onOperatorClick('*')">*</div>
      <div class="block op-div" @click="onOperatorClick('/')">/</div>
      <div class="block eq" @click="onEqualClick">=</div>
      <div class="block clear" @click="clearCalculator">C</div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      output: '',
      equalPressed: false
    }
  },
  methods: {
    clearCalculator () {
      this.output = ''
    },
    onDigitClick (digit) {
      if (this.equalPressed) {
        this.equalPressed = false
        this.output = ''
      }

      // remove leading zero from the start
      if (/^0/.test(this.output) && this.output.length === 1) {
        this.output = this.output.slice(1)
      }

      // remove leading zeroes from anywhere in the middle
      if (/(\+|-|\*|\/)0$/gi.test(this.output)) {
        this.output = this.output.replace(/.$/, digit)
        return
      }

      this.output = `${this.output}${digit}`
    },
    onOperatorClick (operator) {
      // reset equalPressed
      if (this.equalPressed) {
        this.equalPressed = false
      }
      // check if output = '', and operator other than -
      if (!this.output.length && operator !== '-') {
        return
      }

      // if last 2 characters are operator do nothing
      if (/(\+|-|\*|\/){2,}$/gi.test(this.output)) {
        return
      }

      // replace old trailing operator (+ or -)
      if (/(\+|-)$/gi.test(this.output)) {
        this.output = this.output.replace(/.$/, operator)
        return
      }

      // replace old trailing operators (* or /) if new is not -
      if (/(\/|\*)$/gi.test(this.output) && operator !== '-') {
        this.output = this.output.replace(/.$/, operator)
        return
      }

      this.output = `${this.output}${operator}`
    },
    onEqualClick () {
      // check if last character is digit
      if (!/\d$/gi.test(this.output)) {
        return
      }

      // clear output if divided by 0
      if (/\/-?0+(\.?)(0?)/gi.test(this.output)) {
        this.output = ''
        return
      }
      this.equalPressed = true
      // eslint-disable-next-line no-eval
      this.output = `${Math.floor(eval(this.output))}`
    }
  }
}
</script>

<style scoped>
.container {
  width: 400px;
  border: 2px solid #bbb;
  border-radius: 30px;
  overflow: hidden;
}

.container .layout {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 100px));
  grid-template-rows: repeat(4, minmax(0, 100px));
  background: rgba(255, 255, 255, 0.2);
  box-shadow: 0px 20px 40px rgba(255, 255, 255, 0.2);
}

.container .layout .block {
  height: 100px;
  width: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  font-size: 1.5rem;
  font-weight: 500;
}

.container .layout .block:hover {
  background: #cb3066;
  color: #fff;
  font-weight: bold;
}

.container .output {
  height: 100px;
  width: 100%;
  font-size: 2rem;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding: 0 1rem;
}
</style>
