<script setup lang="ts">
  import axios from "axios";
  import {GlobalConstants} from "@/Common/Global-constants";
  import {reactive} from "vue";

  let wordToSend :string
  const responses = reactive([])
  let hasWin: boolean = false

  function sendApiRequestToSemantic(word){
    if (!word){
      alert("Veuillez renseigner un mot")
    }else {
      console.log(word)
      axios.post(GlobalConstants.baseUrl, {"word":word})
          .then((response)=>{
            console.log(response.data.value)
            responses.push(word+" "+response.data.value+"%")
            if (response.data.value === 100){
              console.log('gagné')
              hasWin = true
            }
          })
    }

  }

</script>

<template>


  <div class="interface">
    <h1>cémantix</h1>
    <div class="card">

    </div>
    <div v-show="hasWin" class="status">Gagné</div>
    <input type="text" v-model="wordToSend" v-if="!hasWin">
    <button v-if="!hasWin" @click="sendApiRequestToSemantic(wordToSend)" type="submit">Checker la relation</button>
    <div class="arrayOfResponses">
      <ul class="list">
        <li v-for="response in responses">{{response}}</li>
      </ul>
    </div>
  </div>

</template>

<style scoped lang="scss">
  .interface {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .list{
    list-style: none;
  }

  .arrayOfResponses{
    margin: 10vh;
    width: 20vw;
    min-height: 5vh;
    border: 1px solid black;
  }
</style>