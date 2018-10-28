<template>
  <div>
    <h1>Panneau d'admin</h1>
    <div class="row">
      <div class="col-md-6" v-for="teamName in teams">
        <div>{{ teamName | capitalize }}</div>
        <div v-if="Number.isInteger(scores[teamName])"> {{ scores[teamName] }}</div>
        <div>
          <button v-on:click="updateScore(teamName,1)">+1</button>
        </div>
        <div>
          <button v-on:click="updateScore(teamName,4)">+4</button>
        </div>
        <div>
          <button v-on:click="resetScore(teamName)">Reset (0)</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {globalVars} from '../main.js'
    export default {
      name: "Admin",
      data () {
        return {
          scores: {},
          teams:['ketchup','mayo']
        }
      },
      created() {
        this.getScores()
      },
      methods:{
        getScores: function() {
          this.$http.get(globalVars.apiUrl + 'scores/').then(result => {
            this.scores = result.body;
            return this.scores;
          }, error => {
            console.error(error);
          });
        },
        getScore: function(team){
          let scores = this.scores;
          return parseInt(scores[team]);
        },
        updateScore: function(team,pointsToAdd) {
          let currentScore = this.getScore(team);
          let newScore = {};
          let theoreticalNewScore = currentScore + pointsToAdd;

          if (theoreticalNewScore <= 25) {
            newScore.score = theoreticalNewScore;
          } else {
            newScore.score = 25;
            console.warn("Score higher than 25 impossible; setting score to 25")
          }
          this.$http.patch(globalVars.apiUrl + 'scores/' + team, newScore).then(result => {
            if (result.ok) {
              this.scores[team] = newScore.score;
            }
          }, error => {
            console.error(error);
          });
        },
        resetScore: function(team){
          let newScore = {};
          newScore.score = 0;
          this.$http.patch(globalVars.apiUrl + 'scores/' + team, newScore).then(result => {
            if(result.ok){
              this.scores[team] = newScore.score;
            }
          }, error => {
            console.error(error);
          });
        }
      },
      filters: {
        capitalize: function (value) {
          if (!value) {
            return ''
          }
          value = value.toString();
          return value.charAt(0).toUpperCase() + value.slice(1);
        }
      }
    }

</script>

<style scoped>

</style>
