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
  <h1 className="selection">#{{index+1}}. {{filterResults[index].name}}</h1>
  <div v-if="quantity == false">
    <h3>Do you need {{filterResults[index].name}} ?
      <button className="button-design" @click="displayQuantity">Yes</button>
      <button className="button-design" @click="increaseIndex()">No</button><br/>
    </h3>
  </div>
  <form @submit.prevent="submitAndChangeView(index, itemQuantity)" v-if="quantity">
    <h3>How many?
      <input type="number" v-model="itemQuantity"/>
      <input className="button-design" type="submit"/>
    </h3>
  </form>
</template>

<style>
  .selection{
      text-align: center;
  }
</style>
