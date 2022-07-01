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
  <h2>
    Name:{{subFood.name}}<br/>
    Category:{{subFood.category}}<br/>
    Aisle:{{subFood.aisle}}<br/>
    Quantity:{{subFood.quantity}}
  </h2>
</template>
