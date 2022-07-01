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
  <button @click="handleSubDelete(subFood.id)">DELETE</button>
</template>
