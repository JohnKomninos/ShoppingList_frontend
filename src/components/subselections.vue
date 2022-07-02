<script>
import {ref} from 'vue'

export default{
  data(){
    return{
      index : 0 ,
      quantity : false,
      itemQuantity : 1
    }
  },
  props:["foods", "filterResults" , "handleSubCreate", "getSub"],
  methods:{
    increaseIndex () {
      this.index++
      if(this.index > this.filterResults.length-1){
        this.getSub()
        this.index = 0
      }
    } ,
    displayQuantity () {
      if(this.quantity == false){
          this.quantity = true
      } else {
        this.quantity = false
      }
    } ,
    submitAndChangeView(index, itemQuantity){
      this.handleSubCreate(index, itemQuantity)
      this.displayQuantity()
      this.increaseIndex()
      this.itemQuantity = 1
    }
  }
}
</script>

<template>
  Name:{{filterResults[index].name}}<br/>
  Category:{{filterResults[index].category}}<br/>
  <div v-if="quantity == false">
    Do you need this item?
    <button @click="displayQuantity">Yes</button>
    <button @click="increaseIndex()">No</button><br/>
  </div>
  <form @submit.prevent="submitAndChangeView(index, itemQuantity)" v-if="quantity">
    How many?
    <input type="number" v-model="itemQuantity"/>
    <input type="submit"/>
  </form>
</template>
