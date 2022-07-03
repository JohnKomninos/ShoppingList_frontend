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
        <input v-model="editItem.name" type="text"><br/>
        <input v-model="editItem.category" type="text"><br/>
        <input v-model="editItem.aisle" type="number"><br/>
        <button type="submit">Submit</button>
      </form>
    </details>
  </td>
</template>
