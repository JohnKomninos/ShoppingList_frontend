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
  import QUICKADD from './components/quickadd.vue'
  import SEEDDATA from './components/seeddata.vue'
//variables
  let view = ref("get")
  let page = ref("main")
  let menu = ref(true)
  let edit = ref("")
  let postCategory = ref("")
  let editCategory = ref("")
  let filterItem = ref("")

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
    postCategory.value = postNewItem.value.category
    axios.post('https://grocerylists-backend.herokuapp.com/api/items', postNewItem.value)
    .then((response)=>{
      foods.value = [...foods.value, response.data]
      if(filterResults.value[0].category.toLowerCase() === postCategory.value.toLowerCase()){
      filterResults.value = [...filterResults.value, response.data]
      }
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

  const addToList = (food) =>{
    let newSubItem = ref(
        {
          name:food.name,
          category:food.category,
          aisle:food.aisle,
          quantity:1
        }
      )
      for (let i = 0; i< subFoods.value.length; i++){
        if(food.name == subFoods.value[i].name){
          alert('this item is already in the list, did you mean to adjust the quantity?')
          return
        }
      }
      axios.post('https://grocerylists-backend.herokuapp.com/api/sublist', newSubItem.value)
      .then((response)=>{
        alert(`${food.name} successfully added to list`)
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
      filterResults.value = filterResults.value.filter(food => food.id !== id)
    })
  }

  const handleSubDelete = (id) =>{
    axios.delete('https://grocerylists-backend.herokuapp.com/api/sublist/' + id)
    .then((response)=>{
      subFoods.value = subFoods.value.filter(food => food.id !== id)
    })
  }

  const handleEdit = (food) =>{
    editCategory.value = editItem.value.category
    if(editItem.value.name === undefined || editItem.value.name === "" ){
      editItem.value.name = food.name
    } if(editItem.value.category === undefined || editItem.value.category === "" ){
      editItem.value.category = food.category
    } if(editItem.value.aisle === undefined || editItem.value.aisle === ""){
      editItem.value.aisle = food.aisle
    } if(editItem.value.listname === undefined || editItem.value.listname === ""){
      editItem.value.listname = food.listname
    }
    axios.put('https://grocerylists-backend.herokuapp.com/api/items/' + food.id, editItem.value)
    .then((response)=>{
      edit.value = ""
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
      console.log(filterItem.value)
      if(filterItem.value !== ""){
      filter(filterItem.value)
      }
    })
  }

  const handleSubEdit = (subFood, subQuantity) => {
    if(subQuantity === ""){
        subQuantity = subFood.quantity
    }
    let editSubItem = ref(
        {
          name:subFood.name,
          category:subFood.category,
          aisle:subFood.aisle,
          quantity:subQuantity
        }
      )
    axios.put('https://grocerylists-backend.herokuapp.com/api/sublist/' + subFood.id, editSubItem.value)
    .then((response)=>{
      subFoods.value = subFoods.value.map((food)=>{
        return food.id !== response.data.id ? food : response.data
      })
    })
  }

  const filter = (item) =>{
    filterResults.value = foods.value.filter(food => food.category == item.toLowerCase())
    view.value = "filter"
    filterItem.value = item
  }

  const clearFilter = () =>{
    filterResults.value = foods.value
    view.value = "get"
    filterItem.value = ""
  }

  const search = (searchResults) =>{
    filterResults.value = foods.value.filter(food => food.listname.toLowerCase().includes(searchResults.toLowerCase()))
    view.value = "filter"
  }

  const setID = (iD) =>{
    editItem.value = ref(
      {
        name:"",
        category:"",
        aisle:"",
        listname:""
      }
    )
    if(edit.value === iD){
      edit.value = ""
    } else{
        edit.value = iD
    }
  }

  const toggleMenu = () =>{
    if(menu.value){
      menu.value = false
    } else{
      menu.value = true
    }
  }

  const quickSeed = () =>{
    
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
</script>


<template>
  <div class="flex-parent">
    <HEADER :main="main" :getSub="getSub"/>
    <div className="main-flex" v-if="page == 'main'">
      <h1 className="page-title">Master List</h1>
      <HIDEMENU :menu="menu" :toggleMenu="toggleMenu"/>
      <SEEDDATA :quickSeed="quickSeed"/>
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
          <th><h2>Quick Add</h2></th>
        </tr>
        <tr v-if="view == 'filter'" v-for="filterResult in filterResults">
          <FILTERRESULTS :filterResult="filterResult"/>
          <EDIT :food="filterResult" :handleEdit="handleEdit" :editItem="editItem" :edit="edit" :setID="setID"/>
          <DELETE :handleDelete="handleDelete" :food="filterResult"/>
          <QUICKADD :addToList="addToList" :food="filterResult"/>
        </tr>
        <tr v-if="view == 'get'" class="box" v-for="food in foods">
          <GET :food="food"/>
          <EDIT :food="food" :handleEdit="handleEdit" :editItem="editItem" :edit="edit" :setID="setID"/>
          <DELETE :handleDelete="handleDelete" :food="food"/>
          <QUICKADD :addToList="addToList" :food="food"/>
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
        <GETSUB :subFood="subFood" :handleSubEdit="handleSubEdit"/>
        <DELETESUB :handleSubDelete="handleSubDelete" :subFood="subFood"/>
      </tr>
    </table>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Caveat:wght@500&display=swap');

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
    /* border: 2px solid black; */
    text-align: center;
    border-collapse: collapse;
    margin-bottom: 100px;
  }

  th{
    border: 2px solid black;
    width:120px;
    font-size: 20px;
  }

  td{
    border: 2px solid black;
    text-align: center;
    padding:0;
    margin:0;
    font-size: 30px;
  }

  body{
    background-color: rgb(189, 232, 239);
    color: rgb(255, 0, 127);
    font-family: 'Caveat', cursive;
  }

  .button-design{
    cursor: pointer;
    background-color: #EA4C89;
    border-radius: 8px;
    border-style: none;
    color: #FFFFFF;
    font-size: 14px;
    font-weight: 500;
    height: 40px;
    margin: 3px;
    padding: 10px 16px;
    text-align: center;
  }

  input[type = text], input[type = number]{
    border-radius: 8px;
    border-style: none;
    font-size: 14px;
    height: 30px;
    margin:5px;
    text-align: center;
  }
</style>
