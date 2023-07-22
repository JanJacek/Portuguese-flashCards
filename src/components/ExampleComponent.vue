<template>
  <div>
    <div class="column">
      <div
        @mouseenter="flipCard"
        @mouseleave="flipCardBack"
        class="card cardDimensions"
        :class="{ flipped: isFlipped }"
      >
        <q-card class="front bg-grey-2">
          <q-img class="cardDimensions" :src="img">
            <div class="absolute-bottom text-subtitle2 text-center">
              <p class="text-h3">{{ frontCard }}</p>
            </div>
          </q-img>
        </q-card>
        <q-card class="back bg-teal-2 row content-center justify-around">
          <q-card-section>
            <p class="text-h3">{{ backCard }}</p>
          </q-card-section>
        </q-card>
      </div>
    </div>
    <div class="row justify-center q-mt-md">
      <q-btn flat label="prev" @click="prevCard()"></q-btn>
      <q-btn flat label="next" @click="nextCard()"></q-btn>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { words } from 'src/utils/words';

const isFlipped = ref(false);
function flipCard() {
  isFlipped.value = !isFlipped.value;
}
function flipCardBack() {
  isFlipped.value = !isFlipped.value;
}

const frontCard = ref();
const backCard = ref();
const img = ref();

function nextCard() {
  const card = getRandomElementFromObject(words);
  frontCard.value = card.port;
  backCard.value = card.pl;
  img.value = card.img ?? '';
}

function prevCard() {
  const card = getRandomElementFromObject(words);
  console.log(card);
  frontCard.value = card.port;
  backCard.value = card.pl;
  img.value = card.img ?? '';
}

function getRandomElementFromObject(
  obj: Record<string, { pl: string; port: string; img?: string }>
) {
  const keys = Object.keys(obj);
  const randomKey = keys[Math.floor(Math.random() * keys.length)];
  return obj[randomKey];
}
</script>

<style>
.card {
  position: relative;
  perspective: 1000px;
}
.cardDimensions {
  width: 550px;
  height: 360px;
}

.front,
.back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  transition: transform 0.5s;
}

.back {
  transform: rotateY(180deg);
}

.flipped .front {
  transform: rotateY(180deg);
}

.flipped .back {
  transform: rotateY(0deg);
}
</style>
