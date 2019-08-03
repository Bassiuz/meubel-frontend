<template class="hello">
  <div>
    <div id="flipbook" class="flipbook">
      <div class="page">
      </div>
      <div class="frontpage">
        <h1>Mensen zijn net meubels</h1>
        <h2>Maar zijn meubels ook net mensen?</h2>
      </div>
      <div class="page"> <input v-model="naam" placeholder="vul in" />
        <button v-on:click="searchMeubel">naam</button>
      </div>
      <div class="page">
          <h1 v-if="meubels !== null && meubels.length > 0">Ja! {{submittedNaam}} is een</h1>
          <h1 v-if="meubels !== null && meubels.length === 0">Helaas, {{submittedNaam}} is geen meubel</h1>
          <div v-for="meubel in meubels.slice(0,1)" v-bind:key="meubel.name">
              {{ meubel.name }}
              <img :src="meubel.imageUrl"/>
          </div>
      </div>
      <div class="page">
        <button v-on:click="again">Nog eens zoeken</button>
        <h3 v-if="meubels !== null && meubels.length > 0">Nee, {{submittedNaam}} is toch meer een: </h3>
        <div v-for="meubel in meubels.slice(1)" v-bind:key="meubel.name">
            {{ meubel.name }}
            <img :src="meubel.imageUrl" />
         </div>
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
      meubels: []
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
      $('#flipbook').turn(
        'previous'
      )
    },
    resizeHandler: function (event) {
      console.log('test')
      window.jQuery = require('jquery')
      window.$ = window.jQuery
      // eslint-disable-next-line no-undef
      $('#flipbook').turn(
        'size', window.innerWidth * 0.80, window.innerHeight * 0.80
      )
    }
  },
  created () {
    window.addEventListener('resize', this.resizeHandler)
  },
  destroyed () {
    window.removeEventListener('resize', this.resizeHandler)
  },
  mounted () {
    axios
      .get('https://meubel-backend.herokuapp.com/ping')
      .then(response => (this.ping = response.data))
    // eslint-disable-next-line no-undef
    $('#flipbook').turn({
      page: 2,
      width: window.innerWidth * 0.80,
      height: window.innerHeight * 0.80,
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
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
h1{
  font-size: 6vw;
  color: #FFDA1A;
  text-align: left;
  padding: 20px;
}
h2{
  font-size: 4vw;
  color: #f0f0f0;
  text-align: right;
  padding: 20px;
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
  background:#f0f0f0;
}
div.frontpage{
  background:#0051BA;
}
div.flipbook
{
  overflow: hidden;
  margin: 0 auto;
}
</style>
