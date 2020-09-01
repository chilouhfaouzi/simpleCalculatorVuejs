<template>
  <div class="calculator">
    <div class="display">
      {{ current || "0" }} <span class="pre">{{ previous || "" }}</span>
      <span @click="remove" class="remove"> &lt;==</span>
    </div>
    <div @click="clear" class="btn clear">C</div>
    <div @click="sign" class="btn reg">+/-</div>
    <div @click="percent" class="btn reg">%</div>
    <div @click="divide" class="btn opera">/</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="multiple" class="btn opera">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus" class="btn opera">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="plus" class="btn opera">+</div>
    <div @click="append('0')" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn opera">=</div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      previous: null,
      current: "",
      operator: null,
      operatorClicked: false,
    };
  },
  methods: {
    clear() {
      this.current = "";
    },
    remove() {
      if (this.current) {
        this.current = this.current.slice(0, -1);
      }
    },
    opera() {
      this.current = "";
    },
    sign() {
      this.current =
        this.current.charAt(0) === "-"
          ? this.current.slice(1)
          : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = "";
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    dot() {
      if (this.current.indexOf(".") === -1) {
        //indexOf() return -1 if dont exist // not -2 or -3 ...
        this.append(".");
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => b / a;
      this.setPrevious();
    },
    multiple() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    minus() {
      this.operator = (a, b) => b - a;
      this.setPrevious();
    },
    plus() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    equal() {
      this.current = `${this.operator(
        parseFloat(this.current),
        parseFloat(this.previous)
      )}`;
      this.previous = null;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator {
  width: 600px;
  font-size: 40px;
  margin: auto;
  display: grid;
  grid-template-columns: repeat(4.1fr);
  grid-auto-rows: minmax(50px, auto);
}
.clear {
  color: crimson;
}
.remove {
  position: absolute;
  right: 10px;
  color: forestgreen;
  font-size: 35px !important;
}
.reg {
  color: forestgreen;
}
.display {
  grid-column: 1/5;
  background-color: rgb(59, 53, 56);
  color: #fff;
  position: relative;
  overflow: hidden;
}
.btn {
  border: 1px solid rgba(0, 0, 0, 0.466);
  background-color: #eee;
  padding: 5px;
}
.opera {
  background-color: coral;
  color: white;
}
.zero {
  grid-column: 1/3;
  text-align: center !important;
}
.pre {
  position: absolute;
  left: 5px;
  bottom: 5px;
  font-size: 10px;
}
</style>
