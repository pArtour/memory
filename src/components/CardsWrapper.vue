<template>
  <div class="container">
    <div class="turns">Turns :{{turns}}</div>
    <div class="totalTime"><button @click="reset">Restart</button></div>
    <div class="grid">
      <div v-for="(card, index) in memoryCards" :key="index + card.name" class="card-container" :class="{ 'flipped': card.flipped, 'matched' : card.matched }" @click="flipCard(card)">
        <div class="card">
            <div class="front"></div>
            <div class="back"><img :src="card.img"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {shuffle, cloneDeep} from 'lodash';
const INITIAL_CARDS = [
    {
        name: 'apple',
        img: require('@/assets/images/apple.png'),
        flipped: false,
        matched: false

    },
    {
        name: 'banana',
        img: require('@/assets/images/banana.png'),
        flipped: false,
        matched: false

    },
    {
        name: 'one',
        img: require('@/assets/images/one.png'),
        flipped: false,
        matched: false
    },
    {
        name: 'fly',
        img: require('@/assets/images/fly.png'),
        flipped: false,
        matched: false
    },
    {
        name: 'strawberry',
        img: require('@/assets/images/strawberry.png'),
        flipped: false,
        matched: false

    },
    {
        name: 'watermelon',
        img: require('@/assets/images/watermelon.png'),
        flipped: false,
        matched: false
    },
    {
        name: 'mango',
        img: require('@/assets/images/mango.png'),
        flipped: false,
        matched: false
    },
    {
        name: 'grapes',
        img: require('@/assets/images/grapes.png'),
        flipped: false,
        matched: false
    },
];
export default {
  name: 'Cards-Wrapper',
  data() {
    return {
      cards: INITIAL_CARDS,
      memoryCards: [],
      flippedCards: [],
      gameFinished: false,
      turns: 0,
    }
  },
 created() {
    this.reset();
 }, 
  methods: {
    flipCard(card){
      if (card.matched || card.flipped || this.flippedCards.length === 2) {
        return;
      }
      card.flipped = true;

      if (this.flippedCards.length < 2) {
          this.flippedCards.push(card);
      }
      if (this.flippedCards.length === 2) {
        this.match();  
      }
    },
    match() {
      this.turns++;
      if (this.flippedCards[0].name === this.flippedCards[1].name) {
        setTimeout(() => {
          this.flippedCards.forEach(card => card.mathced = true);
          this.flippedCards = [];

          if (this.memoryCards.every(card => card.mathced === true)) {
              this.gameFinished = true;
          }
        }, 400);
      } else {
        setTimeout(() => {
          this.flippedCards.forEach((card) => {card.flipped = false});
          this.flippedCards = [];
        }, 600);
      }
    },
    reset(){
      this.cards = [...cloneDeep(INITIAL_CARDS)];
      this.memoryCards = shuffle([...cloneDeep(this.cards), ...cloneDeep(this.cards)]);
      this.flippedCards = [];
      this.gameFinished = false;
      this.turns = 0;
    },
  }
}
</script>


<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 800px;
}

.grid {
  display: grid;
  width: 100%;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr;
  gap: 12px;
}

.card {
  position: relative;
}

.card-container {
  min-height: 120px;
  perspective: 1000;
  cursor: pointer;
}
.front,
.back {
    top: 0;
    left: 0;
    width: 100%;
    transition: 0.6s;
    backface-visibility: hidden;
    transform-style: preserve-3d;
}
.back {
  position: absolute;
  transform: rotateY(-180deg);
}

.front {
  height: 200px;
  cursor: pointer;
  background-color: rgb(15, 132, 39);
}

img {
  width: 100%;
  max-width: 100%;
  height: 190px;
}

.card-container.flipped .back {
    transform: rotateY(0deg);
}
.card-container.flipped .front {
    transform: rotateY(180deg);
}

.totalTime {
  margin-bottom: 10px;
}
</style>
