<template>
  <div class="hello">
    <template v-if="!ifFinish">
      <p class="q-title">もんだい {{currentQuestion+1}}</p>
      <span v-for="(n, i2) in values[currentQuestion].prob" class="s-num">
        {{ n }}
        <span v-if="i2 !== (values[currentQuestion].prob.length - 1)" class="s-num">+</span>        
      </span>
      = 
      <input type="text" v-model="values[currentQuestion].answer" class="s-ans">
      <button class="btn-next-pProblem" v-on:click="goNext()">つぎへ</button>
    </template>

    <template v-if="ifFinish">
      <p class="q-title">けっか</p>
      <div v-for="(item, i) in values" class="result-box" v-bind:class="{'is-box-true': item.result, 'is-box-false': !item.result}">
        <span v-for="(n, i2) in item.prob">
          {{ n }}
          <span v-if="i2 !== (item.prob.length - 1)">+</span>        
        </span>
        = 
        <span>{{item.answer}}</span>        
        <span class="result" v-bind:class="{'is-true': item.result, 'is-false': !item.result}">
          <template v-if="item.result">○</template>
          <template v-else>X</template>
        </span>
      </div>
      <p class="result-text">せいかいは、{{correctQuestionNum}}こ！</p>      
      <p><button class="btn-reload-pProblem" v-on:click="reload()">もう一度！</button></p>
    </template>

    <!-- <button v-on:click="submit()">submit</button>
    <button v-on:click="reload()">reload</button> -->

    <!-- <div class="footers">
      <router-link to="/">HOME</router-link>
    </div> -->

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      numOfquestions: 3,    
      values: [],
      msg: 'Math Practice',
      currentQuestion: 0,
      ifFinish: false,
      correctQuestionNum: 0
    }
  },
  created: function() {
    this.getquestion()
  },
  methods: {
    getquestion: function() {
      for(var i = 0; i < this.numOfquestions; i++) {
        let arr = {}
        arr.prob = [this.getRandomInt(9, 0), this.getRandomInt(9, 0)]
        this.values.push(arr)
      }
      this.currentQuestion = 0
    },
    getRandomInt: function(max, min) {
      return Math.floor(Math.random() * (max - min) + min);
    },

    submit: function(){  
      // console.log(this.values)    
      this.ifFinish = true
      for(var i = 0; i < this.values.length; i++) {
        let ans = this.values[i].prob.reduce(function (accumulator, currentValue, currentIndex, array) {
          return accumulator + currentValue;
        })
        let n = this.values[i].answer

        // set results
        let r = (Number(ans) === Number(n))? true : false
        this.$set(this.values[i], "result", r)

        if(r){
          this.correctQuestionNum++
        }
      }
    },
    reload: function(){

      for(var i = 0; i < this.values.length; i++) {
        this.$set(this.values[i], "result", null)
        this.$set(this.values[i], "answer", null)
      }

      this.ifFinish = false
      this.currentQuestion = 0
      this.correctQuestionNum = 0
    },

    goNext: function(){
      let self = this
      
      if((self.currentQuestion + 1) >= self.numOfquestions){
          self.submit()
      } else {
          if(typeof self.values[self.currentQuestion].answer === 'undefined' || self.values[self.currentQuestion].answer === "" || self.values[self.currentQuestion].answer === null) {
              alert("もんだいにこたえてね！")
          } else {
            self.currentQuestion++
          }
      }

    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.q-title{
  font-size: 16px;
  line-height: 1.6;
  color: #000;
  margin-bottom: 4em;
  position: relative;
}
.q-title:after{
  display: block;
  content: "";
  width: 2em;
  height: 2px;
  background-color: #ddd;
  margin: .2em auto;
}
.s-num{
  font-size: 24px;
  line-height: 1.6;
  color: #000;

}
.s-ans{
  font-size: 24px;
  line-height: 1.6;
  color: #000;
  width: 4em;
  border: 1px solid #999;
  text-align: center;
  padding: 0.4em;
}
.btn-next-pProblem{
  font-size: 16px;
  line-height: 1.6;
  color: #000;
  display: block;
  margin: 2em auto;
  border: 1px solid #999;
  background-color: #f1f1f1;
}
.btn-reload-pProblem{
  font-size: 16px;
  line-height: 1.6;
  color: #000;
  display: block;
  margin: 2em auto;
  border: 1px solid #999;
  background-color: #f1f1f1;

}
.result-text{
  font-size: 16px;
  line-height: 1.6;
  color: #000;
  display: block;
  text-align: center;
  margin-top: 2em;
}
.result-box{
  width: 53.3vmin;
  text-align: left;
  margin: 0 auto 0;
}
.is-false{
  font-size: 16px;
  line-height: 1.6;  
}
.is-true{
  font-size: 16px;
  
}
.is-box-true{
  color: #2196f3;
}
.is-box-false{
  color: #999;
}
</style>
