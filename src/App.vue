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
  import CREATESUB from './components/subview.vue'
  import SUBSELECTION from './components/subselections.vue'
  import GETSUB from './components/getsubs.vue'
  import DELETESUB from './components/deletesubs.vue'
  import HEADER from './components/header.vue'
  import HIDEMENU from './components/hidemenu.vue'
//variables
  let view = ref("get")
  let page = ref("main")
  let menu = ref(true)

  let foods = ref([])
  let subFoods = ref([])

  let postNewItem = ref(
      {
        name:"",
        category:"",
        aisle:"",
        listname:""
      }
    )

    let editItem = ref(
    {
      name:"",
      category:"",
      aisle:"",
      listname:""
    }
  )


  let filterResults = ref([])


//methods
  onMounted(()=>{
    axios.get('https://grocerylists-backend.herokuapp.com/api/items').then(response => foods.value = filterResults.value = response.data)
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
          aisle:"",
          listname:""
        }
      )
    })
  }

  const handleSubCreate = (index, itemQuantity) => {
    let newSubItem = ref(
        {
          name:filterResults.value[index].name,
          category:filterResults.value[index].category,
          aisle:filterResults.value[index].aisle,
          quantity:itemQuantity
        }
      )
      for (let i = 0; i< subFoods.value.length; i++){
        if(filterResults.value[index].name == subFoods.value[i].name){
          alert('this item is already in the list, did you mean to adjust the quantity?')
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

  const handleSubDelete = (id) =>{
    axios.delete('https://grocerylists-backend.herokuapp.com/api/sublist/' + id)
    .then((response)=>{
      subFoods.value = subFoods.value.filter(food => food.id !== id)
    })
  }

  const handleEdit = (id) =>{
    axios.put('https://grocerylists-backend.herokuapp.com/api/items/' + id, editItem.value)
    .then((response)=>{
      editItem.value = ref(
        {
          name:"",
          category:"",
          aisle:"",
          listname:""
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
    filterResults.value = foods.value
    view.value = "get"
  }

  const search = (searchResults) =>{
    filterResults.value = foods.value.filter(food => food.listname.toLowerCase().includes(searchResults.toLowerCase()))
    view.value = "filter"
  }

//pages

  const main = () =>{
    page.value = "main"
  }

  const createSub = () =>{
    page.value = "createSub"
  }

  const getSub = () =>{
    page.value = "sub"
  }

  const toggleMenu = () =>{
    if(menu.value){
      menu.value = false
    } else{
      menu.value = true
    }
  }
</script>


<template>
  <div class="flex-parent">
    <HEADER :main="main" :getSub="getSub"/>
    <div className="main-flex" v-if="page == 'main'">
      <h1 className="page-title">Master List</h1>
      <HIDEMENU :menu="menu" :toggleMenu="toggleMenu"/>
      <FILTER :filter="filter" :clearFilter="clearFilter" :search="search" :menu="menu"/>
      <CREATESUB :createSub="createSub" :menu="menu"/>
      <POST :handleCreate = "handleCreate" :postNewItem = "postNewItem" :menu="menu"/>
      <table>
        <tr>
          <th><h2>Name</h2></th>
          <th><h2>Category</h2></th>
          <th><h2>Aisle</h2></th>
          <th><h2>List Name</h2></th>
          <th><h2>Edit Item</h2></th>
          <th><h2>Delete</h2></th>
        </tr>
      <tr v-if="view == 'filter'" v-for="filterResult in filterResults">
          <FILTERRESULTS :filterResult="filterResult"/>
      </tr>
          <tr v-if="view == 'get'" class="box" v-for="food in foods">
            <GET :food="food"/>
            <EDIT :food="food" :handleEdit="handleEdit" :editItem="editItem"/>
            <DELETE :handleDelete="handleDelete" :food="food"/>
          </tr>
      </table>
    </div>
    <div v-if="page == 'createSub'">
      <SUBSELECTION :foods="foods" :filterResults="filterResults" :handleSubCreate="handleSubCreate" :getSub="getSub"/>
    </div>
    <h1 v-if="page == 'createSub' || page == 'sub'" className="page-title">Shopping List</h1>
    <table v-if="page == 'createSub' || page == 'sub'">
      <th><h2>Name</h2></th>
      <th><h2>Category</h2></th>
      <th><h2>Aisle</h2></th>
      <th><h2>Quantity</h2></th>
      <th><h2>Delete</h2></th>
      <tr class="box" v-for="subFood in subFoods">
        <GETSUB :subFood="subFood"/>
        <DELETESUB :handleSubDelete="handleSubDelete" :subFood="subFood"/>
      </tr>
    </table>
  </div>
</template>

<style>
.flex-parent{
  display: flex;
  flex-wrap: wrap;
  width:100%;
  justify-content: center;
}

.main-flex{
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.page-title{
  width:100%;
  text-align: center;
}


.item-detail{
  margin-left: 110px;
}

table{
  border: 2px solid black;
  text-align: center;
  border-collapse: collapse;
}

th{
  border: 2px solid black;
  width:150px;
}

td{
  border: 2px solid black;
  text-align: center;
  padding:0;
  margin:0;
}

</style>
