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
  <details>
    <form @submit.prevent="handleEdit(food.id)">
      <input v-model="editItem.name" type="text"><br/>
      <input v-model="editItem.category" type="text"><br/>
      <input v-model="editItem.aisle" type="number"><br/>
      <button type="submit">Submit</button>
    </form>
</details>
</template>
