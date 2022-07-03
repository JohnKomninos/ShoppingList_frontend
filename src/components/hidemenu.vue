<script>
export default{
  props:["menu", "toggleMenu"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <div className="menu" v-if = "menu == true">
    <button @click = "toggleMenu">Hide Menu</button>
  </div>
  <div className="menu" v-if = "menu == false">
    <button @click = "toggleMenu">Show Menu</button>
  </div>
</template>

<style>
  .menu{
    display: flex;
    width:100%;
    justify-content: center;
  }
</style>
