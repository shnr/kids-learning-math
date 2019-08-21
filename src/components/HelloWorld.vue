<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <router-link to="/q">Start!</router-link>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      numOfquestions: 10,    
      values: [],
      msg: 'Math Practice'
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
    },
    getRandomInt: function(max, min) {
      return Math.floor(Math.random() * (max - min) + min);
    },
    submit: function(){      
      for(var i = 0; i < this.values.length; i++) {
        let ans = this.values[i].prob.reduce(function (accumulator, currentValue, currentIndex, array) {
          return accumulator + currentValue;
        })
        let n = this.values[i].answer

        // set results
        let r = (Number(ans) === Number(n))? true : false
        this.$set(this.values[i], "result", r)
      }
    },
    reload: function(){
      
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
</style>
