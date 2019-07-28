<template>

  <div class="hello">
    <h1>{{ msg }}</h1>
<input v-model="naam" placeholder="vul in">
  <button v-on:click="searchMeubel">naam</button>

  <h1 v-if="meubels !== null && meubels.length > 0">Ja! {{submittedNaam}} is een meubel!</h1>
  <h1 v-if="meubels !== null && meubels.length === 0">Helaas, {{submittedNaam}} is geen meubel</h1>

  <div v-for="meubel in meubels" v-bind:key="meubel.name">
    {{ meubel.name }}
    <img :src="meubel.imageUrl">
  </div>
  </div>

</template>

<script>export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Meubels zijn net mensen, maar zijn mensen ook net meubels?',
      naam: '',
      submittedNaam: '',
      ping: null,
      meubels: null
    }
  },
  methods: {
    searchMeubel: function (event) {
      const axios = require('axios')

      if (this.naam.length === 0) {
        this.submittedNaam = ''
        this.meubels = null
        alert('Voer een naam in')
      } else {
        this.submittedNaam = this.naam
        axios.get('https://meubel-backend.herokuapp.com/LeenBakker/Meubel?name=' + this.naam).then(response => (this.meubels = response.data))
      }
    }
  },
  mounted () {
    const axios = require('axios')
    axios.get('https://meubel-backend.herokuapp.com/ping').then(response => (this.ping = response.data))
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
