<template>
<v-container fluid>
    <v-row>
      <v-col align="center">
        <v-card class="v-card d-inline-block pa-8 px-sm-12">
          <v-row align="stretch" justify="center">
            {{qnum + 1}} of {{qnumtotal}} 
          </v-row>
          <v-row>
            <v-col align="center">
                  <v-expand-x-transition>
                    <div class="question">
                      <strong>Question:</strong><br/>
                      {{questions[qnum]["question"]}}
                    </div>
                  </v-expand-x-transition>
            </v-col>
          </v-row>
          <v-row v-for="answer in questions[qnum]['answers']" v-bind:key="answer">
            <v-col align="center" style="overflow: hidden;">
              <v-hover v-slot:default="{ hover }">
                <v-card :elevation="hover ? 4 : 1" @click="next(answer)" class="v-answer">
                    {{answer.text}}
                </v-card>
              </v-hover>
            </v-col>
          </v-row>

          <v-overlay
          :absolute="absolute"
          :opacity="opacity"
          :value="finished"
          >
            <v-card light class="v-card d-inline-block pa-8 px-sm-12" style="overflow-y: auto">
              
              <Chart width="250" height="250" type="radar" :data="getDataForChart()"></Chart>
              <br/>
              <Chart width="250" height="250" type="horizontalBar" :data="getDataForChart()"></Chart>

            </v-card>
            
          </v-overlay>

        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

import qbank from "../assets/qbanktest.json"
import Chart from "./Chart.vue"

export default {
  name: 'QView',
  data() {
    return {
      qdata: Object,
      questions: Array,
      qnum: 0,
      qnumtotal: 0,
      finished: false
    }
  },
  components: {
    Chart
  },
  methods: {
    next: function(answer){
      //log answer
      this.logAnswer(answer)

      //next
      if(this.qnum < this.qnumtotal - 1){
        this.qnum = this.qnum + 1;
      }
      else{
        this.finished = true
        this.getDataForChart();
      }
    },
    logAnswer: function(answer){
      let cat = answer.category;
      
      this.qdata.Categories.forEach(function(icat){
        if(cat == icat.value){
          if(icat.points != undefined){
            icat.points = icat.points + answer.points;
          }
          else{
            icat.points = answer.points;
          }
        }
        else if(icat.points == undefined || icat.points == null){
            icat.points = 0;
          }
      });
    },
    getDataForChart: function(){

      let tvalues = []
      let points = []
      let cats = this.qdata.Categories;

      for(let i = 0; i < cats.length; i++){
        tvalues.splice(i, 0, cats[i].text)
        points.splice(i, 0, cats[i].points)
      }

      let cdata = {
          labels: tvalues,
          datasets: [{
              label: "Spread",
              data: points,
              backgroundColor: "#CCCCFF77"
          }]
      }

      return cdata;
    }
  },
  created: function(){
    this.qdata = qbank;
    this.questions = this.qdata.Bank;
    this.qnumtotal = this.questions.length;
  }

}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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

.question{
  font-size: 1.2rem;
}

.v-answer:hover {
  background-color: #eeeeff !important;
}

.v-answer{
 width: 100%;
 padding: 1vw;
}


</style>
