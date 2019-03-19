<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 Platzimusic
    select(v-model="selectedContry")
      option(v-for="contry in contries" v-bind:value="contry.value") {{contry.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import getArtits from './api';
import artist from './components/artist.vue'
import Spinner from './components/Spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      contries:[
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'España', value: 'spain'},
        {name: 'Mexico', value: 'mexico'}
      ],
      selectedContry: 'argentina',
      loading: true
    }
  },
  components:{
    Artist: artist,
    Spinner
  },
  methods:{
    refreshArtists: function(){
      const self = this
      this.loading = true
      this.artists = []
      getArtits(this.selectedContry)
      .then(function (artists){
        self.artists = artists
        self.loading = false
      })
    }
  },
  mounted: function (){
    this.refreshArtists()
  },
  watch: {
    selectedContry: function (){
      this.refreshArtists()
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
  color #2c3e50
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
