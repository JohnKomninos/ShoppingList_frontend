<script setup lang="ts">
import {ref} from 'vue'
import {onMounted} from 'vue'
import axios from 'axios'

//importing components
import GET from './components/get.vue'
import POST from './components/post.vue'

//variables
let foods = ref([])

//methods
onMounted(()=>{
    axios.get('https://grocerylists-backend.herokuapp.com/api/items').then(response => foods.value = response.data)
  })

</script>

<template>
<h1>Master List</h1>
  <div v-for="food in foods">
    <GET :food="food"/>
  </div>
  <POST :handleCreate = "handleCreate" :postNewItem = "postNewItem"/>
</template>

<style>
</style>
