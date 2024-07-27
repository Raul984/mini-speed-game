<template>
  <div id="game">
    <div class="game-container">
      <ControlPanel
        :playerScore="playerScore"
        :computerScore="computerScore"
        :timeLimit="timeLimit"
        @start-game="startGame"
      />
      <div class="grid">
        <Cell
          v-for="(cell, index) in cells"
          :key="index"
          :color="cell.color"
          @cell-click="cellClicked(index)"
        />
      </div>
    </div>
    <Modal :gameOver="gameOver" :winner="winner" @reset-game="resetGame" />
  </div>
</template>

<script>
import ControlPanel from "./components/ControlPanel.vue";
import Cell from "./components/Cell.vue";
import Modal from "./components/Modal.vue";

export default {
  components: {
    ControlPanel,
    Cell,
    Modal,
  },
  data() {
    return {
      cells: Array(100)
        .fill()
        .map(() => ({ color: "blue" })),
      playerScore: 0,
      computerScore: 0,
      timeLimit: 1000,
      gameOver: false,
      currentYellowCell: null,
      timer: null,
      winner: "",
    };
  },
  methods: {
    startGame(newTimeLimit) {
      this.timeLimit = newTimeLimit;
      this.resetCells();
      this.nextTurn();
    },
    nextTurn() {
      if (this.playerScore >= 10 || this.computerScore >= 10) {
        this.endGame();
        return;
      }
      const randomIndex = Math.floor(Math.random() * this.cells.length);
      this.currentYellowCell = randomIndex;
      this.cells[randomIndex].color = "yellow";

      this.timer = setTimeout(() => {
        if (this.cells[randomIndex].color === "yellow") {
          this.cells[randomIndex].color = "red";
          this.computerScore++;
          this.nextTurn();
        }
      }, this.timeLimit);
    },
    cellClicked(index) {
      if (this.cells[index].color === "yellow") {
        clearTimeout(this.timer);
        this.cells[index].color = "green";
        this.playerScore++;
        this.nextTurn();
      }
    },
    resetCells() {
      this.cells = Array(100)
        .fill()
        .map(() => ({ color: "blue" }));
      this.currentYellowCell = null;
    },
    resetGame() {
      this.playerScore = 0;
      this.computerScore = 0;
      this.gameOver = false;
      this.resetCells();
      clearTimeout(this.timer);
    },
    endGame() {
      this.gameOver = true;
      this.winner =
        this.playerScore > this.computerScore ? "Игрок" : "Компьютер";
    },
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background-color: #f0f0f0;
}

.game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.grid {
  display: grid;
  grid-template-columns: repeat(10, 30px);
  grid-gap: 5px;
}
</style>
