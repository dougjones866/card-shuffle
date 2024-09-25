<!-- Use preprocessors via the lang attribute! e.g. <template lang="pug"> -->
<template>
  <div id="app">
    <div class="count-section"># of Shuffles: {{ shuffleCount }}</div>
    <h1 class="title">

      Card Shuffling
    </h1> <div class="speed-buttons">
    <button v-for="type in shuffleTypes" @click="shuffleSpeed = `shuffle${type}`">
      {{ type }}
    </button>
  </div>
  <div class="main-buttons">
    <button v-if="isDeckShuffled" @click="displayInitialDeck">
      Reset
      <i class="fas fa-undo"></i>
    </button>
    <button @click="shuffleDeck">
      Shuffle
      <i class="fas fa-random"></i>
    </button>
  </div>
  <transition-group :name="shuffleSpeed" tag="div" class="deck">
    <div v-for="card in cards" :key="card.id" class="card" :class="suitColor[card.suit]">
      <span class="card__suit card__suit--top">{{ card.suit }}</span>
      <span class="card__number">{{ card.rank }}</span>
      <span class="card__suit card__suit--bottom">{{ card.suit }}</span>
    </div>
  </transition-group>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const ranks = ref([
  "A",
  "2",
  "3",
  "4",
  "5",
  "6",
  "7",
  "8",
  "9",
  "10",
  "J",
  "Q",
  "K"
]);
const suits = ref(["♥", "♦", "♠", "♣"]);
const cards = ref([]);
const suitColor = {
  "♠": "black",
  "♣": "black",
  "♦": "red",
  "♥": "red"
};
const shuffleSpeed = ref("shuffleMedium");
const shuffleTypes = ["Slow", "Medium", "Fast"];
const isDeckShuffled = ref(false);
const shuffleCount = ref(0);

const displayInitialDeck = () => {
  let id = 1;
  cards.value = [];

  for (let s = 0; s < suits.value.length; s++) {
    for (let r = 0; r < ranks.value.length; r++) {
      let card = {
        id: id,
        rank: ranks.value[r],
        suit: suits.value[s]
      };
      cards.value.push(card);
      id++;
    }
  }

  isDeckShuffled.value = false;
  shuffleCount.value = 0;
};

const shuffleDeck = () => {
  for (let i = cards.value.length - 1; i > 0; i--) {
    let randomIndex = Math.floor(Math.random() * i);

    let temp = cards.value[i];
    cards.value[i] = cards.value[randomIndex];
    cards.value[randomIndex] = temp;
  }

  isDeckShuffled.value = true;
  shuffleCount.value = shuffleCount.value + 1;
};

onMounted(() => {
  displayInitialDeck();
});
</script>

<style scoped>


.title {
  font-family: Roboto Slab, sans-serif;
  text-align: center;
  padding-top: 30px;
  margin-bottom: 0 !important;
  font-weight: 300;
  font-size: 3rem;
}

button {
  background: #0faf87;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin: 5px;
}

.speed-buttons {
  text-align: center;
  padding-top: 30px;

}

button:hover {
  background-color: red;
}

.speed-buttons .button {
  height: 2.5em;
}

.main-buttons {
  display: block;
  margin: 0 auto;
  text-align: center;
  padding-top: 30px;
  margin-bottom: 0 !important;
}

.count-section {
  position: absolute;
  top: 10px;
  right: 10px;
}

.fas {
  padding-left: 5px;
}

.deck {
  margin-left: 30px;
  padding-top: 30px;
}

.card {
  width: 75px;
  height: 100px;
  float: left;
  margin-right: 5px;
  margin-bottom: 5px;
  border-radius: 3px;
  border: solid thin;
  background-color: white;
  box-shadow: 10px 10px 8px -8px rgba(0, 0, 0, 0.2);
}

.card:hover {
  transform: scale(1.1);
}

.card__suit {
  width: 100%;
  display: block;
}

.card__suit--top {
  text-align: left;
  padding-left: 5px;
}

.card__suit--bottom {
  position: relative;
  top: 50px;
  text-align: left;
  transform: rotate(180deg);

}

.card__number {
  width: 100%;
  position: relative;
  left: 40%;
  top: 30%;
  text-align: center;

}

.red {
  color: #ff0000;
}

.black {
  color: #000;
}

.twitter-section {
  position: absolute;
  bottom: 10px;
  right: 10px;
}

.twitter-section .fa-twitter-square {
  color: #00d1b2;
  font-size: 30px;
}

.shuffleSlow-move {
  transition: transform 2s;
}

.shuffleMedium-move {
  transition: transform 1s;
}

.shuffleFast-move {
  transition: transform 0.5s;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

@media (max-width: 1210px) {
  .deck {
    position: initial;
    top: 0;
  }

  .twitter-section {
    display: none;
  }
}
</style>