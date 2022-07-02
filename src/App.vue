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
//variables
  let view = ref("get")
  let page = ref("main")

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
      aisle:""
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
    console.log(foods)
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

</script>


<template>
  <div class="flex-parent">
    <HEADER :main="main" :getSub="getSub"/>
    <div v-if="page == 'createSub'">
      <SUBSELECTION :foods="foods" :filterResults="filterResults" :handleSubCreate="handleSubCreate" :getSub="getSub"/>
    </div>
    <div v-if="page == 'createSub' || page == 'sub'" class="box" v-for="subFood in subFoods">
      <GETSUB :subFood="subFood"/>
      <DELETESUB :handleSubDelete="handleSubDelete" :subFood="subFood"/>
    </div>
    <div className="main-flex" v-if="page == 'main'">
      <h1 className="page-title">Master List</h1>
      <FILTER :filter="filter" :clearFilter="clearFilter" :search="search"/>
      <CREATESUB :createSub="createSub"/>
      <div v-if="view == 'filter'" class="filter-result" v-for="filterResult in filterResults">
        <div className="item-detail">
          <FILTERRESULTS :filterResult="filterResult"/>
        </div>
      </div>
      <div v-if="view == 'get'" class="box" v-for="food in foods">
        <GET :food="food"/>
        <EDIT :food="food" :handleEdit="handleEdit" :editItem="editItem"/>
        <DELETE :handleDelete="handleDelete" :food="food"/>
      </div>
      <POST :handleCreate = "handleCreate" :postNewItem = "postNewItem"/>
    </div>
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

.filter-result{
  display: flex;
  flex-wrap: wrap;
  width:400px;
}

.item-detail{
  margin-left: 110px;
}

</style>
