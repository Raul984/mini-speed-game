<template>
  <div v-if="gameOver" class="modal">
    <div class="modal-content">
      <h2>{{ winner }} выиграл!</h2>
      <button @click="resetGame" class="btn">Сбросить игру</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    gameOver: {
      type: Boolean,
      required: true,
    },
    winner: {
      type: String,
      required: true,
    },
  },
  methods: {
    resetGame() {
      this.$emit("reset-game");
    },
  },
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
}
</style>
