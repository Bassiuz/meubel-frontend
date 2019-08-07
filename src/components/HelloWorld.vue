<template class="hello">
  <div>
    <div id="flipbook" class="flipbook">
      <div class="page">
      </div>
      <div class="frontpage">
        <h1>Mensen zijn net meubels</h1>
        <h2>Maar zijn meubels ook net mensen?</h2>
      </div>
      <div class="page">
        <h3>Welk meubel ben jij?</h3>
        <input v-model="naam" placeholder="vul in" />
        <button v-on:click="searchMeubel">naam</button>
        <button v-on:click="randomName">doe maar wat</button>
         <div v-for="meubel in randomMeubels" v-bind:key="meubel.name" class="small-item">
            {{ meubel.name }}
            <img class='small-image' :src="meubel.imageUrl" />
         </div>
      </div>
      <div class="page">
          <h1 v-if="meubels !== null && meubels.length > 0">Ja! {{submittedNaam}} is een</h1>
          <h1 v-if="meubels !== null && meubels.length === 0">Helaas, {{submittedNaam}} is geen meubel</h1>
          <div v-for="meubel in meubels.slice(0,1)" v-bind:key="meubel.name">
              {{ meubel.name }}
              <img class='big-image' :src="meubel.imageUrl"/><br/>
              <button v-on:click="scoreMeubel(meubel.name)">cool, helemaal mee eens</button>
          </div>
      </div>
      <div class="page">
        <button v-on:click="again">Nog eens zoeken</button>
        <h3 v-if="meubels !== null && meubels.length > 0">Nee, {{submittedNaam}} is toch meer een: </h3>
        <div v-for="meubel in meubels.slice(1)" v-bind:key="meubel.name" class="small-item">
            <img class='small-image' :src="meubel.imageUrl" />

            <div class="name-button-box">
               {{ meubel.name }}
              <button v-on:click="scoreMeubel(meubel.name)">nee, dit past beter</button>
            </div>
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
      meubels: [],
      randomMeubels: []
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
        $('#flipbook').turn(
          'next'
        )
        this.searchName(this.naam)
      }
    },
    searchName: function (name) {
      axios.get(
        'https://meubel-backend.herokuapp.com/Meubel/getAll?name=' + name)
        .then(response => (this.meubels = response.data))
    },
    scoreMeubel: function (meubelName) {
      axios.get(
        'https://meubel-backend.herokuapp.com/Score/scoreByNameAndIndex?name=' + this.naam + '&meubelName=' + meubelName)
        .then(this.again)
    },
    processRandomName: function (name) {
      this.naam = name
      this.submittedNaam = name
      this.searchName(this.naam)
    },
    randomName: function (event) {
      window.jQuery = require('jquery')
      window.$ = window.jQuery
      // eslint-disable-next-line no-undef
      $('#flipbook').turn(
        'next'
      )
      axios
        .get(
          'https://meubel-backend.herokuapp.com/Meubel/getRandomName'
        )
        .then(response => this.processRandomName(response.data))
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
      .get('https://meubel-backend.herokuapp.com/Meubel/getTop5')
      .then(response => (this.randomMeubels = response.data))
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
div.small-item
{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 1vh;
}
div.name-button-box
{
  display: flex;
  flex-direction: column;
  justify-content: center;
}
img.small-image
{
   width:100%;
   max-width:8vw;
   height:100%;
   max-height:8vw;
}
img.big-image
{
   width:100%;
   max-width:20vw;
   height:100%;
   max-height:20vw;
}
</style>
