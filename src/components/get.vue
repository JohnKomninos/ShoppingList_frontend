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
    <td>
      {{food.name}}
    </td>
    <td>
      {{food.category}}
    </td>
    <td>
      {{food.aisle}}
    </td>
    <td>
      {{food.listname}}
    </td>
</template>
