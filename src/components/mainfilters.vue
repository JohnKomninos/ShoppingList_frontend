<script>
export default{
  props:["filter", "clearFilter"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <h2>
    Filter by:
    <button @click="filter('Grain')">Grain</button>
    <button @click="filter('Meat')">Meat</button>
    <button @click="filter('Vegetable')">Vegetable</button>
    <button @click="clearFilter">Clear Filter</button>
  </h2>
</template>
