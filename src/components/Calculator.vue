<template>
  <div class="calculator">
    <div class="display">{{current || '0'}}</div>
    <div @click="clear" class="btn clear">C</div>
    <div @click="sign" class="btn operator">+/-</div>
    <div @click="percent" class="btn operator">%</div>
    <div @click="divide" class="btn operator">÷</div>
     <div @click="sqroot" class="btn operator">SQRT</div>
    <div @click="doSquare" class="btn operator">x^2</div>
    <div @click="denominate" class="btn operator">1/x</div>
    <div @click="exponentiate" class="btn operator">x^y</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="times" class="btn operator">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus" class="btn operator">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="add" class="btn operator">+</div>
    <div @click="append('0')" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: null,
      current: '',
      operator: null,
      previousOperator: null,
      operatorClicked: false,
    }
  },
  methods: {
    clear() {
      this.current = '';
    },
    sign() {
      this.current = this.current.charAt(0) === '-' ? 
        this.current.slice(1) : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    //TODO: For two operand operators, implement repeat function
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    times() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
      },
    minus() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    sqroot() {
      this.current = `${Math.sqrt(parseFloat(this.current))}`},
    doSquare() {
      this.current = `${parseFloat(this.current)*parseFloat(this.current)}`
    },
    denominate() {
      this.current = `${1 / parseFloat(this.current)}`
    },
    exponentiate() {
      this.operator = (a, b) => a ** b;
      this.setPrevious();
    },
    //fixed order so order-dependent operations work right
    equal() {
      this.current = `${this.operator(
        parseFloat(this.previous), 
        parseFloat(this.current)
      )}`;
      this.previous = null;
      this.previousOperator = this.operator;
    }
  }
}
</script>

<style scoped>
.calculator {
  margin: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
}

.zero {
  grid-column: 1 / 3;
}

.btn {
  background-color: #F2F2F2;
  border: 1px solid #999;
}

.operator {
  background-color: orange;
  color: white;
}

.clear {
  background-color: darkred;
  color: white; 
}
</style>
