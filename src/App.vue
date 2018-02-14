<template lang=pug>

#app
  img(src='./assets/logo.png')
  h1 PlatziMusic
  select(v-model="selectedCountry")
    option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
  spinner(v-show="loading")
  ul
    artist(v-for="a in artists" v-bind:artist="a") 


</template>

<script>
import artist from "./component/Artist.vue"
import getArtists from './api'
import Spinner from "./component/Spinner.vue"

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'Honduras', value: 'honduras'},
        {name: 'Espana', value: 'spain'}
      ],
      selectedCountry: 'argentina',
      loading:true
    }
  },
  components:{Artist:artist,Spinner},
  methods:{
    refreshArtists() {
         const self = this
         this.loading = true
         this.artists = []
          console.log('In Mounted')
          getArtists(this.selectedCountry)
            .then(function (artists) {

              self.artists = artists
              self.loading = false;

              console.log(artists)
            })
    }
  },
  mounted() {
   this.refreshArtists();
  },
  watch: {
    selectedCountry() {
      this.refreshArtists();
    }
  }
}
</script>

<style lang="stylus">
  #app
      font-family 'Avenir', Helvetica, Arial, sans-serif
      -webkit-font-smoothing antialiased
      -moz-osx-font-smoothing grayscale
      text-align center
      color red
      margin-top 60px

  h1, h2
      font-weight normal

  ul
      list-style-type none
      padding 0

  li
      display inline-block
      margin 0 10px

  a
      color #42b983
</style>
