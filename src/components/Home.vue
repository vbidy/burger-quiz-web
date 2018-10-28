<template>
  <div class="hello">
    <h1>Welcome to {{ page }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Hello',
  data () {
    return {
      page: 'Page d\'accueil qui sert à rien'
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
