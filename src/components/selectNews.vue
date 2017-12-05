<template>
    <div class="container is-semi-fluid">
      <div class="notification">
        <p class="heading"> <i class="fa fa-list"></i> Select Your News Vendor</p>
        <div class="field has-addons">
  <div class="control is-expanded">
    <div class="select is-fullwidth">
      <select name="country" @change="sourceChanged">
        <option v-for="(source,index) in sources" :key="index" :value="source.name">{{source.name}} </option>
      </select>
    </div>
  </div>

  <div class="control">
    <button type="submit" class="button is-primary">Get News</button>
  </div>
</div>
<div v-if="source" class="selectFooter">
  <p class="vendor-information">
    {{ source.description }}
  </p>
  <p class="controls"> <a class="button is-primary" target="_blank" :href="source.url"> Visit Vendor's Website</a> </p>
  </div>
</div>
      </div>
    </div>

</template>

<script>
export default {
  name: 'selectNews',
  data () {
    return {
      description:'',
      sources:[],
      source:''
    }
  },
  methods:{
    // getallcomponents:function(evt){
    //     sources.
    // },

    sourceChanged:function(evt){
      // console.log(evt);
      // console.log(evt.target.value);
      var that=this;
       Object.values(this.sources).map(function(elem,index){
            if(elem.name==evt.target.value){
              that.source=elem;
           }
       });
      this.$emit('newVendor',this.source.id);
    }
  },
    created:function(){
    this.$http.get('https://newsapi.org/v1/sources?language=en')
    .then(function(response){
      if(response.body.status!='ok'){
        throw new Error('Could not fetch the list of supported sources');
      }
      return JSON.parse(response.bodyText);
    }).then(function(response){
        this.sources=response.sources;
    }).catch(function(err){
      console.log(err.stack);
    });
  }
}
</script>

<style>

.is-semi-fluid{
      margin-bottom: 20px;
  }

p.controls{
  text-align: center;
  margin-top:20px;
}
@media (min-width: 600px){
  .is-semi-fluid{
      width:70%;
  }
}

</style>
