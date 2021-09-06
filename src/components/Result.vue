<template>
  <div class="result">
        <div class="title">You got {{correctAnswered}} {{resultTitle}} !</div>
        <div class="desc">
            {{resultDesc}}
        </div>
  </div>
</template>


<script lang="ts">
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
  name:"Result",
  props:{
    resultTitle:{type:String,required:true},
    resultDesc:{type:String,required:true},
    correctAnswered:{type:Number,required:true}
  }
})
</script>