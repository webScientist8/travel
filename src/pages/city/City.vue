<template>
    <div>
      <city-header></city-header>
      <city-search></city-search>
      <city-list
        :cities="cities"
        :hot="hotCities"
        :letter="letter"
      ></city-list>
      <city-aiphabet
        :cities="cities"
        @change="handleLetterChange"
      ></city-aiphabet>
    </div>
</template>

<script>
  import axios from 'axios'
  import CityHeader from './components/Header'
  import CitySearch from './components/Search'
  import CityList from './components/List'
  import CityAiphabet from './components/Aiphabet'
  export default {
    name: 'City',
    components: {
      CityHeader,
      CitySearch,
      CityList,
      CityAiphabet
    },
    data () {
      return {
        cities: {},
        hotCities: [],
        letter: ''
      }
    },
    methods: {
      getCityInfo () {
        axios.get('/api/city.json')
          .then(this.handleGetCityInfoSucc)
      },
      handleGetCityInfoSucc (res) {
        res = res.data
        if (res.ret && res.data) {
          const data = res.data
          this.cities = data.cities
          this.hotCities = data.hotCities
        }
      },
      handleLetterChange (letter) {
        this.letter = letter
      }
    },
    mounted () {
      this.getCityInfo()
    }
  }
</script>

<style lang="stylus" scoped>
  @import '~styles/varibles.styl'
  .search
    height: .72rem
    padding: 0 .1rem
    background: $bgColor
    .search-input
      box-sizing: border-box
      width: 100%
      height: .62rem
      padding: 0 .1rem
      line-height: .62rem
      text-align: center
      border-radius: .06rem
      color: #666
  .search-content
    z-index: 1
    overflow: hidden
    position: absolute
    top: 1.58rem
    left: 0
    right: 0
    bottom: 0
    background: #eee
    .search-item
      line-height: .62rem
      padding-left: .2rem
      background: #fff
      color: #666
</style>
