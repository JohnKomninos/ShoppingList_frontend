<script>
export default{
  props:["filter", "clearFilter", "search"],
  data(){
    return{
      searchResults : ""
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
    <button @click="clearFilter">Clear Filter</button><br/>
    Search by List Name:
    <form @submit.prevent="search(searchResults)">
      <input type="text" v-model="searchResults">
      <input type="submit">
    </form>
  </h2>
</template>
