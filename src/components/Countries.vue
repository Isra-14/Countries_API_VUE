<script>
import axios from 'axios';

export default {
  data(){
    return {
      countriesInfo: [],
      regions: [],
      region: ''
    }
  },
  computed: {
    countries: function(){
      return this.countriesInfo.filter((country) => {
        if(this.region === '' || this.region === 'Any')
          return country.name.common;
        else
          return country.region.match(this.region);
      });
    }
  },
  mounted() {
    axios.get(`https://restcountries.com/v3.1/all`)
    .then((res) =>{
      this.countriesInfo = res.data
      this.regions = res.data.map(country => country.region)
      this.regions = this.regions.filter((item, index) => this.regions.indexOf(item) === index)
    })
    .catch((err) =>{
      console.log(err)
    })
  },
  methods: {
    changeRegion(value){
      this.region = value
    }
  }
}
</script>

<template>
  <header>
    <button v-if=" region === '' || region === 'Any' " class="actual" @click="changeRegion('Any')">Any</button>
    <button v-else @click="changeRegion('Any')">Any</button>
    <button v-for="_region in regions" :class="_region === this.region ? 'actual' : ''" @click="changeRegion(_region)">{{ _region }}</button>
  </header>
  <br>
  <div class="row" v-for="i in Math.ceil(countries.length / 4)">
    <div class="card col-4" v-for="country in countries.slice((i-1) * 4, i * 4)">
      <img :src="country.flags.png" class="card-img-top">
      <div class="card-body">
        <h5 class="card-title">{{ country.name.common }}</h5>
        <p class="card-text" v-for="capital in country.capital"> {{ capital }} </p>
      </div>
    </div>
  </div>
</template>

<style>
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.card {
  background: #3a3a3a !important;
  width: 18rem; 
  margin: 2.5rem .5rem;
}

.card-body{
  text-align: center !important;
}

img {
  border-radius: 20% !important;
  object-fit: cover !important;
  width: 100px !important;
  height: 100px !important;
  display: block;
  margin-left: auto;
  margin-right: auto;
  margin-top: -50px;
}

button{
    background: #3a3a3a !important;
    color: #fff;
    border: none;
    padding: .5rem;
    border-radius: 50px;
    width: 90px;
    margin: .5rem;
    cursor: pointer;
}

button:hover{
    background: #5a5a5a !important;
}

.actual {
    color: #00ee00;
}
</style>