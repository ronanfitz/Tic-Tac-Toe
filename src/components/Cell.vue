<template>
  <td class="cell" @click="play">{{ mark }}</td>
</template>

<script>
/* eslint-disable */
  export default {
    props: ['name'],
    data() {
      return {
        stopped: false,
        mark: '',
      }
    },
    methods: {
      play() {
        if (! this.stopped) {
          this.mark = this.$parent.activePlayer;
          this.stopped = true;
          Event.$emit('play', this.name);
        }
      }
    },
    created() {
      Event.$on('clearCell', () => {
        this.mark = '';
        this.stopped = false;
      })
      Event.$on('stop', () => this.stopped = true);
    }
  }
</script>

<style>
.cell {
  width: 33.333%;
  height: 90px;
  border: 6px solid black;
  font-size: 3.5em;
  font-family: courier;
  color: teal
}
.cell:hover {
  background-color: grey;
}
.cell::after {
  content: '';
  display: block;
}
.cell:first-of-type {
  border-left-color: transparent;
  border-top-color: transparent;
}
.cell:nth-of-type(2) {
  border-top-color: transparent;
}
.cell:nth-of-type(3) {
  border-right-color: transparent;
  border-top-color: transparent;
}
tr:nth-of-type(3) .cell {
  border-bottom-color: transparent;
}
</style>
