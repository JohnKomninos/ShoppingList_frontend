<script>
export default{
  props:["subFood"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <td>
    {{subFood.name}}
  </td>
  <td>
    {{subFood.category}}
  </td>
  <td>
    {{subFood.aisle}}
  </td>
  <td>
    {{subFood.quantity}}
  </td>
</template>
