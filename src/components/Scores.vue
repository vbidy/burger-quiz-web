<template>
    <div>
      <div class="row">
        <div v-for="(value, key) in scores" class="col-md-6">
          <div class="score" v-bind:class="key">
            {{ value }}
          </div>
        </div>
      </div>
    </div>
</template>

<script>
    import {globalVars} from '../main.js'
    export default {
        name: "Scores",
        created() {
          this.getScores()
        },
        data() {
          return {
            scores: {}
          }
        },
        methods: {
          getScores() {
            this.$http.get(globalVars.apiUrl + 'scores/').then(result => {
              this.scores = result.body;
              console.log(result);
            }, error => {
              console.error(error);
            });
          }
        }
    }
</script>

<style scoped>

</style>
