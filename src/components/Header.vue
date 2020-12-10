<!--
    Name: Angela McLaughlin
    Class Section: CIS-131-W01-Fall 2020
    Date: 12/5/2020
-->

<template>
    <div class="jumbotron bg-secondary text-white">
        <h1>{{theTitle}}</h1>
        <p>{{paragraph}}</p>
    </div>
</template>

<script>
export default {
    name: "Header",
    props: ['theTitle','paragraph']
}
</script>