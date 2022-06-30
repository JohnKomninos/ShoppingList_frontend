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
  <button @click="handleDelete(food.id)">DELETE</button>
</template>
