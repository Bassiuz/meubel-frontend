<template class="hello">
  <div>
    <div id="flipbook">
      <div class="page">
      </div>
      <div class="page">
        <h1>Mensen zijn net meubels</h1>
        <h2>Maar zijn meubels ook net mensen?</h2>
      </div>
      <div class="page"> <input v-model="naam" placeholder="vul in" />
    <button v-on:click="searchMeubel">naam</button></div>
      <div class="page">
         <h1 v-if="meubels !== null && meubels.length > 0">Ja! {{submittedNaam}} is een meubel!</h1>
    <h1 v-if="meubels !== null && meubels.length === 0">Helaas, {{submittedNaam}} is geen meubel</h1>

    <div v-for="meubel in meubels" v-bind:key="meubel.name">
      {{ meubel.name }}
      <img :src="meubel.imageUrl" />
    </div>

      </div>
      <div class="page">
        <button v-on:click="again">Nog eens zoeken</button>
      </div>
    </div>
  </div>
</template>

<script>
window.jQuery = require('jquery')
window.$ = window.jQuery
const axios = require('axios')

export default {
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
      window.jQuery = require('jquery')
      window.$ = window.jQuery

      if (this.naam.length === 0) {
        this.submittedNaam = ''
        this.meubels = null
        alert('Voer een naam in')
      } else {
        this.submittedNaam = this.naam
        // eslint-disable-next-line no-undef
        console.log($('#flipbook'))
        // eslint-disable-next-line no-undef
        $('#flipbook').turn(
          'next'
        )
        axios
          .get(
            'https://meubel-backend.herokuapp.com/LeenBakker/Meubel?name=' +
              this.naam
          )
          .then(response => (
            this.meubels = response.data))
      }
    },
    again: function (event) {
      window.jQuery = require('jquery')
      window.$ = window.jQuery
      // eslint-disable-next-line no-undef
      console.log($('#flipbook'))
      // eslint-disable-next-line no-undef
      $('#flipbook').turn(
        'previous'
      )
    }
  },
  mounted () {
    axios
      .get('https://meubel-backend.herokuapp.com/ping')
      .then(response => (this.ping = response.data))
    // eslint-disable-next-line no-undef
    console.log($('#flipbook'))
    // eslint-disable-next-line no-undef
    $('#flipbook').turn({
      page: 2,
      width: 968,
      height: 650,
      when: {
        start: function (event, pageObject, corner) {
          if (corner != null) {
            return event.preventDefault()
          }
        }
      }
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
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
div.page{
  background:#ffffff;
}
template.hello{
  background: #f0f0f0;
}
</style>
