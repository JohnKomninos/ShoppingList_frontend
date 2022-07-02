<script>
export default{
  props:["createSub"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <div className="subview">
    <button @click="createSub">Create Sub List</button>
  </div>
</template>

<style>
  .subview{
    display: flex;
    width:100%;
    justify-content: center;
  }
</style>
