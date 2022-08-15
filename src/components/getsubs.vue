<script>
export default{
  props:["subFood", "handleSubEdit"],
  methods:{
    editQuantity(){
      if(this.edit == true){
          this.edit = false
      } else {
      this.edit = true
      }
    },
    submitAndChangeView(subFood, subQuantity){
      this.handleSubEdit(subFood, subQuantity)
      this.editQuantity()
    }
  },
  data(){
    return{
      edit : false,
      subQuantity : this.subFood.quantity
    }
  }
}
</script>

<template>
  <td>
    {{subFood.name}}
  </td>
  <td>
    {{subFood.category}}
  </td>
  <td>
    {{subFood.aisle}}
  </td>
  <td>
    <div v-if="edit == false">
      {{subFood.quantity}}<br/>
      <button @click="editQuantity">Edit</button>
    </div>
    <div v-if="edit == true">
    <form @submit.prevent="submitAndChangeView(subFood, subQuantity)">
      <input type="number" v-model="subQuantity"/>
      <input className="button-design" type="submit"/>
      <button @click="editQuantity" className="button-design" >Cancel</button>
    </form>
    </div>
  </td>
</template>
