<script>
export default{
  props:["handleEdit", "food", "editItem", "edit", "setID"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <td>
      <button v-if="food.id != edit" @click="setID(food.id)">E</button>
      <button v-if="food.id == edit" @click="setID(food.id)">C</button>
      <form v-if="food.id == edit" @submit.prevent="handleEdit(food)">
        <input v-model="editItem.name" type="text" placeholder="Name" ><br/>
        <input v-model="editItem.category" type="text" placeholder="Category"><br/>
        <input v-model="editItem.aisle" type="number" placeholder="Aisle"><br/>
        <input v-model="editItem.listname" type="text" placeholder="List Name"><br/>
        <button type="submit">Submit</button>
      </form>
  </td>
</template>

<style>
</style>
