<script>
export default{
  props:["handleCreate", "postNewItem", "menu"],
  data(){
    return{

    }
  }
}
</script>

<template>
  <div v-if="menu == true" className="post">
    <form class="hello" @submit.prevent="handleCreate">
      <input v-model="postNewItem.name" type="text" placeholder="Name"><br/>
      <input v-model="postNewItem.category" type="text" placeholder="Category"><br/>
      <input v-model="postNewItem.aisle" type="number" placeholder="Aisle"><br/>
      <input v-model="postNewItem.listname" type="text" placeholder="ListName"><br/>
      <button className="button-design" type="submit">Submit</button>
    </form>
  </div>
</template>

<style>
  .post{
    display: flex;
    width:100%;
    justify-content: center;
    margin-bottom: 10px;
  }

  .post button{
    margin-top: 15px;
  }
</style>
