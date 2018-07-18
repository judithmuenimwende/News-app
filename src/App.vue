<template>
  <div class="container">

    <h1>Trending news</h1>

    <input v-model="search" type="text" name="search" placeholder="Search..">
    <button v-on:click="loadNews" type="button">Search</button>

    <p v-if="loading">Please wait...</p>
    
    <div v-for="article in news">
      
      
      <img v-if="article.urlToImage" :src="article.urlToImage" alt="">

      <h3>{{article.title}} </h3>

      <h4>{{article.author}}</h4>

      <a class="button" :href="article.url" target="_blank">Read more</a>

    </div>

  </div>
</template>


<script>

import axios from 'axios'

const baseUrl = "https://newsapi.org/v2";
const apikey = "8398f125526a43beaaf82b35de2ef3c6";
const endpoint = "/everything";

const data = {
  news: [],
  search:'',
  loading:false
}



export default {
  data: function() {
    return data 
  },

  created() {
    this.loadNews();
  },

  methods: {
      loadNews() {
        if(this.search.length < 1) {
          return;
        }

        this.loading=true;

        this.news = [];

      let url = baseUrl + endpoint + '?q=' + this.search + '&apikey=' + apikey;



      axios.get(url).then(function(response) {
        console.log(response.data.articles)
        data.loading=false;
        data.news = response.data.articles
        
        
      }).catch(function(error) {
          console.log(error.message)
      })
    }
  }
}
</script>
