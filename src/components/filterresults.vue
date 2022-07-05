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
  <td>
    {{filterResult.name}}
  </td>
  <td>
    {{filterResult.category}}
  </td>
  <td>
    {{filterResult.aisle}}
  </td>
  <td>
    {{filterResult.listname}}
  </td>
</template>

<style>


</style>
