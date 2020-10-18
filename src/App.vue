<template>
  <div id="app">
    <div :id="main">
      <img :src="imageSrc" alt="" :id="searchEngineLogo">
      <div :id="searchBar">
        <input :id="inputBox" v-model="searchQuery" placeholder="Enter what ever you want about Cities ..." @keyup.enter="search">
        <img :src="searchIcon" alt="searchIcon" :id="searchImg" @click="search" class="imgPointer">
      </div>
      <!-- <div id="divider" v-if="!showIt"></div> -->
      <p v-if="showIt"><b id="boldInfo">This website is a "Search Engine" based on Space Vector Model.</b><br> designed and developed by : Javad Heyrani, Ebrahim Yaali<br><br>Autumn 2020</p>
    </div>
    <div id="searchResults" v-if="!showIt">
      <h1 v-if="noResult" id="noResult">We can't found any result.</h1>
      <div id="result" v-for="(item , index) in searchResult" v-bind:key="index">
        <h4>{{ item.key }}</h4>
        <h6>Similarity : {{ item.similarity }}%</h6>
        <p>{{ item.content }}</p>
        <!-- <div id="divider" style="width: 50vw; margin-top: 20px;"></div> -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data: () => {
    return {
      showIt: true,
      noResult: false,
      searchQuery: '',
      searchResult: {},
      imageSrc: require('./assets/SearchEngineLogo.svg'),
      searchIcon: require('./assets/searchIcon.svg'),
      main: 'main',
      searchEngineLogo: 'searchEngineLogo',
      searchBar: 'searchBar',
      inputBox: 'inputBox',
      searchImg: 'searchImg'
    }
  },
  methods: {
    search () {
      this.showIt = false
      this.imageSrc = require('./assets/SearchEngineLogoResult.svg')
      this.searchIcon = require('./assets/loading.gif')
      this.main = 'mainResult'
      this.searchEngineLogo = 'searchEngineLogoResult'
      this.searchBar = 'searchBarResult'
      this.inputBox = 'inputBoxResult'
      this.searchImg = 'searchImgResult'
      axios.post('/', { query: this.searchQuery })
        .then((res) => {
          this.searchResult = res.data
          this.searchResult.sort((a, b) => a.similarity - b.similarity)
          this.searchResult.reverse()
          this.searchIcon = require('./assets/searchIcon.svg')
          this.noResult = false
          if (this.searchResult.length === 0) {
            this.noResult = true
          }
        }).catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>

<style lang="scss">
html , body {
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif, Helvetica, Arial;
}
#main {
  position: relative;
  top: calc(35vh - 160px);
}
p {
  position: relative;
  display: block;
  top: 15px;
  text-align: center;
  color: #474747;
  font-size: 0.7rem;
}
#boldInfo {
  font-size: 0.8rem;
}
#searchEngineLogo {
  width: 330px;
  height: 230px;
  margin: 0px auto;
  display: block;
}
#searchBar {
  max-width: 550px;
  min-width: 350px;
  margin-top: -10px;
  margin-right: auto;
  margin-left: auto;
  border: 1px solid #dfdfdf;
  border-radius: 100px;
}
#inputBox {
  width: calc(100% - 92px);
  height: 20px;
  top: 0;
  left: 0;
  border-radius: 5px;
  display: inline-block;
  border: none;
  padding: 10px 22px;
  font-size: 0.9rem;
  color: #2b2b2b;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, Helvetica, Arial, sans-serif;
  background-color: #f5f5f500;
  vertical-align: middle;
}
#searchImg {
  width: 17px;
  height: 17px;
  margin: 0px 10px;
  top: 0;
  right: 0;
  display: inline-block;
  vertical-align: middle;
}
.imgPointer:hover,
.imgPointer:active {
  cursor: pointer;
}
input:focus{
  outline: none;
  background-color: #ffffff00 !important;
}
//////////////////////////////// On Result
#mainResult {
  position: relative;
}
#searchEngineLogoResult {
  width: 300px;
  margin: 20px 30px;
  display: block;
}
#searchBarResult {
  max-width: 600px;
  min-width: 350px;
  margin-left: 22px;
  border: 1px solid #dfdfdf;
  border-radius: 100px;
  margin-top: -5px;
}
#inputBoxResult {
  width: calc(100% - 92px);
  height: 15px;
  top: 0;
  left: 0;
  border-radius: 5px;
  display: inline-block;
  border: none;
  padding: 10px 22px;
  font-size: 0.9rem;
  color: #2b2b2b;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, Helvetica, Arial, sans-serif;
  background-color: #f5f5f500;
  vertical-align: middle;
}
#searchImgResult {
  width: 17px;
  height: 17px;
  margin: 0px 10px;
  top: 0;
  right: 0;
  display: inline-block;
  vertical-align: middle;
}
#divider {
  height: 0.7px;
  width: 97.7vw;
  margin-top: 30px;
  background-color: #dfdfdf;;
}
//////////////////////////////// Results
#searchResults {
  position: relative;
  padding: 0px 10px;
  margin: 40px 10px;
}
#result {
  width: 75vw;
  padding: 20px 30px;
  border-radius: 7px;
  text-align: left;
  color: #2b2b2b;
  background-color: #fbfbfb;
  border: 1px solid #dfdfdf;
  margin-top: 15px;
}
#result h4 {
  margin: 0px;
  display: inline-block;
  color: #3989f1;
  font-size: 1.3rem;
}
#result p {
  margin: 0px;
  display: block;
  text-align: left;
  padding-bottom: 20px;
  color: #525252;
  font-size: 0.8rem;
}
#result h6 {
  margin: 3px;
  text-align: right;
  display: inline-block;
  float: right;
  color: #fd6a6a;
  font-size: 0.8rem;
}
#noResult {
  color: #ffcccc;
  position: relative;
  margin: 35px 20px;
  font-size: 3rem;
}
</style>
