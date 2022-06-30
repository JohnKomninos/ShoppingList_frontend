<script>
export default{
  props:[],
  data(){
    return{
      namess : [{name:"John"}]
    }
  }
}
</script>

<template>
  <h2>hello</h2>
  {{namess[0].name}}
</template>
