<template>
   <div class="calculator">
       <div class="display">{{ current }}</div>
       <div @click="clear" class="btn">AC</div>
       <div @click="sign" class="btn">+/-</div>
       <div @click="percent" class="btn">%</div>
       <div @click="divide" class="btn operator">÷</div>
       <div @click="append($event)" class="btn">7</div>
       <div @click="append($event)" class="btn">8</div>
       <div @click="append($event)" class="btn">9</div>
       <div @click="multiply" class="btn operator">x</div>
       <div @click="append($event)" class="btn">4</div>
       <div @click="append($event)" class="btn">5</div>
       <div @click="append($event)" class="btn">6</div>
       <div @click="minus" class="btn operator">-</div>
       <div @click="append($event)" class="btn">1</div>
       <div @click="append($event)" class="btn">2</div>
       <div @click="append($event)" class="btn">3</div>
       <div @click="add" class="btn operator">+</div>
       <div @click="append($event)" class="btn zero">0</div>
       <div @click="dot" class="btn">.</div>
       <div @click="answer" class="btn operator">=</div>
   </div>
</template>

<script>
export default {
  name: 'Calculator',
  data(){
    return {
      current: '0',
      previous: '0',
      operator: null,
      operatorClicked: false
    }
  },
  props: {
    msg: String
  },
  methods: {
    clear(){
      this.current = '0'

    },
    sign(){
      if (this.current !== '0'){
        this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`
      }

    },
    reset(){
      this.operatorClicked = false;


    },
    switch(){
      this.previous = this.current;
      this.operatorClicked = true;
      this.current = this.previous

    },
    percent(){
      if (this.current !== '0'){
        this.current = `${parseFloat(this.current) / 100}`
      }
    },
    append(event){
      if(this.operatorClicked){
        this.current = (this.current.charAt(0) === '0' && this.current.indexOf(".") === -1) ?
          `${this.current.slice(1)}${event.target.innerHTML}` : `${event.target.innerHTML}`;
        this.reset()
      }
      else{
        this.current = (this.current.charAt(0) === '0' && this.current.indexOf(".") === -1) ?
          `${this.current.slice(1)}${event.target.innerHTML}` : `${this.current}${event.target.innerHTML}`
      }


    },
    add(){
      this.switch();
      this.operator = (a,b) => a + b;
    },
    dot(){
      if(this.current.indexOf(".") === -1){
        this.current = `${this.current}.`
      }
    },
    divide(){
      this.switch();
      this.operator = (a,b) => a / b;

    },
    multiply(){
      this.switch();
      this.operator = (a,b) => a * b
    },
    minus(){
      this.switch();
      this.operator = (a,b) => a - b
    },
    answer(){
      this.current  = `${this.operator(parseFloat(this.previous), parseFloat(this.current))}`;
      this.previous = '0'

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .calculator{
        margin: 0 auto;
        width: 400px;
        font-size: 40px;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-auto-rows: minmax(50px, auto)
    }

    .display{
        color: white;
        padding-top: 3rem;
        text-align: right;
        grid-column: 1 / 5;
        background-color: #5D5A59;

    }
    .zero{
        grid-column: 1/3;
    }

    .btn{
        border: 1px solid #999;
        background-color: #eeee;
        cursor: pointer;
    }

    .operator {
        background-color: orange;
        color: white;
    }

</style>
