<script>
export default{
  props:["main", "getSub"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <div class="header">
    <div @click="main">
      <h1 className="cursor">Master List</h1>
    </div>
    <div @click="getSub">
      <h1 className="cursor">Shopping List</h1>
    </div>
  </div>
</template>

<style>
  .header{
    display: flex;
    width:100%;
    justify-content: space-around;
    border-bottom: solid white;
    background-color: lightgray;
    margin-bottom: 20px;
    background-color: rgb(189, 232, 239)
  }

  .cursor{
    cursor: pointer;
  }
</style>
