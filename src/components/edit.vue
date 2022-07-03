<script>
export default{
  props:["handleEdit", "food", "editItem"],
  data(){
    return{
      
    }
  }
}
</script>

<template>
  <td>
    <details>
      <summary></summary>
      <form @submit.prevent="handleEdit(food.id)">
        <input v-model="editItem.name" type="text" placeholder="Name"><br/>
        <input v-model="editItem.category" type="text" placeholder="Category"><br/>
        <input v-model="editItem.aisle" type="number" placeholder="Aisle"><br/>
        <input v-model="editItem.listname" type="text" placeholder="List Name"><br/>
        <button type="submit">Submit</button>
      </form>
    </details>
  </td>
</template>

<style>
</style>
