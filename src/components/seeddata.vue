<script>
export default{
  props:['quickSeed', 'menu'],
  data(){
    return{

    }
  }
}
</script>

<template>
  <button v-if="menu == true" @click="quickSeed" class="button-design">Quick Seed</button>
</template>
