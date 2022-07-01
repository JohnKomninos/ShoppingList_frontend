<script>
export default{
  props:["handleCreate", "postNewItem"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <form @submit.prevent="handleCreate">
    <input v-model="postNewItem.name" type="text" placeholder="Name"><br/>
    <input v-model="postNewItem.category" type="text" placeholder="Category"><br/>
    <input v-model="postNewItem.aisle" type="number" placeholder="Aisle"><br/>
    <input v-model="postNewItem.listname" type="text" placeholder="ListName"><br/>
    <button type="submit">Submit</button>
  </form>
</template>
