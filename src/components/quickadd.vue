<script>
export default{
  props:["addToList", "food"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <td>
    <button @click="addToList(food)">A</button>
  </td>
</template>
