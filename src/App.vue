<script setup lang="ts">
  import {ref} from 'vue'
  import {onMounted} from 'vue'
  import axios from 'axios'

//importing components
  import GET from './components/get.vue'
  import POST from './components/post.vue'
  import DELETE from './components/delete.vue'
  import EDIT from './components/edit.vue'
  import FILTER from './components/mainfilters.vue'
  import FILTERRESULTS from './components/filterresults.vue'
  import CREATESUB from './components/createsub.vue'
  import SUBSELECTION from './components/subselection.vue'
  import GETSUB from './components/getsub.vue'

//variables
  let view = ref("get")
  let page = ref("main")

  let foods = ref([])
  let subFoods = ref([])

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


  let filterResults = ref([])

//methods
  onMounted(()=>{
    axios.get('https://grocerylists-backend.herokuapp.com/api/items').then(response => foods.value = response.data)
  })

  onMounted(()=>{
    axios.get('https://grocerylists-backend.herokuapp.com/api/sublist').then(response => subFoods.value = response.data)
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

  const handleSubCreate = (index, itemQuantity) => {
    let newSubItem = ref(
        {
          name:foods.value[index].name,
          category:foods.value[index].category,
          aisle:foods.value[index].aisle,
          quantity:itemQuantity
        }
      )
      for (let i = 0; i< subFoods.value.length; i++){
        if(foods.value[index].name == subFoods.value[i].name){
          alert('this item is already in the list')
          return
        }
      }
      axios.post('https://grocerylists-backend.herokuapp.com/api/sublist', newSubItem.value)
      .then((response)=>{
        subFoods.value = [...subFoods.value, response.data]
        newSubItem.value = ref(
          {
            name:"",
            category:"",
            aisle:"",
            quantity:""
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

  const filter = (item) =>{
    filterResults.value = foods.value.filter(food => food.category == item.toLowerCase())
    view.value = "filter"
  }

  const clearFilter = () =>{
    view.value = "get"
  }

  const createSub = () =>{
    page.value = "createSub"
  }

  const getSub = () =>{
    page.value = "sub"
  }

</script>


<template>
<div v-if="page == 'createSub'">
  <SUBSELECTION :foods="foods" :handleSubCreate="handleSubCreate" :getSub="getSub"/>
</div>
<div v-if="page == 'createSub' || page == 'sub'" class="box" v-for="subFood in subFoods">
  <GETSUB :subFood="subFood"/>
</div>
<div v-if="page == 'main'">
  <h1>Master List</h1>
  <FILTER :filter="filter" :clearFilter="clearFilter"/>
  <CREATESUB :createSub="createSub"/>
  <div v-if="view == 'filter'" class="box" v-for="filterResult in filterResults">
    <FILTERRESULTS :filterResult="filterResult"/>
  </div>
  <div v-if="view == 'get'" class="box" v-for="food in foods">
    <GET :food="food"/>
    <EDIT :food="food" :handleEdit="handleEdit" :editItem="editItem"/>
    <DELETE :handleDelete="handleDelete" :food="food"/>
  </div>
  <POST :handleCreate = "handleCreate" :postNewItem = "postNewItem"/>
</div>
</template>

<style>
.box{
  border:2px solid gold;
}
</style>
