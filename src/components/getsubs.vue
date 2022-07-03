<script>
export default{
  props:["subFood"],
  methods : {
    sayHi(){
      console.log('hello')
    }
  },
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
