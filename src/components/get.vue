<script>
export default{
  props:["food"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <h2>
    Name:{{food.name}}<br/>
    Category:{{food.category}}<br/>
    Aisle:{{food.aisle}}<br/>
    ID:{{food.id}}
  </h2>
</template>
