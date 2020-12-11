<!--
    Name: Angela McLaughlin
    Class Section: CIS-131-W01-Fall 2020
    Date: 12/5/2020
-->

<template>
  <div id="app" class="container">
    <Header theTitle="Current Weather" paragraph="Enter a valid five digit US Zip Code, select Celsius or Fahrenheit, and click 'Current Weather' button to see the current weather for the location."/>
    <!-- Entry Form -->
    <form>
      <div class="form-row align-items-center">
        <div class="col-auto">
          <label class="sr-only" for="inlineFormInput">Zip Code</label>
          <input type="text" class="form-control mb-2" id="inlineFormInput" placeholder="Zip Code" v-model="zip">
        </div>
        <div class="col-auto">
          <div class="form-check">
            <input class="form-check-input" type="radio" name="degrees" id="F" value="F" v-model="degrees" v-on:change="updateTemp()">
            <label class="form-check-label" for="F">
              Fahrenheit
            </label>
          </div>
          <div class="form-check">
            <input class="form-check-input" type="radio" name="degrees" id="C" value="C" v-model="degrees" v-on:change="updateTemp()">
            <label class="form-check-label" for="C">
              Celsius
            </label>
          </div>
        </div>
        <div class="col-auto">
          <button type="button" class="btn mb-2" id="button" v-on:click="validateZip()">Current Weather</button>
        </div>
      </div>
      <!-- Error Message -->
      <div v-if="error !== null">
        <h5 style="color:red">{{error}}</h5>
      </div>
    </form>
    <div v-if="error === null && temp !== null">
    <Card v-bind:cityprop="city" v-bind:degreesprop="degrees" v-bind:tempprop="temp" v-bind:currentprop="current" v-bind:iconprop="icon" v-bind:descprop="description"/>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Card from './components/Card.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Header,
    Card
  },
  data(){
    return{
      tempKelvin: null,
      city: "",
      current: "",
      description:"",
      icon:"",
      zip: "",
      degrees: "F",
      temp: null,
      error: null,
      regex: /\b\d{5}\b/
    }
  },
  methods: {
    //Validates Zip code before API call
    validateZip(){

        if(this.zip === ""){
            this.error = "Please enter a zip code."
        }
        else if (this.regex.test(this.zip) === true){
            this.error = null
            this.apiCall()
        }
        else{
          this.error = "Zip code must be five digits."
        }
    },

    //Toggle from F to C changes already displayed temp and called during inital api
    updateTemp(){
        //Convert from Kelvin to selection on radio button
        if(this.error === null && this.tempKelvin !== null){
            if(this.degrees==="F"){
                this.temp = ((this.tempKelvin - 273.15) * (9/5) + 32).toFixed(0)
            }
            else{
                this.temp = (this.tempKelvin - 273.15).toFixed(0)
            }
        }
    },
    
    apiCall(){
    this.temp = null
    //Use Axios to grab weather using zip entered in form
    axios.get("https://api.openweathermap.org/data/2.5/weather?zip=" + this.zip +",us&appid=c234a81ae7d1d5eba19ea29fb50f695e")
    .then((response) => {
        //Place weather data into variables
        this.tempKelvin = response.data.main.temp
        this.city = response.data.name
        this.current = response.data.weather[0].main
        this.description = response.data.weather[0].description
        this.icon = response.data.weather[0].icon
        this.updateTemp() //Convert temp
    //catches error with api call returns the message that the zip is invalid
    }).catch((err) => {
        console.log(err)
        this.error = "Zip code is not valid."
    })
    },
  }
}
</script>

<style>
body{
  background-image: url('./assets/background.jpg');
  background-size: cover;
}
#button{
  background-color:rgb(166, 57, 196);
  color: white;
}
</style>
