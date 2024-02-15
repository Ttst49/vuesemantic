<script setup lang="ts">
  import axios from "axios";
  import {GlobalConstants} from "@/Common/Global-constants";
  import {reactive, ref} from "vue";

  let wordToSend :string
  const responses = reactive([])

  function sendApiRequestToSemantic(word){
    if (!word){
      alert("Veuillez renseigner un mot")
    }else {
      console.log(word)
      axios.post(GlobalConstants.baseUrl, {"word":word})
          .then((response)=>{
            console.log(response.data.value)
            responses.push(word+" "+response.data.value+"%")
          })
    }

  }

</script>

<template>
  <input type="text" v-model="wordToSend">
  <button @click="sendApiRequestToSemantic(wordToSend)" type="submit">Checker la relation</button>
  <ul class="list">
    <li v-for="response in responses">{{response}}</li>
  </ul>

</template>

<style scoped lang="scss">
  .list{
    list-style: none;
  }
</style>