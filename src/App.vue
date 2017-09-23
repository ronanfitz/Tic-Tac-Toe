<template>
  <div>
    <div id="app">
      <h1>Let's play Tic-Tac-Toe</h1>
    </div>
    <div class="scoreBoard">
      <span>X wins</span>
      <h2>Score Board</h2>
      <span>O wins</span>
    </div>
    <div class="score">
      <div class="left">
        <span>{{ wins.X }}</span>
      </div>
      <div class="right">
        <span>{{ wins.O }}</span>
      </div>
    </div>
    <div id="app">
      <board></board>
      <button class="restart" @click="restart">Restart</button>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import Board from './components/Board.vue'
export default {
  components: { Board },
  name: 'app',
  data() {
    return {
      matches: 0,
      wins: {
        O: 0,
        X: 0
      }
    }
  },
  methods: {
    restart() {
      Event.$emit('clearCell')
      Event.$emit('boardReset')
      this.matches++
    }
  },
  created() {
    Event.$on('win', winner => this.wins[winner]++)
  }
}
</script>

<style>
body {
  color: #fff;
  font-family: courier;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0px;
}
#app {
  margin: 30px auto;
  max-width: 350px;
  color: black;
}
h1 {
  text-transform: uppercase;
  font-weight: bold;
  font-size: 3em;
  font-family: courier;
}
.restart {
  background-color: teal;
  color: white;
  border: 0px;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  font-family: courier;
  font-size: 1.4em;
  font-weight: bold;
  margin: 30px 0;
  padding: 15px;
  width: 100%;
}
.restart:hover {
  background-color: turquoise;
  cursor: pointer;
}
.scoreBoard {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: 15px;
  background-color: teal;
  padding: 20px 0;
  overflow-x: none;
  margin: -10px;
}
.scoreBoard h2 {
  margin: 0px;
}
.scoreBoard span {
  float: right;
  font-size: 1.5em;
  font-weight: bold;
  margin-left: 20px;
}

.score {
  display: flex;
  width: 100%;
  height: 15px;
  background-color: teal;
  box-shadow: 10px solid #fff;
  padding: 20px 0;
  overflow-x: none;
}

.left {
  margin-left: 16%;
  float: left;
  font-size: 1.5em;
  font-weight: bold;
}

.right {
  margin-left: 67%;
  float: left;
  font-size: 1.5em;
  font-weight: bold;
}
</style>
