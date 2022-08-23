<script>
export default{
  props:["filter", "clearFilter", "search", "menu"],
  methods:{
    searchAndClear(searchResults){
        this.search(searchResults)
        this.searchResults = ""
    }
  },
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
        <button className="button-design" @click="filter('Grain')">Grain</button>
        <button className="button-design" @click="filter('Meat')">Meat</button>
        <button className="button-design" @click="filter('Vegetable')">Vegetable</button>
        <button className="button-design" @click="clearFilter">Clear Filter</button><br/>
      </h2>
    </div>
    <div>
      <h2 className="search">
        <form @submit.prevent="searchAndClear(searchResults)">
          Search by List:
          <input type="text" v-model="searchResults">
          <input className="button-design" type="submit">
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
