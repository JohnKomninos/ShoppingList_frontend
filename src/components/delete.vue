<script>
export default{
  props:["food", "handleDelete"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <td>
    <button @click="handleDelete(food.id)">X</button>
  </td>
</template>

<style>
  td button{
    cursor: pointer;
    background-color: #EA4C89;
    border-radius: 8px;
    border-style: none;
    color: #FFFFFF;
    font-size: 14px;
    font-weight: 500;
    height: 30px;
    margin: 3px;
    padding: 5px 10px;
    text-align: center;
  }
</style>
