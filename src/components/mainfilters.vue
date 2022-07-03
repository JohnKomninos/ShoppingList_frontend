<script>
export default{
  props:["filter", "clearFilter", "search", "menu"],
  data(){
    return{
      searchResults : ""
    }
  }
}
</script>

<template>
  <div v-if=" menu == true" className="mainfilters">
    <div className="filter">
      <h2 >
        Filter by:
        <button @click="filter('Grain')">Grain</button>
        <button @click="filter('Meat')">Meat</button>
        <button @click="filter('Vegetable')">Vegetable</button>
        <button @click="clearFilter">Clear Filter</button><br/>
      </h2>
    </div>
    <div>
      <h2 className="search">
        <form @submit.prevent="search(searchResults)">
          Search by List:
          <input type="text" v-model="searchResults">
          <input type="submit">
        </form>
      </h2>
    </div>
  </div>
</template>

<style>
  .mainfilters{
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    justify-content: center;
    align-content: space-between;
  }

  .filter{
    text-align: center;
    width:100%;
  }

  .search{
    text-align: center;
    width:100%;
  }
</style>
