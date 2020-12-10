<!--
    Name: Angela McLaughlin
    Class Section: CIS-131-W01-Fall 2020
    Date: 12/5/2020
-->

<template>
    <div class="card mb-3 bg-secondary text-white" style="max-width: 540px;">
        <div class="row no-gutters">
        <div class="col-md-4">
        <img v-bind:src="'http://openweathermap.org/img/wn/' + iconprop + '@2x.png'" class="card-img-top" alt="icon">
        </div>
        <div class="col-md-8">
        <div class="card-body">
            <h5 class="card-title">{{cityprop}} Current Conditions</h5>
            <p class="card-text">Temperature: {{tempprop}}° {{degreesprop}}</p>
            <p class="card-text">Description: {{currentprop}}</p>
            <p class="card-text">Detail: {{descprop}}</p>
        </div>   
    </div>
    </div>
    </div>
</template>

<script>
export default {
    name:"Card",
    props:['cityprop','tempprop','degreesprop','currentprop','iconprop','descprop']
}
</script>