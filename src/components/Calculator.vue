<template>
  <div class = "calculator">

    <div class = "display">{{current || 0}}</div>
    <div @click = "clear" class = "btn">C</div>
    <div @click = "sign" class = "btn">+/-</div>
    <div @click = "percent" class = "btn">%</div>
    <div @click = "divide" class = "operator btn">/</div>

    <div @click ="append('7')" class = "btn">7</div>
    <div @click ="append('8')" class = "btn">8</div>
    <div @click ="append('9')" class = "btn">9</div>
    <div @click = "times" class ="operator btn">x</div>

    <div @click ="append('4')" class = "btn">4</div>
    <div @click ="append('5')" class = "btn">5</div>
    <div @click ="append('6')" class = "btn">6</div>
    <div @click = "minus" class ="operator btn">-</div>

    <div @click ="append('1')" class = "btn">1</div>
    <div @click ="append('2')" class = "btn">2</div>
    <div @click ="append('3')" class = "btn">3</div>
    <div @click = "plus" class ="operator btn">+</div>

    <div @click ="append('0')" class="btn zero">0</div>
    <div @click = "dot" class = "btn">.</div>
    <div @click = "equal" class = "operator btn">=</div>

  </div>
</template>

<script>
export default {
  //This is where we declare functions
  data() {
    return {
      previous: null,
      current: '0',
      operator: null,
      operatorClicked: false,
    }
  },
  methods: {
    clear(){
      this.current = '0'
    },
    setPrevious(){
      this.previous = this.current;
        this.operatorClicked = true;
    },
    sign(){
      this.current = this.current.charAt(0) === '-' ?
        this.current.slice(1) : `-${this.current}`; //interesting, what is this?
    },
    percent(){
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(number){
      if (this.operatorClicked){
        this.current = '';
        this.operatorClicked = false;
      }

      //fixes the zero staying around
      this.current = this.current.charAt(0) === '0' ?
        this.current.slice(1) : `${this.current}`;
      
      this.current = `${this.current}${number}`;
    },
    dot(){
      if (this.current.indexOf('.') === -1){
        this.append('.');
      }
    },
    times(){
        this.operator = (a, b) => a * b;
        this.setPrevious();
    },
    divide(){
        this.operator = (a, b) => a / b;
       this.setPrevious();
    },
    plus(){
        this.operator = (a, b) => a + b;
        this.setPrevious();
    },
    minus(){
        this.operator = (a, b) => a - b;
        this.setPrevious();
    },
    equal(){
      this.current = `${this.operator(
        parseFloat(this.previous), 
        parseFloat(this.current)
        )}`; //interesting, this casts the whole thing to a string
        this.previous = null;
    }

  }
}
</script>

<style scoped>
/* Any children of my calculator class will be placed
  into a 4 by infinity grid */
.calculator {
  margin: 0 auto;
  width: 300px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

/* Our display class should take up the top 4 columns */
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
  border: 1px solid #999
}

.operator {
  background-color: orange;
  color: white;
}
</style>
