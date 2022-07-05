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
    <button className="button-design" @click = "toggleMenu">Hide Menu</button>
  </div>
  <div className="menu" v-if = "menu == false">
    <button className="button-design" @click = "toggleMenu">Show Menu</button>
  </div>
</template>

<style>
  .menu{
    display: flex;
    width:100%;
    justify-content: center;
    margin-bottom: 10px;
  }
</style>
