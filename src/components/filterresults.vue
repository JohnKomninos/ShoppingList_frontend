<script>
export default{
  props:["filterResult"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <h2>
    Name:{{filterResult.name}}<br/>
    Category:{{filterResult.category}}<br/>
    Aisle:{{filterResult.aisle}}<br/>
    ListName:{{filterResult.listname}}
  </h2>
</template>
