<template>
  <div class="calculator-container">
    <div class="result">{{ current || '0' }}</div>
    <div class="calc-elem" @click="clear">C</div>
    <div class="calc-elem" @click="negative">+/-</div>
    <div class="calc-elem" @click="setOperator('%')">%</div>
    <div class="calc-elem operator" @click="setOperator('÷')">÷</div>
    <div class="calc-elem" @click="append('7')">7</div>
    <div class="calc-elem" @click="append('8')">8</div>
    <div class="calc-elem" @click="append('9')">9</div>
    <div class="calc-elem operator" @click="setOperator('*')">x</div>
    <div class="calc-elem" @click="append('4')">4</div>
    <div class="calc-elem" @click="append('5')">5</div>
    <div class="calc-elem" @click="append('6')">6</div>
    <div class="calc-elem operator" @click="setOperator('-')">-</div>
    <div class="calc-elem" @click="append('1')">1</div>
    <div class="calc-elem" @click="append('2')">2</div>
    <div class="calc-elem" @click="append('3')">3</div>
    <div class="calc-elem operator" @click="setOperator('+')">+</div>
    <div class="calc-elem zero" @click="zero">0</div>
    <div class="calc-elem dot" @click="dot">.</div>
    <div class="calc-elem operator" @click="equalFunc">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: "",
      previous: null,
      operatorClicked: false,
      currentOperator: null,
      operators: {
        "+": (a, b) => parseFloat(a) + parseFloat(b),
        "-": (a, b) => parseFloat(a) - parseFloat(b),
        "÷": (a, b) => parseFloat(a) / parseFloat(b),
        "*": (a, b) => parseFloat(a) * parseFloat(b),
        "%": (a, b) => (a / b) * 100 + "%"
      }
    };
  },
  methods: {
    append(number) {
      if (this.operatorClicked) {
        this.current = "";
        this.operatorClicked = false;
      }
      this.current += number;
    },
    clear() {
      this.current = "";
    },
    zero() {
      this.current === "" ? (this.current = "") : (this.current += "0");
    },
    dot() {
      if (this.current.includes(".")) {
        return false;
      } else if (this.current === "") {
        this.current += "0.";
      } else {
        this.current += ".";
      }
    },
    negative() {
      if (this.current.charAt(0) === "-") {
        this.current = this.current.slice(1);
      } else {
        this.current = `-${this.current}`;
      }
    },
    setOperator(op) {
      this.currentOperator = op;
      this.previous = this.current;
      this.current = this.currentOperator;
      this.operatorClicked = true;
    },
    equalFunc() {
      if (this.previous === null) {
        return false;
      }
      this.current = this.operators[this.currentOperator](
        this.previous,
        this.current
      );
      this.previous = null;
    }
  }
};
</script>

<style scoped>
.calculator-container {
  max-width: 300px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 5px;
  font-size: 25px;
}
.result {
  grid-column: span 4;
  text-align: right;
  background: #202020;
  color: #fff;
  padding: 10px 30px 10px 0;
  border-radius: 15px;
}

.calc-elem {
  font-family: sans-serif;
  background: #d1d2d6;
  text-align: center;
  padding: 10px;
  border-radius: 15px;
  cursor: pointer;
  transition: 0.4s;
}
.calc-elem:hover {
  background: #c5c6ca;
}
.zero {
  grid-column: span 2;
}

.operator {
  background: #f99013;
  color: #fff;
}

.operator:hover {
  background: #d37b10;
}
</style>
