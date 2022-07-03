<script>
export default{
  props:["subFood", "handleSubDelete"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <td>
    <button @click="handleSubDelete(subFood.id)">DELETE</button>
  </td>
</template>
