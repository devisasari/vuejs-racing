<script setup>
import { ref, watch } from 'vue'

const horse1Position = ref(-41)
const horse2Position = ref(-41)
const horse3Position = ref(-41)
const horse4Position = ref(-41)
const horse5Position = ref(-41)
const horse6Position = ref(-41)
const horse7Position = ref(-41)
const horse8Position = ref(-41)
const countdown = ref(0)
const leaderboard = ref([])
const results = ref([])
const horses = ref([
  { id: 1, name: 'Sugar (1)', position: 0 },
  { id: 2, name: 'Dakota (2)', position: 0 },
  { id: 3, name: 'Cisco (3)', position: 0 },
  { id: 4, name: 'Spirit (4)', position: 0 },
  { id: 5, name: 'Willow (5)', position: 0 },
  { id: 6, name: 'Dash (6)', position: 0 },
  { id: 7, name: 'Magic (7)', position: 0 },
  { id: 8, name: 'Star (8)', position: 0 },
])

const startGame = () => {
  countdown.value = 3
  const interval = setInterval(() => {
    countdown.value--
    if (countdown.value === 0) {
      clearInterval(interval)
      countdown.value = 0
      startRace()
    }
  }, 1000)
}

const stopGame = () => {
  countdown.value = 0
  horse1Position.value = -41
  horse2Position.value = -41
  horse3Position.value = -41
  horse4Position.value = -41
  horse5Position.value = -41
  horse6Position.value = -41
  horse7Position.value = -41
  horse8Position.value = -41
  leaderboard.value = []
  results.value = []
}

const startRace = () => {
  const interval = setInterval(() => {
    horse1Position.value += Math.floor(Math.random() * 21) + 20
    horse2Position.value += Math.floor(Math.random() * 21) + 20
    horse3Position.value += Math.floor(Math.random() * 21) + 20
    horse4Position.value += Math.floor(Math.random() * 21) + 20
    horse5Position.value += Math.floor(Math.random() * 21) + 20
    horse6Position.value += Math.floor(Math.random() * 21) + 20
    horse7Position.value += Math.floor(Math.random() * 21) + 20
    horse8Position.value += Math.floor(Math.random() * 21) + 20
    horses.value = [
      { id: 1, name: 'Sugar (1)', position: horse1Position.value },
      { id: 2, name: 'Dakota (2)', position: horse2Position.value },
      { id: 3, name: 'Cisco (3)', position: horse3Position.value },
      { id: 4, name: 'Spirit (4)', position: horse4Position.value },
      { id: 5, name: 'Willow (5)', position: horse5Position.value },
      { id: 6, name: 'Dash (6)', position: horse6Position.value },
      { id: 7, name: 'Magic (7)', position: horse7Position.value },
      { id: 8, name: 'Star (8)', position: horse8Position.value },
    ]
    if (horse1Position.value >= 700 && horse2Position.value >= 700 && horse3Position.value >= 700 && horse4Position.value >= 700 && horse5Position.value >= 700 && horse6Position.value >= 700 && horse7Position.value >= 700 && horse8Position.value >= 700) {
      clearInterval(interval)
      results.value = horses.value.sort((a, b) => b.position - a.position).map((horse, index) => ({ ...horse, rank: index + 1 }))
    }
  }, 250)
}

watch(horses, () => {
  leaderboard.value = horses.value.sort((a, b) => b.position - a.position).map((horse, index) => ({ ...horse, rank: index + 1 }))
})
</script>

<template>
  <div id="app">
    <div id="game">
      <div id="game-controls">
        <!-- when the click start button countdown is shown -->
        <button id="game-controls-start" v-on:click="startGame">Start</button>
        <div id="game-controls-countdown" v-if="countdown > 0">{{ countdown }}</div>
      </div>
      <div class="vertical-lines">
        <hr>
        <div id="horse-1" class="horse" v-bind:style="{ left: horse1Position + 'px' }"></div>
        <hr>
        <div id="horse-2" class="horse" v-bind:style="{ left: horse2Position + 'px' }"></div>
        <hr>
        <div id="horse-3" class="horse" v-bind:style="{ left: horse3Position + 'px' }"></div>
        <hr>
        <div id="horse-4" class="horse" v-bind:style="{ left: horse4Position + 'px' }"></div>
        <hr>
        <div id="horse-5" class="horse" v-bind:style="{ left: horse5Position + 'px' }"></div>
        <hr>
        <div id="horse-6" class="horse" v-bind:style="{ left: horse6Position + 'px' }"></div>
        <hr>
        <div id="horse-7" class="horse" v-bind:style="{ left: horse7Position + 'px' }"></div>
        <hr>
        <div id="horse-8" class="horse" v-bind:style="{ left: horse8Position + 'px' }"></div>
        <hr>
      </div>
    </div>
    <div id="game-leaderboard">
      <!-- Instantly updated leaderboard during the race -->
      <div id="game-leaderboard-title">
        <h3>Leaderboard</h3>
      </div>
      <div id="game-leaderboard-list">
        <div class="game-leaderboard-list-item" v-for="horse in leaderboard" v-bind:key="horse.id">
          <div class="game-leaderboard-list-item-rank">{{ horse.rank }}</div>
          <div class="game-leaderboard-list-item-name">{{ horse.name }}</div>
        </div>
      </div>
    </div>
    <!-- Show the results list list when the race is over -->
    <div id="game-results" v-if="results.length > 0">
      <div id="game-results-title">
        <h3>Results</h3>
      </div>
      <div id="game-results-list">
        <div class="game-results-list-item" v-for="horse in results" v-bind:key="horse.id">
          <div class="game-results-list-item-rank">{{ horse.rank }}</div>
          <div class="game-results-list-item-name">{{ horse.name }}</div>
        </div>
        <button id="stop-button" v-on:click="stopGame">Restart</button>
      </div>
    </div>
    <div id="footer">
      <a href="https://github.com/devisasari" target="_blank">2022 @ İsa Sarı</a>
    </div>
  </div>
</template>

<style scoped>
#game {
  position: relative;
  width: 700px;
  height: 100%;
  margin: 0 auto;
  background: rgb(0, 170, 0);
  border: 3px solid rgb(0, 0, 170);
  border-radius: 5px;
  overflow: hidden;
}

.vertical-lines {
  border-left: 42px solid rgb(85, 255, 255);
  border-right: 10px solid rgb(255, 85, 85);
  height: 480px;
}

#game-leaderboard {
  position: relative;
  top: -500px;
  left: 30px;
  width: 150px;
  height: 80%;
  background: rgb(255, 170, 0);
  border: 10px solid rgb(203, 131, 15);
  border-radius: 5px;
  overflow: hidden;
}

#game-leaderboard-title {
  width: 150px;
  height: 50px;
  border: 1px solid black;
}

#game-leaderboard-list {
  width: 200px;
  height: 418px;
  border: 1px solid black;
}

.game-leaderboard-list-item {
  width: 200px;
  height: 50px;
  border: 1px solid black;
  display: flex;
  align-items: center;
}

.game-leaderboard-list-item-rank {
  width: 50px;
  height: 50px;
  border: 1px solid black;
}

.game-leaderboard-list-item-name {
  width: 100px;
  height: 50px;
  border: 1px solid black;
}

#game-results {
  position: relative;
  top: -1040px;
  left: 1000px;
  width: 150px;
  height: 100%;
  background: rgb(85, 255, 85);
  border: 10px solid rgb(40, 218, 13);
  border-radius: 5px;
  overflow: visible;
}

#game-results-title {
  width: 150px;
  height: 50px;
  border: 1px solid black;
}

#game-results-list {
  width: 150px;
  height: 418px;
  border: 1px solid black;
}

.game-results-list-item {
  width: 150px;
  height: 50px;
  border: 1px solid black;
  display: flex;
  align-items: center;
}

.game-results-list-item-rank {
  width: 50px;
  height: 50px;
  border: 1px solid black;
}

.game-results-list-item-name {
  width: 100px;
  height: 50px;
  border: 1px solid black;
}

#stop-button {
  border-color: rgb(255, 117, 24);
  border-width: 6px;
  width: 100px;
  height: 50px;
  margin: 20px 0 0 10px;
}

#game-controls {
  width: 700px;
  height: 75px;
  border: 1px solid black;
  background-color: rgb(0, 0, 170)
}

#game-controls-start {
  position: relative;
  top: 12px;
  width: 100px;
  height: 50px;
  border: 1px solid black;
}

#game-controls-countdown {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 100px;
  color: #fff;
}

.horse {
  border-radius: 50%;
  width: 40px;
  height: 40px;
  background-color: red;
  position: relative;
  top: 0;
  left: 0;
  transition: all 0.5s ease;
}

#horse-1 {
  background-image: url('https://png.pngtree.com/png-vector/20191018/ourlarge/pngtree-cute-horse-avatar-with-a-yellow-background-png-image_1770352.jpg');
  background-size: cover;
}

#horse-2 {
  background-image: url('https://i.pinimg.com/736x/a2/2b/8c/a22b8c1724fea23a2f16f5affbfe8da4.jpg');
  background-size: cover;
}

#horse-3 {
  background-image: url('https://img.freepik.com/premium-vector/horse-with-blonde-mane-cute-animal-face-isolated-flat-cartoon-head-vector-funny-childish-mask-horse-carnival-costume-portrait-cute-comic-emoticon-emoji_53500-1030.jpg');
  background-size: cover;
}

#horse-4 {
  background-image: url('https://is1-ssl.mzstatic.com/image/thumb/Purple118/v4/64/75/c7/6475c7dd-bca3-4f66-262a-4b1f91f15d0f/source/256x256bb.jpg');
  background-size: cover;
}

#horse-5 {
  background-image: url('https://i.pinimg.com/474x/82/bb/25/82bb2516074894d6358430d044e1cc98.jpg');
  background-size: cover;
}

#horse-6 {
  background-image: url('https://howtodrawforkids.com/wp-content/uploads/2022/01/How-to-draw-a-Horse-Face-for-kids.jpg');
  background-size: cover;
}

#horse-7 {
  background-image: url('https://png.pngtree.com/png-vector/20220121/ourlarge/pngtree-cartoon-hand-drawn-cute-animal-horse-head-png-image_4280560.png');
  background-size: cover;
}

#horse-8 {
  background-image: url('https://img.freepik.com/premium-vector/horse-head-cartoon-mascot-character-illustration-isolated-white_20412-377.jpg?w=2000');
  background-size: cover;
}

#footer {
  width: 100%;
  height: 50px;
  border: 1px solid black;
  background-color: rgb(0, 0, 0)
}
</style>