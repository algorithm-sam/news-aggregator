<template>
  <div id="app">
    <nav class="navbar is-link">
      <div class="container">
          <div class="navbar-brand">
            <a class="navbar-item" href="#">
              A News Aggregator Web App
            </a>
            <div class="navbar-burger burger" data-target="navbarExampleTransparentExample">
              <span></span>
              <span></span>
              <span></span>
            </div>
          </div>
          </div>
        </nav>
    <div class="container" id="main_container">
        <select-news @newVendor="newVendor"/>

        <news :articles="articles"/>
    </div>


  </div>
</template>

<script>
import Selectnews from './components/selectNews'
import News from './components/news'

export default {
  name: 'app',
  components:{
    'select-news':Selectnews,
    'news':News

  },
  data () {
    return {
      source:'',
      articles:[]
    }
  },
  methods:{
    newVendor:function(value){
      this.source=value;
      this.$http.get('https://newsapi.org/v2/top-headlines?sources='+this.source+'&apiKey=37ea35a2fe62480e926dae101822a675')
      .then(function(response){
        return JSON.parse(response.bodyText);
      })
      .then(function(response){
        this.articles=response.articles;
      })
      .catch(function(err){
        console.log(err);
      })
    }
  }

}
</script>

<style>

  #main_container{
      margin: 0 auto;
      margin-top:60px;
  }

  .heading{
    text-align: center;
    font-size:1.2em;
    margin-bottom:1em;
  }
</style>

