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
    border-style: none;
  }
</style>
