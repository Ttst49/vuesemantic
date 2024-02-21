<script setup lang="ts">
  import axios from "axios";
  import {GlobalConstants} from "@/Common/Global-constants";
  import { reactive} from "vue";

  let wordToSend :string
  const responses = reactive([])
  let hasWin: boolean = false



  function sendApiRequestToSemantic(word) {
    console.log(responses.sort())
    console.log(responses)
    console.log(word)

    if (responses.includes(word)){
      alert("Vous avez déjà entré ce mot")
      return true
    }
    
      if (!word) {
            alert("Veuillez renseigner un mot")
          } else {
            console.log(word)
            axios.post(GlobalConstants.baseUrl, {"word": word})
                .then((response) => {
                  console.log(response.data.value)
                  responses.push(response.data.value + "% " + word)
                  if (response.data.value === 100) {
                    console.log('gagné')
                    hasWin = true
                    setTimeout(()=>{responses.sort().reverse()},500)
                  }
                })
          }
      responses.sort().reverse()
  }

</script>

<template>


  <div class="interface">
    <div class="leftPanel">

    </div>

    <h1>cémantix</h1>
    <div class="card">

    </div>
    <div v-show="hasWin" class="status">Gagné</div>
    <input type="text" ref="input" v-model="wordToSend" v-if="!hasWin">
    <button v-if="!hasWin" @click="sendApiRequestToSemantic(wordToSend);" type="submit" class="button">Checker la relation</button>
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
    padding: 0;
  }

  .arrayOfResponses{
    margin: 10vh;
    width: 20vw;
    min-height: 5vh;
    border: 1px solid black;
  }

  .button {
    margin: 15px;
    padding: 15px 25px;
    border: unset;
    border-radius: 15px;
    color: #212121;
    z-index: 1;
    background: #e8e8e8;
    position: relative;
    font-weight: 1000;
    font-size: 17px;
    -webkit-box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
    box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
    transition: all 250ms;
    overflow: hidden;
  }

  .button::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 0;
    border-radius: 15px;
    background-color: #212121;
    z-index: -1;
    -webkit-box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
    box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
    transition: all 250ms
  }

  .button:hover {
    color: #e8e8e8;
    cursor: pointer;
  }

  .button:hover::before {
    width: 100%;
  }

  .leftPanel{
    height: 200vh;
    width: 20vw;
    position: fixed;
    left: 0;
    background-color: #212121;
  }
</style>