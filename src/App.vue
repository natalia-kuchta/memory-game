<script setup>
import {reactive, ref, onMounted} from 'vue'
import foxImg from './assets/fox.jpg';
import leoImg from './assets/leo.jpg';
import wolfImg from './assets/wolf.jpg';
import turtleImg from './assets/turtle.jpg';
import zebraImg from './assets/zebra.jpg';
import capyImg from './assets/capy.jpg'
import birdImg from './assets/bird.jpg'
import frogImg from './assets/frog.jpg'
import igunaImg from './assets/iguna.jpg'
import tigerImg from './assets/tiger.jpg'
import pumaImg from './assets/puma.jpg'
import elephantImg from './assets/elephant.jpg'
import monkeyImg from './assets/monkey.jpg'
import kangoroImg from './assets/kangoro.jpg'
import bearImg from './assets/bear.jpg'
import suricataImg from './assets/suricata.jpg'

function loadImages() {
  return [
    foxImg,
    leoImg,
    wolfImg,
    turtleImg,
    zebraImg,
    capyImg,
    birdImg,
    frogImg,
    igunaImg,
    tigerImg,
    pumaImg,
    elephantImg,
    monkeyImg,
    kangoroImg,
    bearImg,
    suricataImg


  ]
}

const images = loadImages();

function defaultCards() {
  return [...new Array(images.length * 2)].map((c, index) => ({
    revealed: false,
    matched: false,
    value: Math.floor(index / 2),
    img: images[Math.floor(index / 2)],
  }))
}

const cards = reactive(defaultCards());

const moves = ref(0)
const gameOver = ref(false)
const revealedCards = ref([]);

function shuffle(array){
  for(let i=array.length -1 ; i>0; i-- ){
    const j= Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];

  }
  return array
}

function setupCards() {
  const shuffledCards = shuffle(defaultCards());
  cards.splice(0, cards.length, ...shuffledCards);
}

async function startGame() {
  moves.value = 0;
  gameOver.value = false;
  revealedCards.value = [];
  setupCards();
}



function handleRevealCard(index){
  const card= cards[index]
  if(card.revealed || card.matched) return;

  card.revealed= true
  revealedCards.value.push(card);

  if(revealedCards.value.length === 2){

    const[firstCard, secondCard]=revealedCards.value;


    if(firstCard.value === secondCard.value){
      firstCard.matched=true;
      secondCard.matched=true;
      checkGameOver();
      moves.value += 2
    }else{
      setTimeout(()=>{
        firstCard.revealed=false
        secondCard.revealed=false
      }, 1000)

    }
    revealedCards.value = [];

  }

}
onMounted(startGame);

function checkGameOver(){
  gameOver.value=cards.every(card => card.matched);
}

// col 8 8 rem     ->  64.00 rem
// gaps 7 0.75 rem ->   5.25 rem
// width           ->  69.25 rem

</script>

<template>
  <div class="memory-game bg-gradient-to-r from-green-400 via-green-500 to-green-600 w-full min-h-screen">
    <h1 class="text-4xl text-center text-violet-700 font-bold p-3 text-outline">Memory Game</h1>
    <p class="text-xl text-center font-bold p-3 text-amber-50">Your score: {{ moves }}</p>
    <div class="grid grid-cols-8 gap-3 mt-6 w-[69.25rem] m-auto">
      <div
          v-for="(card, index) in cards"
          :key="index"
          @click="handleRevealCard(index)"
          class="card flex items-center justify-center mx-auto w-32 h-24 bg-gradient-to-r from-violet-600 via-violet-500 rounded-lg shadow-lg transition-all duration-300 ease-in-out transform cursor-pointer"
          :class="card.revealed || card.matched ? 'bg-gray-500 scale-105' : 'text-transparent bg-violet-700'"
      >

        <img
            v-if="card.revealed || card.matched"
            :src="card.img"
            class="w-full h-full object-cover rounded-md cursor-pointer hover:scale-110 hover:border-2 hover:border-gray-700"
            alt="Card Image"

        />

      </div>
    </div>

   <div class="flex items-center justify-center w-full mt-6">
    <button
        v-if="gameOver"
        @click="startGame"
        class="bg-gradient-to-r from-violet-400 via-violet-500 to-violet-600 text-amber-50 text-outline text-lg font-semibold rounded-lg shadow-xl hover:shadow-xl transform transition-all duration-300 p-4 mt-4 active:scale-95 cursor-pointer hover:scale-110 border-4 border-violet-700"
    >

      🎉 Restart Game 🎉
    </button>
   </div>
  </div>

</template>

<style scoped>
.card {
  transition: background-color 0.3s, color 0.3s;
}
.text-outline {
  text-shadow:
      1px 1px 1px #4c1d95,   /* fioletowe obramowanie */
      -1px 1px 1px #4c1d95,
      1px -1px 1px #4c1d95,
      -1px -1px 1px #4c1d95,
      2px 2px 1px #4c1d95,
      -2px 2px 1px #4c1d95,
      2px -2px 1px #4c1d95,
      -2px -2px 1px #4c1d95,
      0 0 6px #ffffff;
}

</style>


