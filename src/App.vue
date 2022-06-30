<script setup lang="ts">
  import {ref} from 'vue'
  import {onMounted} from 'vue'
  import axios from 'axios'

//importing components
  import GET from './components/get.vue'
  import POST from './components/post.vue'
  import DELETE from './components/delete.vue'
  import EDIT from './components/edit.vue'

//variables
  let foods = ref([])

  let postNewItem = ref(
      {
        name:"",
        category:"",
        aisle:""
      }
    )

    let editItem = ref(
    {
      name:"",
      category:"",
      aisle:""
    }
  )
//methods
  onMounted(()=>{
    axios.get('https://grocerylists-backend.herokuapp.com/api/items').then(response => foods.value = response.data)
  })

  const handleCreate = () =>{
    axios.post('https://grocerylists-backend.herokuapp.com/api/items', postNewItem.value)
    .then((response)=>{
      foods.value = [...foods.value, response.data]
      postNewItem.value = ref(
        {
          name:"",
          category:"",
          aisle:""
        }
      )
    })
  }

  const handleDelete = (id) =>{
    axios.delete('https://grocerylists-backend.herokuapp.com/api/items/' + id)
    .then((response)=>{
      foods.value = foods.value.filter(food => food.id !== id)
    })
  }

  const handleEdit = (id) =>{
    axios.put('https://grocerylists-backend.herokuapp.com/api/items/' + id, editItem.value)
    .then((response)=>{
      editItem.value = ref(
        {
          name:"",
          category:"",
          aisle:""
        }
      )
      foods.value = foods.value.map((food)=>{
        return food.id !== response.data.id ? food : response.data
      })
    })
  }
</script>


<template>
<h1>Master List</h1>
  <div class="box" v-for="food in foods">
    <GET :food="food"/>
    <EDIT :food="food" :handleEdit="handleEdit" :editItem="editItem"/>
    <DELETE :handleDelete="handleDelete" :food="food"/>
  </div>
  <POST :handleCreate = "handleCreate" :postNewItem = "postNewItem"/>
</template>

<style>
.box{
  border:2px solid gold;
}
</style>
