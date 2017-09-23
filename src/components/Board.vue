<template>
  <div>
    <!-- <vue-toastr ref="toastr"></vue-toastr> -->
    <table class="board">
      <tr>
        <cell name="1"></cell>
        <cell name="2"></cell>
        <cell name="3"></cell>
      </tr>
      <tr>
        <cell name="4"></cell>
        <cell name="5"></cell>
        <cell name="6"></cell>
      </tr>
      <tr>
        <cell name="7"></cell>
        <cell name="8"></cell>
        <cell name="9"></cell>
      </tr>
    </table>
  </div>
</template>

<script>
/* eslint-disable */
import Cell from './Cell.vue'
// window.toastr = require('vue-toastr');
export default {
  components: { Cell },
  data() {
    return {
      activePlayer: 'X',
      gameStatus: 'turn',
      gameStatusColor: 'statusTurn',
      moves: 0,
      cells: {
        1: '', 2: '', 3: '',
        4: '', 5: '', 6: '',
        7: '', 8: '', 9: '',
      },
      winConditions: [
        [1, 2, 3], [4, 5, 6], [7, 8, 9],
        [1, 4, 7], [2, 5, 8], [3, 6, 9],
        [1, 5, 9], [3, 5, 7],
      ],
    }
  },
  computed: {
    otherPlayer() {
      if (this.activePlayer === 'O') {
        return 'X';
      }
      return 'O';
    }
  },
  // watch: {
  //   gameStatus() {
  //     if (this.gameStatus === 'win') {
  //       // this.$refs.toastr.s("SUCCESS MESSAGE");
  //       return
  //     } else if (this.gameStatus === 'draw') {
  //       this.gameStatusMessage = 'Draw !';
  //       return
  //     }
  //   }
  // },
  methods: {
    changePlayer() {
      this.activePlayer = this.otherPlayer;
    },
    checkForWin() {
      for (let i = 0; i < this.winConditions.length; i++) {
        let win = this.winConditions[i];
        let cells = this.cells;
        if (this.areEqual(cells[win[0]], cells[win[1]], cells[win[2]])) {
          return true;
        }
      }
      return false;
    },
    gameIsWon() {
      Event.$emit('win', this.activePlayer);
      Event.$emit('stop');
      return 'win'
    },
    changeGameStatus() {
      if (this.checkForWin()) {
        return this.gameIsWon();
      } else if (this.moves === 9) {
        return 'draw';
      }
      return 'turn';
    },
    areEqual() {
      var len = arguments.length;
      for (var i = 1; i < len; i++){
        if (arguments[i] === '' || arguments[i] !== arguments[i-1])
          return false;
       }
       return true;
    }
  },
  created() {
    Event.$on('play', (cellNumber) => {
      this.cells[cellNumber] = this.activePlayer;
      this.moves++;
      this.gameStatus = this.changeGameStatus();
      this.changePlayer();
    })
    Event.$on('boardReset', () => {
      Object.assign(this.$data, this.$options.data());
    })
  }
}
</script>

<style>
.board {
  width: 100%;
  border-collapse: collapse;
}
</style>
