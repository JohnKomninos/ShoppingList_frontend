<script>
export default{
  props:["createSub", "menu"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <div v-if="menu == true" className="subview">
    <button @click="createSub">Create Shopping List</button>
  </div>
</template>

<style>
  .subview{
    display: flex;
    width:100%;
    justify-content: center;
    margin-bottom: 20px;
  }
</style>
